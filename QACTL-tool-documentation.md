# QACTL Documentation

To launch a test and get its reports, it is needed to prepare the proper environment and provisioning it with the necessary tools for running the tests. It takes some time to complete all the steps before being able to launch the tests and we want the person that is going to run some tests to avoid spending the time preparing the whole testing environment.

That is the main purpose of the **qa-ctl** tool.

This new tool has been developed to automatize all the preparing testing environment so the test user doesn't need to concern about it, all the user needs to do now for preparing the environment, launch the desired tests and get its results is to run a single command.

All the processes automatized by this tool are:
- Create the test environment. There are several types available here:
  - Virtual Machine
  - Docker container
- Install Wazuh and the Wazuh QA framework.
- Run the desired tests and get the results.

The next image shows a flow diagram of how the tool works:

<p align="center">
<img width="576px" "height="538px" src="https://github.com/wazuh/wazuh-qa/wiki/images/qactl_tool_imgs/diagram.png">
</p>

- The infrastructure module is in charge of creating the needed VMs, containers, or AWS instances.
- The provisioning module is in charge of installing Wazuh and the QA framework. This module also installs all the dependencies to run the tests.
- The run test module is responsible for launching the tests and collect the results.


The user will interact with each module using the `qa-ctl` tool. This tool takes a configuration file in YAML format with the information needed by each module.

## System requirements

There are some requirements needed to get this tool working properly
- `Linux Host` 
- `Python`
- `Docker`
- `Vagrant` (>=2.2.6)
- `VirtualBox` (>=6.0.18)
- `Ansible`
- `sshpass` (additional)

The tool will also need the following python libraries installed:

- `ansible-runner`==2.0.1
- `docker`==5.0.0
- `filetype`==1.0.7
- `lockfile`==0.12.2
- `pathlib`==1.0.1
- `python-vagrant`==0.5.15
- `PyYAML`==5.4.1

## Configuration setup

We have specified before the three main different modules available and running in this tool. To interact with each module and set up a configuration for each one of them, there will be needed a configuration file in the YAML format. This yaml can 4 different sections:
- `deployment`: Section dedicated to creating the desired environment. These environments can be created either with vagrant or docker.
- `provision`: Section dedicated to installing Wazuh (agent or manager), the Wazuh QA framework, and all the possible dependencies needed.
- `tests`: Section dedicated to run all the desired tests configured by the user and get its parameters.
- `config`: This additional section is used for creating custom terminal outputs in order to understand better every step that the script goes through every time is running.

All of these sections have different configurable fields, some required, some optional, and declared by default if not given. In the next points of this documentation, every field will be explained so the user can understand better what the field is asking for or if he can skip its declaration

## Infrastructure deployment

The deployment module can create virtual machines using vagrant or docker containers using docker. To accomplish it, this module will take the information from the configuration file and will create the Vagrantfile using that information or will use an existing Dockerfile (specified in the configuration file) to build an image that will be used to run the containers.

The configuration section for the infrastructure module must follow the next structure:

```yaml
deployment:
    host1:
      provider:
        vagrant:
          enabled: True/False (Required)
          vagrantfile_path: String (Required)
          vagrant_box: String (Required)
          vm_memory: Number(int) (Required)
          vm_cpu: Number(int) (Required)
          vm_name: String (Required)
          vm_system: String (Required)
          label: String (Required)
          vm_ip: IP address (Required)

     host2:
        provider:
          docker:
            enabled: True/False (Required)
            dockerfile_path: String (Required)
            name: String (Required)
            ip: String (Optional) Default: None
            remove: True/False (Optional) default: False
            ports: Dict (optional) default: Empty
            detach: True/False (Optional) default: True
            stdout: True/False (Optional) default: False
            stderr: True/False (Optional) default: False
```

## Infrastructure deployment module fields information

### Available fields using Vagrant as the provider

- `enabled`: If False, the VM won’t be created.
- `vagrantfile_path`: Path where the vagrantfile will be created.
- `vagrant_box`: Path to the vagrant box file, link, or box specifying the box that will be used.
- `vm_memory`: Memory assigned to the VM.
- `vm_cpu`: Number of CPUs assigned to the VM.
- `vm_system`:  VM operative system.
- `Label`: Assign a label to the VM.
- `vm_ip`: assigned IP for the VM

### Available fields using Docker as the provider

