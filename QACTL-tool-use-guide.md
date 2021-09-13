## Table of contents

- [Introduction](#introduction)
- [How to use it](#how-to-use-it)
  - [Parameter restrictions](#parameter-restrictions)
  - [Modes of use](#modes-of-use)
    - [Automatic: Launch tests and get results](#automatic-launch-tests-and-get-results)
    - [Manual: Specify configuration parameters to launch the tests](#manual-specify-configuration-parameters-to-launch-the-tests)
    - [Simulated: Generate configuration file](#simulated-generate-configuration-file)
  - [Examples](#examples)
- [Setting up a configuration file](#setting-up-a-configuration-file)
  - [Infrastructure deployment module](#infrastructure-deployment-module)
  - [Provisioning module](#provisioning-module)
  - [Test launch module](#test-launch-module)
  - [Configuration module](#configuration-module)
  - [YAML configuration file examples](#yaml-configuration-file-examples)

## Introduction

`qa-ctl` is a tool that allows you to run tests locally and get the results without worrying about deployments,
provisioning and dependencies.

To achieve this, this tool does the following:

- Deploy the necessary environment to run the test/s.
- Install Wazuh and the wazuh-qa framework on the deployed machines. In short, ready for running tests.
- Execute the test/s and return the results to the user.

The next image shows a flow diagram of how the tool works:

<p align="center">
    <img width="576px" "height="538px" src="https://github.com/wazuh/wazuh-qa/wiki/images/qactl_tool_imgs/diagram.png">
</p>

- The `infrastructure` module is in charge of creating the needed VMs or containers.
- The `provisioning` module is in charge of installing Wazuh and the QA framework.
- The `run` test module is responsible for launching the tests and collect the results.

## How to use it

First, you have to [install](https://github.com/wazuh/wazuh-qa/wiki/QACTL-tool-installation-guide) the `qa-ctl` tool.

Check if it is installed with `qa-ctl -h`, you will see the help menu.

This tool has the following parameters:

- `-c, --config <file_path>`: Specifies the custom configuration file to be used in `qa-ctl`.
- `-d, --dry-run`: Mode to simulate and create the configuration file from one or a set of tests.
- `-h, --help`: Show `qa-ctl` help menu.
- `-p, --persistent`: If specified, the environment will not be destroyed after finishing.
- `-r, --run <test_name_1> <test_name_2> ...`: Set automatic mode. Launches the tests and returns the results.
- `-v, --version <version>`: Specify the version of wazuh to use. If not set, the latest released version will be used.

### Parameter restrictions

- `-r`, `--run` cannot be launched with the `-c`, `--config` parameter. They represent independent modes.
- `-d`, `--dry-run` only can be launch with `r`, `--run` mode (incompatible with `-c`, `--config`).
- `-v`, `--version` parameter has to be in `x.y.z` format. For example `4.2.1`.

### Modes of use

#### Automatic: Launch tests and get results

Quick and simple way to run a test and obtain the results using default parameters. In this mode the documentation of
each test is parsed and the necessary environment is built, provisioning it with the specific Wazuh version
(with the `-v` or` --version` parameter), or else using the latest released version of Wazuh and using the equivalent
version for `wazuh-qa` framework and testing.

To use this mode, we have to specify the parameter `-r`, `--run`.

```bash
qa-ctl -r <test_name_1> <test_name_2> ...
```

<details>
<summary>For example:</summary>

```
$ qa-ctl -r test_general_settings_enabled

2021-09-08 16:35:31,651 - INFO - Starting 1 instances deployment
2021-09-08 16:38:01,214 - INFO - The instances deployment has finished sucessfully
2021-09-08 16:38:01,216 - INFO - Checking hosts SSH connection
2021-09-08 16:38:07,584 - INFO - Hosts connection OK. The instances are accessible via ssh
2021-09-08 16:38:07,584 - INFO - Provisioning 1 instances
2021-09-08 16:39:06,467 - INFO - Waiting 60 seconds before performing the healthcheck in 10.150.50.2 host
2021-09-08 16:41:10,351 - INFO - The instances have been provisioned sucessfully
2021-09-08 16:41:10,354 - INFO - Launching 1 tests
2021-09-08 16:41:10,355 - INFO - Waiting for tests to finish
2021-09-08 16:41:11,277 - INFO - Running /tmp/wazuh-qa/tests/integration/test_vulnerability_detector/test_general_settings/test_general_settings_enabled.py test on ['10.150.50.2'] hosts
2021-09-08 16:41:17,795 - INFO -

============================= test session starts ==============================
platform linux -- Python 3.8.10, pytest-6.2.4, py-1.10.0, pluggy-0.13.1
rootdir: /tmp/wazuh-qa/tests/integration, configfile: pytest.ini
plugins: html-2.0.1, testinfra-6.0.0, metadata-1.11.0, testinfra-6.4.0
collected 4 items

../../tmp/wazuh-qa/tests/integration/test_vulnerability_detector/test_general_settings/test_general_settings_enabled.py . [ 25%]
ss.                                                                      [100%]

- generated html file: file:///tmp/wazuh-qa/test/integration/reports/test_report-2021-09-08 16:41:11.277135.html -
========================= 2 passed, 2 skipped in 1.90s =========================


2021-09-08 16:41:17,795 - INFO - The test run is finished
2021-09-08 16:41:17,795 - INFO - Destroying 1 instances
2021-09-08 16:41:23,143 - INFO - The instances have been destroyed sucessfully
```

</details>

#### Manual: Specify configuration parameters to launch the tests

Mode in which we will specify the execution flow of `qa-ctl` according to a custom configuration file.

In this configuration file we can specify information about deployment, provisioning and test executions.

Also, it can be used to launch independent modules without making use of the complete flow, for example, if we
already have a previously deployed and provisioned environment, we can choose only to launch the tests, or choose
to deploy only one environment...

To use this mode, we have to specify the parameter `-c`, `--config`.

```
qa-ctl -c <configuration_file_path>
```

#### Simulated: Generate configuration file

Hybrid mode that mixes automatic and manual. The objective is to automatically generate the configuration file from
the information of the tests, so that later the user can edit and use it.

Useful when you want to change some small aspect of the default configuration. First the configuration file is
generated with the information of the tests, it is modified and then it is launched in manual mode.

To use this mode, we have to specify the parameter `-d`, `--dry-run` along with `-r`, `--run`.

```
qa-ctl -d <test_name_1> <test_name_2> ...

```

<details>
<summary>For example:</summary>

```bash
$ qa-ctl -d -r test_general_settings_enabled

2021-09-08 16:53:40,179 - INFO - Run as dry-run mode. Configuration file saved in /tmp/config_1631112820.17649.yaml
```

</details>

### Examples

- Launch a single test.

```bash
qa-ctl -r <test_name>
```

- Launch multiple tests (In parallel using different environments).

```bash
qa-ctl -r <test_name_1> <test_name_2> ...
```

- Launch a test with a specific version of Wazuh.

```bash
qa-ctl -r <test_name> -v <wazuh_version>
```

- Launch tests with a custom `qa-ctl` configuration (tests are include in the configuration file).

```bash
qa-ctl -c <config_file_path>
```

- Generate tests configuration, update it and run it.

```bash
qa-ctl -d -r <test_name>
<Update configuration file generated in showed path>
qa-ctl -c <config_file_path>
```

## Setting up a configuration file

QACTL consists of several different and independent modules. We can interact with these modules through the
configuration file written in `YAML` format.

This file consists of 4 main sections:

- `deployment`: Section to specify the environment deployment information. It can be with vagrant or docker.
- `provision`: Section for specifying provisioning information for an environment.
- `tests`: Section to indicate the set of tests to be run in the environment.
- `config`: Section to specify custom `qa-ctl` configuration such as logging.

All of these sections have different configurable fields, some required, some optional, and declared by default if not
given. In the next points of this guide, every field will be explained so the user can understand it better.

### Infrastructure deployment module

```yaml
deployment:
    host1:
      provider:
        vagrant:
          enabled: Boolean (Required)
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
            enabled: Boolean (Required)
            dockerfile_path: String (Required)
            name: String (Required)
            ip: String (Optional) Default: None
            remove: Boolean (Optional) Default: False
            ports: Dict (optional) Default: Empty
            detach: Boolean (Optional) Default: True
            stdout: Boolean (Optional) Default: False
            stderr: Boolean (Optional) Default: False
```

**Vagrant deployment**

- `enabled`: If False, the VM won’t be created.
- `vagrantfile_path`: Path where the vagrantfile will be created.
- `vagrant_box`: Path to the vagrant box file, link, or box specifying the box that will be used.
- `vm_memory`: Memory assigned to the VM.
- `vm_cpu`: Number of CPUs assigned to the VM.
- `vm_system`:  VM operative system.
- `Label`: Assign a label to the VM.
- `vm_ip`: assigned IP for the VM.

**Docker deployment**

- `enabled`: If False, the VM won’t be created.
- `dockerfile_path`: Path to an existing dockerfile.
- `name`: Name assigned to the container.
- `ip`: Assign a static IP to the container. To do this, we need to create a docker network based on the
        specified network (to not create as many networks as containers specified in the dockerfile, only one network
        is allowed). The mask of the network is `/24`. If this option is not specified, the container won't have a
        static IP.
- `remove`: Remove the container. Defaults to `False`.
- `Ports`: Ports to bind to the container. This field is a python dictionary where the keys are the ports to bind
           inside the container in the form port/protocol, and the values are integers corresponding to the ports we
           want to open in the host.
- `detach`: Run the container in the background. Defaults to `True`.
- `stdout`: Return logs from STDOUT when detach=`False`.
- `stderr`: Return logs from STDERR when detach=`False`.

### Provisioning module

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
          manager_ip: String (Optional)
          wazuh_branch: String (Optional)
          s3_package_url: String (Optional)
          local_package_path: String (Optional)
          system: String (Optional) Possible values: rpm, deb, windows, macos, solaris10, 
          solaris11, rpm5, wpk-linux, wpk-windows
          version: String (Optional)
          revision: String (Optional)
          repository: String (Optional)
          installation_files_path: String (Required)
          wazuh_install_path: String
          healt_check: Boolean

        qa_framework:
          wazuh_qa_branch: String (Required)
          qa_workdir: String (Required)
```

**Host info section**

- `connection_method`: This field defines the connection method selected.
- `host`: String with the IP or DNS of the host.
- `user`: String with the existing username in the host.
- `password`: String with the password of the user.
- `connection_port`: The port number that will be used to establish the connection.
- `ansible_python_interpreter`: String with the Python path.

**Wazuh deployment section**

- `type`: Type of the installation method. This value can be either `sources` or `package`.
- `target`: Target of the Wazuh installation. This field can take only two different values:
  - `manager`: In case we want to install wazuh manager.
  - `agent`: In case we want to install wazuh agent.
- `manager_ip`: Manager IP to get connected. This field will be required only when the target
                specified is `agent`.
- `wazuh_branch`: Branch containing the desired wazuh installation files. This field is only
                  required when `type` has the value `sources`.
- `s3_package_url`: URL of a Wazuh s3 package to download. This parameter is only required under
                    two conditions:
  - `type` field has to have `package` selected as the parameter.
  - None of the fields, `local_package_path`,  `system`, `version`, `revision`, and `repository` are given.
- `local_package_path`: Local path where the wazuh package is located. As with `s3_package_url`,
                        this field will also be required under two conditions:
  - The field `type` has to have `package` selected as the parameter.
  - None of the fields, `s3_package_url`, `system`, `version`, `revision`, and `repository` are given.
- `system`: System type of the machine where Wazuh is going to be installed. The available values for this parameter are: `rpm, deb, windows, macos, solaris10, 
          solaris11, rpm5, wpk-linux, and wpk-windows`.
    > **Note**:  If the fields `version, revision and repository` are not given along with this field itself, there will be a validation error.

  This parameter is only required under
            two conditions:
  -  The field `type` has to have `package` selected as the parameter.
  -  None of the fields `s3_package_url` and `local_package_path` are given.
- `version`: Version of the Wazuh package that is going to be installed. This parameter is only required under
             two conditions:
  -  The field `type` has to have `package` selected as the parameter.
  -  None of the fields `s3_package_url` and `local_package_path` are given.
- `revision`: Revision of the Wazuh package that is going to be installed. This parameter is only required
              under two conditions:
  -  The field `type` has to have `package` selected as the parameter.
  -  None of the fields `s3_package_url` and `local_package_path` are given.
- `repository`: S3 Repository where the Wazuh package is located. This parameter is only required under two conditions:
  -  The field `type` has to have `package` selected as the parameter.
  -  None of the fields `s3_package_url` and `local_package_path` are given.
- `installation_files_path`: Path where to place all the downloaded Wazuh installation files.
- `wazuh_install_path`: Path where Wazuh will be installed. This field is not required and by
                        default the path defined will be `/var/ossec`.
- `healt_check`: Boolean that determinates if health check is performed.
  
**QA framework section**

- `wazuh_qa_branch`: Branch containing the qa wazuh framework desired.
- `qa_workdir`: Defines the path where the qa repo will be located.

### Test launch module

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

**Host info section**

- `connection_method`: This field defines the connection method selected.
- `host`: String with the IP or DNS of the host.
- `user`: String with the existing username in the host.
- `password`: String with the password of the user.
- `connection_port`: The port number that will be used to establish the connection.
- `ansible_python_interpreter`: String with the Python path.
- `ssh_private_key_file_path`: This field is optional and contains the path of an ssh private key in case the user
                               wants to log into the host with a different method.

**Test section**

- `hosts`: A list of desired hosts where the test must be launched. Every host in this list is represented by
           the same ip address used in every particular host under the provision module. If this field is empty,
           then the test will be launched in every host defined in ansible inventory.
- `type`: Tool used to run the tests. As for now, pytest is the only tool available in this field.
- `path`: This subsection field holds three different paths, all of them required:
  - `test_files_path`: The path pointing to the folder containing the desired tests to be launched.
  - `run_tests_dir_path`: The path to the folder from where we want to run the tests (can be different from
                          `test_files_path`).
  - `test_results_path`: The path to the folder in the local machine where the test reports will be stored.

**Parameters section**

- `tiers`: A list containing all the tiers to launch.
- `stop_after_first_failure`: -x option, stop the execution after the first failure. Default value set to `false`.
- `keyword_expression`: It works along `test_files_path`, if this value is empty then run all the tests in `test_file_path`.
                        If it’s not empty, then there must be a valid regex to select specific tests in that folder.
                        By default is set to `None`.
- `traceback`: Modify the traceback printing of python, its possible values are `auto, long, short, native, line, no`. Default value set to `auto`.
- `dry_run`: Collects all the tests but doesn’t run them. By default is set to `false`.
- `custom_args`: A list containing a string of space-separated values, corresponding to key-value pair.
- `verbose_level`: Add the option `--verbose`. By default is set to `false`.
- `log_level`: Sets the log level for the tests. By default is set to `None`.
- `markers`: Allow adding markers to pytest command. If the test has a decorator with the same marker, then it will
             run. The format is a list of strings with each one desired marked.

### Configuration module

```yaml
config:
    vagrant_output: Boolean
    ansible_output: Boolean
    logging:
        enable: Boolean
        level: String
        file: String
```

- `vagrant_output`: Field that defines if the vagrant's outputs are going to be replaced by customized outputs(`true`) or
                    if they remain with the default outputs (`false`).
- `ansible_output`: Field that defines if the ansible's outputs are going to be replaced by customized outputs(`true`) or
                    if they remain with the default outputs (`false`).
- `logging`:
  - `enable`: This field is used for enabling(`true`) or disabling(`false`) the logging outputs option.
  - `level`: This field defines the logging level for the outputs. Four options are
             available: `DEBUG, INFO, WARNING, ERROR, CRITICAL`.
  - `file`: This field defines a path for a file where the outputs will be logged as well.

> **Note**: It is recommended to set `vagrant_output` and `ansible_output` to `False`, and `logging/enable` to `True` \
(default values) in order to get a clean detail and report on the status of the process.


### YAML configuration file examples

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


- Create a vagrant provider with the deployment module with 1024MB memory given, a single cpu assigned, and with the
  box imported as `qactl/ubuntu_20_04`.

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

- Provisioning the vagrant machine created before with a wazuh manager using a S3 URL as a provider of the package and
  selecting the wazuh qa framework from the `master` branch.
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


- Run the `general settings enabled` test in the already provisioned machine.

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


- This is an example of a yaml configuration with all the modules given. In this case, this configuration will
  create an `Ubuntu` vagrant instance with the vagrantfile located in the `/tmp` folder, with two assigned CPUs
  and the ip `172.16.1.10`. For the provisioning module, it will install a wazuh-agent from a local package
  connected to the manager created before through the `manager_ip` value, and will run the general settings
  enabled test on the manager created before. Lastly, the config section will configure custom outputs.

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