- `enabled`: If False, the VM won’t be created.
- `dockerfile_path`: Path to an existing dockerfile.
- `name`: Name assigned to the container.
- `ip`: Assign a static IP to the container. To do this, we need to create a docker network based on the specified network (to not create as many networks as containers specified in the dockerfile, only one network is allowed). The mask of the network is `/24`. If this option is not specified, the container won't have a static IP
- `remove`: Remove the container. Defaults to False.
- `Ports`: Ports to bind to the container. This field is a python dictionary where the keys are the ports to bind inside the container in the form port/protocol, and the values are integers corresponding to the ports we want to open in the host.
- `detach`: Run the container in the background. Defaults to True.
- `stdout`: Return logs from STDOUT when detach=False.
- `Stderr`: Return logs from STDERR when detach=False.

## Provisioning

The provisioning module takes care of the complete installation of Wazuh and the QA framework downloading the necessary files according to the type of installation configured. The structure of this section is:

```yaml
provision:
  hosts:
    host1:
      host_info:
        connection_method: String (Required)
        host: String (Required)
        user: String (Required)
        password: String (Required)
        connection_port: Integer (Required)
        ansible_python_interpreter: String (Required)

        wazuh_deployment:
          type: String(Required)
          target: String (Required)
          manager_ip: String (conditional requirement)
          wazuh_branch: String (conditional requirement)
          s3_package_url: String (conditional requirement)
          local_package_path: String (conditional requirement)
          installation_files_path: String (Required)
          wazuh_install_path: String
          healt_check: Boolean

        qa_framework:
          wazuh_qa_branch: String (Required)
          qa_workdir: String (Required)

```

## Provisioning module fields information

### Available fields for the host info section

- `connection_method`: This field defines the connection method selected.
- `host`: String with the IP or DNS of the host.
- `user`: String with the existing username in the host.
- `password`: String with the password of the user.
- `connection_port`: The port number that will be used to establish the connection.
- `ansible_python_interpreter`: String with the Python path.


### Available fields for the wazuh deployment section

- `type`: Type of the installation method. This value can be either 'sources' or 'package'.
- `target`: Target of the Wazuh installation. This field can take only two different values:
  - `manager`: In case we want to install wazuh manager.
  - `agent`: In case we want to install wazuh agent.
- `manager_ip`: It contains the manager IP to get connected. This field will be required only when the target specified is 'agent'.
- `wazuh_branch`: It contains the branch containing the desired wazuh installation files. This field is only required when `type` has the value 'sources'.
- `s3_package_url`: It contains the url of a wazuh s3 package to download. This parameter is only required only two conditions:
  - `type` field has to have `package` selected as parameter.
  - The field `local_package_path` doesn't have to be defined.
- `local_package_path`: It contains the local path where the wazuh package is located. As with s3_package_url, this field will also be required only two conditions:
  - The field `type` has to have `package` selected as parameter.
  - The field `s3_package_url` doesn't have to be defined.
- `installation_files_path`: It contains the path where to place Wazuh repo.
- `wazuh_install_path`: It contains the path where wazuh will be installed. This field is not required and by default the path defined will be "/var/ossec".
- `healt_check`: Boolean that determinates if health check is performed.

### Available fields for qa framework section

- `wazuh_qa_branch`: It contains the branch containing the qa wazuh framework desired.
- `qa_workdir`: This parameter defines the path where the qa repo will be located.


## Test Launch

The test launch module is in charge of collecting all the tests from the configuration file and running them.
The structure for this module is:
```yaml
tests:
  host1:
    host_info:
        connection_method: String (Required)
        host: String (Required)
        user: String (Required)
        password: String (Required)
        ssh_private_key_file_path: String
        connection_port: Integer (Required)
        ansible_python_interpreter: String (Required)

      test:
        hosts: String
        type: String
        path: (required)
          test_files_path: String (required)
          run_tests_dir_path: String (required)
          test_results_path: String (required)
      parameters:
        tier: String
        stop_after_first_failure: Boolean
        keyword_expression: Srting
        traceback: String
        dry_run: Boolean Default value: False
        custom_args: String
        verbose_level: Boolean
        log_level: String
        markers: List

```

## Test launch module fields information
### Available fields Host info section 
All the fields available in this section are similar to the host info section defined in the provision module plus an additional parameter:
- `connection_method`: This field defines the connection method selected.
- `host`: String with the IP or DNS of the host.
- `user`: String with the existing username in the host.
- `password`: String with the password of the user.
- `connection_port`: The port number that will be used to establish the connection.
- `ansible_python_interpreter`: String with the Python path.
- `ssh_private_key_file_path` : This field is optional and contains the path of an ssh private key in case the user wants to log into the host with a different method.

### Available fields for the test section

- `hosts`: A list of desired hosts where the test must be launched. Every host in this list is represented by the same ip address used in every particular host under the provision module. If this field is empty, then the test will be launched in every host defined in ansible inventory.
- `type`: Tool used to run the tests. As for now, pytest is the only tool available in this field.
- `path`: This subsection field holds three different paths, all of them required:
  - `test_files_path`: The path pointing to the folder containing the desired tests to be launched.
  - `run_tests_dir_path`: The path to the folder from where we want to run the tests (can be different from test_files_path)
  - `test_results_path`: The path to the folder in the local machine where the test reports will be stored.

### Available fields for the parameter section
This section contains several optional parameters available for pytest:

- `tiers`: a list containing all the tiers to launch.
- `stop_after_first_failure`: -x option, stop the execution after the first failure. Default value set to false.
- `keyword_expression`: it works along test_files_path, if this value is empty then run all the tests in test_file_path. If it’s not empty, then there must be a valid regex to select specific tests in that folder. By default is set to None.
- `traceback`: modify the traceback printing of python [auto, long, short, native, line, no]. Default value set to auto.
- `dry_run`: collects all the tests but doesn’t run them. By default is set to false.
- `custom_args`: a list containing a string of space-separated values, corresponding to key-value pair.
- `verbose_level`: add the option --verbose. By default is set to false.
- `log_level`: sets the log level for the tests. By default is set to None.
- `markers`: allow adding markers to pytest command. If the test has a decorator with the same marker, then it will run. The format is a list of strings with each one desired marked.

### Configuration

The functionality of this module is to show custom outputs different than the vagrant default outputs or the ansible default outputs in order to get a better understanding of what is exactly happening when the script is running
```yaml
config:
    vagrant_output: Boolean
    ansible_output: Boolean
    logging:
        enable: Boolean
        level: String
        file: String
```

## Configuration module fields information

The available fields for this module are listed below. As additional information, none of the fields for this module are required.
- `vagrant_output`: Field that defines if the vagrant's outputs are going to be replaced by customized outputs(true) or if they remain with the default outputs (false)
- `ansible_output`: Field that defines if the ansible's outputs are going to be replaced by customized outputs(true) or if they remain with the default outputs (false)
- `logging`:
  - `enable`: This field is used for enabling(true) or disabling(false) the logging outputs option
  - `level`: This field defines the logging level for the outputs. Four options are available: `DEBUG, INFO, WARNING, ERROR, CRITICAL`
  - `file`: This field defines a path for a file where the outputs will be logged as well

> **Note**: It is recommended to set `vagrant_output` and `ansible_output` to `False`, and `logging/enable` to `True` (default values) in order to get a clean detail and report on the status of the process.

## A few more things...

The tool offers the next entries as  arguments:
- `-c, --config`: This parameter is used for declaring the path of the yaml configuration file.
- `-d, --destroy`: When this parameter is given, all the possible temporary files created while the script was running will be deleted. 
- `-h, --help`: Offers the helping message of this tool.

This tool has been developed with the possibility of running the different modules in an independent way, this means that is not necessary to create a whole yaml configuration file with all the modules. The user can instead add the modules he desires and if case he needs it, the user can add new more modules. 

An example of this use is to make a yaml configuration file for the deployment and provisioning, and after that we can create a new yaml file where only the test section will be given.
Therefore, the user can use already created testing environments with this tool and run different tests every time he needs it by just editing the yaml file containing the test module.

Another point worth mentioning is the task parallelization implemented. This functionality allows the user to run several instances of each module at the same time. As a result, the time for a single execution containing several instances in all three modules will be short.

It is mentioned before that all the modules are able to work independently. We are talking here about the Deployment, Provisioning, and Testing modules, which are the three main modules. However, the fourth module (Config) won't work if it's the only module defined. This will happen since this module is used for determining if the default outputs of the three main modules are going to be changed by custom outputs.

Regarding the Docker infrastructure deployment section, there are two existing dockerfiles ready to set up for the script, they can be found on the folder dockerfiles, each one of them in separate folders: `amazon_linux_2` and `ubuntu`. To generate the images of these dockerfiles, we have to set their paths in the `dockerfile_path`.

## Usage examples

In order to help the user getting a better understanding of the tool, some use cases will be shown as examples.

All the yaml configuration file examples can be used with the command line:
```
 python3 qa_ctl.py --config <PATH_TO_YAML_CONFIG_PATH>  --destroy
```

>**Note**: There might be necessary to use this command with root access

- Create a Docker instance from the ubuntu dockerfile placed on the specified path and with the name `test1`.
  
  <details>
    <summary>yaml configuration</summary>

  ```yaml
  deployment:
      host1:
          provider:
            docker:
              enabled: True
              dockerfile_path: /home/username/wazuh-qa/deps/wazuh_testing/wazuh_testing/qa_ctl/deployment/dockerfiles/ubuntu
              name: test1
  ```
  </details>


- Create a vagrant provider with the deployment module with 1024MB memory given, a single cpu assigned, and with the box placed in the path `qactl/ubuntu_20_04`.

  <details>
    <summary>yaml configuration</summary>

  ```yaml
  deployment:
    host2:
      provider:
      vagrant:
      enabled: True
      vagrantfile_path: /home/username/test2
      vagrant_box: qactl/ubuntu_20_04
      vm_memory: 1024
      vm_cpu: 1
      vm_name: test2
      vm_system: ubuntu
      label: test2
      vm_ip: 172.18.1.15
  ```
</details>

- Provisioning the vagrant machine created before with a wazuh manager using an url as a provider of the package and selecting the wazuh qa framework from the master branch
  <details>
    <summary>yaml configuration</summary>

  ```yaml
  provision:
      hosts:
        host2:
            host_info:
                connection_method: ssh
                host: 172.18.1.15
                user: vagrant
                password: vagrant
                connection_port: 22
                ansible_python_interpreter: /usr/bin/python
  
            wazuh_deployment:
                  type: package
                  target: manager
                  s3_package_url: https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-manager/wazuh-manager_4.1.5-1_amd64.deb
                  installation_files_path: /tmp
  
            qa_framework:
                wazuh_qa_branch: master
                qa_workdir: /tmp
  ```
</details>


- Lastly, run the general settings enabled test in the already provisioned machine.
  <details>
    <summary>yaml configuration</summary>

  ```yaml

  tests:
      host2:
          host_info:
              connection_method: ssh
              host: 172.18.1.15
              user: vagrant
              password: vagrant
              connection_port: 22
              ansible_python_interpreter: /usr/bin/python3

          test:
              hosts: 172.18.1.15
              type: pytest
              path:
                  test_files_path: /tmp/wazuh-qa/tests/integration/test_vulnerability_detector/test_general_settings/test_general_settings_enabled.py
                  run_tests_dir_path: /tmp/wazuh-qa/tests/integration
                  test_results_path: /tmp/test-results
  ```
</details>


- This is an example of a yaml configuration with all the modules given. In this case, this configuration will create an `Ubuntu` vagrant instance with the vagrantfile located in the `/tmp` folder, with two assigned CPUs and the ip `172.16.1.10`. For the provisioning module, it will install a wazuh-agent from a local package connected to the manager created before through the `manager_ip` value, and will run the general settings enabled test on the manager created before. Lastly, the config section will configure custom outputs.

  <details>
    <summary>yaml configuration</summary>
    
  ```yaml

  deployment:
    host1:
      provider:
        vagrant:
          enabled: True
            vagrantfile_path: '/tmp'
            vagrant_box: 'qactl/ubuntu_20_04'
            vm_memory: 1024
            vm_cpu: 2
            vm_name: 'ubu20'
            vm_system: 'Linux'
            label: 'qactl'
            vm_ip: '172.16.1.10'

  provision:
    hosts:
      host1:
        host_info:
          connection_method: 'ssh'
          host: '172.16.1.10'
          user: 'vagrant'
          password: 'vagrant'
          connection_port: '22'
          ansible_python_interpreter: '/usr/bin/python3'

        wazuh_deployment:
          type: 'package'
          target: 'agent'
          manager_ip: 172.18.1.15
          local_package_path: '/home/username/work/packages/wazuh-agent4.2.0-1_amd64.deb'
          installation_files_path: '/tmp'
          health_check: False

        qa_framework:
          wazuh_qa_branch: 'master'
          qa_workdir: '/tmp'

  tests:
    host1:
      host_info:
      connection_method: ssh
      host: 172.16.1.15
      user: vagrant
      password: vagrant
      connection_port: 22
      ansible_python_interpreter: /usr/bin/python3
      
    test:
      hosts: '172.16.1.15'
      type: pytest
      path:
        test_files_path: '/tmp/wazuh-qa/tests/integration/test_vulnerability_detector/test_general_settings/test_general_settings_enabled.py'
        run_tests_dir_path: '/tmp/wazuh-qa/tests/integration/test_vulnerability_detector/test_general_settings'
        test_results_path: '/home/username/Documents/test'
  config:
    vagrant_output: True
    ansible_output: True
    logging:
      enable: True
      level: INFO


  ```
</details>
