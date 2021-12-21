[![version](https://img.shields.io/badge/version-v0.2-blue)]()
[![status](https://img.shields.io/badge/status-needs%20updating-yellow)]()


## Table of contents

- [Introduction](#introduction)
- [How to use it](#how-to-use-it)
  - [Parameter restrictions](#parameter-restrictions)
  - [Modes of use](#modes-of-use)
    - [Automatic: Launch tests and get results](#automatic-launch-tests-and-get-results)
    - [Manual: Specify configuration parameters to launch the tests](#manual-specify-configuration-parameters-to-launch-the-tests)
    - [Mixed: generate an environment with automatic mode and then reused it in manual mode](#mixed-generate-an-environment-with-automatic-mode-and-then-reused-it-in-manual-mode)
  - [Examples](#examples)
- [Setting up a configuration file](#setting-up-a-configuration-file)
  - [Infrastructure deployment module](#infrastructure-deployment-module)
  - [Provisioning module](#provisioning-module)
  - [Test launch module](#test-launch-module)
  - [Configuration module](#configuration-module)
  - [YAML configuration file examples](#yaml-configuration-file-examples)
  - [Deployment section](#deployment-section)
  - [Provision section](#provision-section)
    - [Host info section](#host-info-section)
    - [Wazuh deployment section](#wazuh-deployment-section)
    - [QA framework section](#qa-framework-section)
  - [Testing section](#testing-section)
  - [Full YAML configuration files examples](#full-yaml-configuration-files-examples)

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

- The `infrastructure` module is in charge of creating the needed VMs.
- The `provisioning` module is in charge of installing Wazuh and the QA framework.
- The `run` test module is responsible for launching the tests and collect the results.

## How to use it

First, you have to [install](https://github.com/wazuh/wazuh-qa/wiki/QACTL-tool-installation-guide) the `qa-ctl` tool.

Check if it is installed with `qa-ctl -h`, you will see the help menu.

This tool has the following parameters:

- `-c, --config <file_path>`: Specifies the custom configuration file to be used in `qa-ctl`.
- `-d, --debug`: Run in debug mode. You can increase the debug level with more [-d+]:
   - `-d`: Show `qa-ctl` debug logs.
   - `-dd`: Show `vagrant` and `ansible` output in logs.
- `-h, --help`: Show `qa-ctl` help menu.
- `-p, --persistent`: If specified, the environment will not be destroyed after finishing.
- `-r, --run <test_name_1> <test_name_2> ...`: Set automatic mode. Launches the tests and returns the results.
- `-v, --version <version>`: Specify the version of wazuh to use. If not set, the latest released version will be used.
- `-o, --os <os_system>`: Specify the system(s) with which to launch each test.
- `--dry-run`: Config generation mode. The test data will be processed and the configuration will be generated without running anything.
- `--no-validation`: Disable the script parameters validation.
- `--no-validation-logging`: Disable parameters validation logging. Useful when it is run inside a docker container.
- `--qa-branch <repository_branch>`:  Set a custom wazuh-qa branch to use in the run and provisioning. This has a higher priority than the specified in the configuration file.
- `--skip-deployment`: Flag to skip the deployment phase. Set it only if `-c` or `--config` (manual mode) was specified.
- `--skip-provisioning`: Flag to skip the provisioning phase. Set it only if `-c` or `--config` (manual mode) was specified. 
- `--skip-testing`: Flag to skip the testing phase. Set it only if `-c` or `--config` (manual mode) was specified.

### Parameter restrictions

- `-r`, `--run` cannot be launched with the `-c`, `--config` parameter. They represent independent modes.
- `-d`, `--dry-run` can only be specified with `r`, `--run` (automatic mode).
- `-v`, `--version` parameter has to be in `x.y.z` format. For example `4.2.1`.
- `-v`, `--version` can only be specified with `r`, `--run` (automatic mode).
- `-v`, `--version` value has to correspond to a released version of Wazuh.
- `--skip-deployment`, `--skip-provisioning`, `--skip-testing` can only be launched with `-c`, `--config` (manual mode).
- `--qa-branch` must exist in the wazuh-qa repository on github.
- `-r`, `--run` values have to correspond to existing and documented tests of the specified branch of the wazuh-qa repository.
- `-o`, `--os` can only be specified with `r`, `--run` (automatic mode).

**Allowed values**

- `-o`, `--os`: [`centos`, `ubuntu`, `windows`]

### Modes of use

#### Automatic: Launch tests and get results

A quick and simple way to run a test and obtain the results using default parameters. In this mode the documentation of
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
qa-ctl -r test_general_settings_enabled

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

> **Important note**: To use the automatic mode, the test/s must be previously documented according to the documentation schema (proposed by `qa-docs` team), in order to obtain the necessary information for the run. You can see an example of a documented test in [this piece of code](https://github.com/wazuh/wazuh-qa/blob/master/tests/integration/test_vulnerability_detector/test_general_settings/test_general_settings_enabled.py#L1-L55).

#### Manual: Specify configuration parameters to launch the tests

A mode in which we will specify the execution flow of `qa-ctl` according to a custom configuration file.

In this configuration file, we can specify information about deployment, provisioning and test executions.

Also, it can be used to launch independent modules without making use of the complete flow, for example, if we
already have a previously deployed and provisioned environment, we can choose only to launch the tests, or choose
to deploy only one environment...

To use this mode, we have to specify the parameter `-c`, `--config`.

```
qa-ctl -c <configuration_file_path>
```

#### Mixed: generate an environment with automatic mode and then reused it in manual mode

The purpose of this mode is to generate a configuration file automatically, to modify it later and use it in qa-ctl.

We can generate a configuration file automatically in the following ways:

- Perform a run as simulated `--dry-run`, in this way instead of executing the different phases corresponding to one or 
a set of tests, the corresponding configuration file is generated and its path is indicated so that it can be modified 
and used later.

- After a `qa-ctl` run, if the `-p`, or `--persistent` parameter was specified, then the environment and the 
corresponding generated files will not be deleted, so we can use the auto-generated configuration file again.

The usefulness of this mode is to be able to specify specifically the environment we want to deploy, or in case it 
is already deployed, to reuse it and directly use it to speed up the whole testing process and obtain results.

**For example**

<details>
<summary>Generate a configuration file from test information</summary>

```bash
qa-ctl --dry-run -r test_general_settings_enabled
    2021-09-08 16:53:40,179 - INFO - Run as dry-run mode. Configuration file saved in /tmp/config_1631112820.17649.yaml
```

</details>

<details>
<summary>Perform a run with persistent environment, and then re-launch the test in the same environment</summary>

```bash
qa-ctl -r test_general_settings_enabled --persistent
    ......
    INFO - Configuration file saved in /tmp/qa_ctl/config_1633608335.685262.yaml
    ......
qa-ctl -c  /tmp/qa_ctl/config_1633608335.685262.yaml --skip-deployment --skip-provisioning
```

</details>

### Examples

<details>
<summary>Launch a single test.</summary>

```bash
qa-ctl -r <test_name>
```

</details>


<details>
<summary>Launch a single test for a specified system (in this case windows).</summary>

```bash
qa-ctl -r <test_name> -o windows
```

</details>

<details>
<summary>Launch multiple tests (In parallel using different environments).</summary>

```bash
qa-ctl -r <test_name_1> <test_name_2> ...
```

</details>

<details>
<summary>Launch two tests for multiple systems (cross product).</summary>

```bash
qa-ctl -r <test_name_1> <test_name_2> -o centos windows
```

</details>


<details>
<summary>Launch a test with a specific version of Wazuh.</summary>

```bash
qa-ctl -r <test_name> -v <wazuh_version>
```

</details>

<details>
<summary>Launch tests with a custom <code>qa-ctl</code> configuration (tests are include in the configuration file).</summary>

```bash
qa-ctl -c <config_file_path>
```

</details>

<details>
<summary>Generate tests configuration, update it and run it.</summary>

```bash
qa-ctl --dry-run -r <test_name>
    ...
    <Update configuration file generated in showed path>
    ...
qa-ctl -c <config_file_path>
```

</details>

<details>
<summary>Launch a test, without destroying the environment.</summary>

```bash
qa-ctl -r <test_name> -p
```

</details>

<details>
<summary>Launching a test using a custom branch of wazuh-qa.</summary>

```bash
qa-ctl -r <test_name> --qa-branch <wazuh_qa_branch>
```

</details>

<details>
<summary>Launch <code>qa-ctl</code> in debug mode</summary>

```bash
qa-ctl -r <test_name> -d
```

</details>

<details>
<summary>Launch <code>qa-ctl</code> in debug 2 mode</summary>

```bash
qa-ctl -r <test_name> -dd
```

</details>

<details>
<summary>Launch a test run without destroying the environment, and then relaunch the test again.</summary>

```bash
qa-ctl -r test_general_settings_enabled --persistent
    ......
    INFO - Configuration file saved in /tmp/qa_ctl/config_1633608335.685262.yaml
    ......

qa-ctl -c /tmp/qa_ctl/config_1633608335.685262.yaml --skip-deployment --skip-provisioning
```

</details>

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
    host_1:
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

### Provisioning module

```yaml
provision:
  hosts:
    host1:
      host_info:
        ansible_connection: String (Required)
        ansible_user: String (Required)
        ansible_password: String (Required)
        ansible_port: Integer (Required)
        ansible_python_interpreter: String (Required)
        system: String (Required)
        installation_files_path: String (Required)
        host: String (Required)

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
          wazuh_install_path: String (Required)
          healt_check: Boolean (Optional)

        qa_framework:
          wazuh_qa_branch: String (Required)
          qa_workdir: String (Required)
```

**Host info section**

- `ansible_connection`: This field defines the connection method selected.
- `ansible_user`: String with the existing username in the host.
- `ansible_password`: String with the password of the user.
- `ansible_port`: The port number that will be used to establish the connection.
- `ansible_python_interpreter`: String with the Python path.
- `system`:  System type of the machine where qa-ctl is going to be launched.
- `installation_files_path`: Path where qa-ctl is going to be installed
- `host`: String with the IP or DNS of the host.

**Wazuh deployment section**

- `type`: Type of the installation method. This value can be either `sources` or `package`.
- `target`: Target of the Wazuh installation. This field can take only two different values:
  - `manager`: In case we want to install wazuh manager.
  - `agent`: In case we want to install wazuh agent.
- `manager_ip`: Manager IP to get connected. This field will be required only when the target specified is `agent`.
- `wazuh_branch`: Branch containing the desired wazuh installation files. This field is only required when `type` has the value `sources`.
- `s3_package_url`: URL of a Wazuh s3 package to download. This parameter is only required under two conditions:
  - `type` field has to have `package` selected as the parameter.
  - None of the fields, `local_package_path`,  `system`, `version`, `revision`, and `repository` are given.
- `local_package_path`: Local path where the wazuh package is located. As with `s3_package_url`, this field will also be required under two conditions:
  - The field `type` has to have `package` selected as the parameter.
  - None of the fields, `s3_package_url`, `system`, `version`, `revision`, and `repository` are given.
- `system`: System type of the machine where Wazuh is going to be installed. The available values for this parameter are: `rpm, deb, windows, macos, solaris10, solaris11, rpm5, wpk-linux, and wpk-windows`.
> **Note**:  If the fields `version, revision and repository` are not given along with this field itself, there will be a validation error.

  This parameter is only required under two conditions:
  -  The field `type` has to have `package` selected as the parameter.
  -  None of the fields `s3_package_url` and `local_package_path` are given.
- `version`: Version of the Wazuh package that is going to be installed. This parameter is only required under two conditions:
  -  The field `type` has to have `package` selected as the parameter.
  -  None of the fields `s3_package_url` and `local_package_path` are given.
- `revision`: Revision of the Wazuh package that is going to be installed. This parameter is only required under two conditions:
  -  The field `type` has to have `package` selected as the parameter.
  -  None of the fields `s3_package_url` and `local_package_path` are given.
- `repository`: S3 Repository where the Wazuh package is located. This parameter is only required under two conditions:
  -  The field `type` has to have `package` selected as the parameter.
  -  None of the fields `s3_package_url` and `local_package_path` are given.
- `installation_files_path`: Path where to place all the downloaded Wazuh installation files.
- `wazuh_install_path`: Path where Wazuh will be installed. This field is not required and by default the path defined will be `/var/ossec`.
- `healt_check`: Boolean that determinates if health check is performed.
  
**QA framework section**

- `wazuh_qa_branch`: Branch containing the qa wazuh framework desired.
- `qa_workdir`: Defines the path where the qa repo will be located.

### Test launch module

```yaml
tests:
  host1:
    host_info:
        ansible_connection: String (Required)
        ansible_user: String (Required)
        ansible_password: String (Required)
        ansible_port: Integer (Required)
        ansible_python_interpreter: String (Required)
        system: String (Required)
        installation_files_path: String (Required)
        host: String (Required)
        ssh_private_key_file_path: String (Optional)

      test:
        hosts: String (Required)
        type: String (Required)
        path: (Required)
          test_files_path: String (Required)
          run_tests_dir_path: String (Required)
          test_results_path: String (Required)
        wazuh_install_path: String (Required)
        system: String (Required)
        component: String(Required)
        modules: List (Required)
      parameters:
        tier: String (Optional)
        stop_after_first_failure: Boolean (Optional)
        keyword_expression: String (Optional)
        traceback: String (Optional)
        dry_run: Boolean (Optional) Default value: False
        custom_args: String (Optional)
        verbose_level: Boolean (Optional)
        log_level: String (Optional)
        markers: List (Optional)
```

**Host info section**

- `ansible_connection`: This field defines the connection method selected.
- `ansible_user`: String with the existing username in the host.
- `ansible_password`: String with the password of the user.
- `ansible_port`: The port number that will be used to establish the connection.
- `ansible_python_interpreter`: String with the Python path.
- `system`:  System type of the machine where qa-ctl is going to be launched.
- `installation_files_path`: Path where qa-ctl is going to be installed
- `host`: String with the IP or DNS of the host.
- `ssh_private_key_file_path`: This field is optional and contains the path of an ssh private key in case the user
                               wants to log into the host with a different method.

**Test section**

- `hosts`: A list of desired hosts where the test must be launched. Every host in this list is represented by the same ip address used in every particular host under the provision module. If this field is empty then the test will be launched in every host defined in ansible inventory.
- `type`: Tool used to run the tests. As for now, pytest is the only tool available in this field.
- `path`: This subsection field holds three different paths, all of them required:
  - `test_files_path`: The path pointing to the folder containing the desired tests to be launched.
  - `run_tests_dir_path`: The path to the folder from where we want to run the tests (can be different from `test_files_path`).
  - `test_results_path`: The path to the folder in the local machine where the test reports will be stored.
- `wazuh_install_path`: Path where wazuh is going to be installed in the virtual machine
- `system`: System of the host where the test is going to be launched
- `component`: Wazuh component target of the test.
- `modules`: The correspondent modules of the test.

**Parameters section**

- `tiers`: A list containing all the tiers to launch.
- `stop_after_first_failure`: -x option, stop the execution after the first failure. Default value set to `false`.
- `keyword_expression`: It works along `test_files_path`, if this value is empty then run all the tests in `test_file_path`. If it’s not empty, then there must be a valid regex to select specific tests in that folder. By default is set to `None`.
- `traceback`: Modify the traceback printing of python, its possible values are `auto, long, short, native, line, no`. Default value set to `auto`.
- `dry_run`: Collects all the tests but doesn’t run them. By default is set to `false`.
- `custom_args`: A list containing a string of space-separated values, corresponding to key-value pair.
- `verbose_level`: Add the option `--verbose`. By default is set to `false`.
- `log_level`: Sets the log level for the tests. By default is set to `None`.
- `markers`: Allow adding markers to pytest command. If the test has a decorator with the same marker, then it will run. The format is a list of strings with each one desired marked.

### Configuration module

```yaml
config:
    qa_ctl_launcher_branch: String (Required when qa-ctl is launched on Windows)
    vagrant_output: Boolean (Optional)
    ansible_output: Boolean (Optional
    logging:
        enable: Boolean (Optional)
        level: String (Optional)
        file: String (Optional)
```
- `qa_ctl_launcher_branch`:Field that defines the branch that will be used to launch `qa-ctl` in the Docker container. This field is optional an only required when `qa-ctl` is being used in Windows and using `Docker` for the `provisioning` or `testing` modules.
- `vagrant_output`: Field that defines if the vagrant's outputs are going to be replaced by customized outputs(`true`) or if they remain with the default outputs (`false`).
- `ansible_output`: Field that defines if the ansible's outputs are going to be replaced by customized outputs(`true`) or if they remain with the default outputs (`false`).
- `logging`:
  - `enable`: This field is used for enabling(`true`) or disabling(`false`) the logging outputs option.
  - `level`: This field defines the logging level for the outputs. Four options are available: `DEBUG, INFO, WARNING, ERROR, CRITICAL`.
  - `file`: This field defines a path for a file where the outputs will be logged as well.

> **Note**: It is recommended to set `vagrant_output` and `ansible_output` to `False`, and `logging/enable` to `True` \
(default values) in order to get a clean detail and report on the status of the process.


### YAML configuration file examples

Now, there are going to be shown some examples of YAML files separated by use cases.

### Deployment section 

The most mentionable fields for the deployment section are `vagrant_box` and `vagrantfile_path`.
These fields are used to determine the name of the box and the location of the vagrantfile that is going to be used. As for the `vagrant_box` field, any available box can be used, but `qa-ctl` provides some boxes that are ready to use with this tool. The name of these boxes are: `qactl/ubuntu_20_04`, `qactl/centos_8` and `qactl/windows_2019`


- Deploying a single virtual machine instance:
  <details>
    <summary>yaml configuration</summary>

  ```yaml
  deployment:
    host_1:
      provider:
        vagrant:
          enabled: true
          vagrantfile_path: /tmp/wazuh_qa_ctl
          vagrant_box: qactl/ubuntu_20_04  # Any vagrant box can be used
          vm_memory: 512
          vm_cpu: 1
          vm_name: test1
          vm_system: linux
          label: test1
          vm_ip: 10.150.50.2
  ```
</details>

- Deploying multiple virtual machines at once:
  <details>
    <summary>yaml configuration</summary>

  ```yaml
  deployment:
    host_1:
      provider:
        vagrant:
          enabled: true
          vagrantfile_path: /tmp/wazuh_qa_ctl
          vagrant_box: qactl/centos_8
          vm_memory: 1024
          vm_cpu: 1
          vm_name: test2
          vm_system: linux
          label: test2
          vm_ip: 10.150.50.10
    host_2:
      provider:
        vagrant:
          enabled: true
          vagrantfile_path: /tmp/wazuh_qa_ctl
          vagrant_box: my_custom_box
          vm_memory: 2048
          vm_cpu: 2
          vm_name: test3
          vm_system: windows
          label: test3
          vm_ip: 10.150.50.11
  ```
</details>

> **Important note**: The `vm_name` can not be repeated, every deployed instance has to have a different name. The IP declared in the field `vm_ip` cannot be used by two VMs at the same time.

### Provision section

This yaml section has three different sub-sections: `host_info`, `wazuh_deployment` and `qa_framework`

#### Host info section
This section contains the necessary information for being able to make a connection with the host where the provisioning stage is going to be made. The `host_info` section is always required and its absence will make the `qa-ctl` validation parameters stage fail.

<details>
    <summary>yaml configuration</summary>

  ```yaml
    host_info:
      ansible_connection: ssh # Or winrm for windows virtual machines
      ansible_user: vagrant
      ansible_password: vagrant
      ansible_port: 22
      ansible_python_interpreter: /usr/bin/python3
      system: deb # This field changes depending on the system of the VM
      installation_files_path: /tmp
      host: 10.150.50.4 # IP from the VM

  ```
</details>


#### Wazuh deployment section


This section handles all the info needed for provisioning the VM host with `Wazuh manager or agent`. There are several ways to install Wazuh: using an `S3 URL`, from a `local downloaded package`, from the `file sources` available on the `wazuh repository` and by using a specific `system`, `version`, `revision` and `repository`.


- Provisioning an instance with `Wazuh manager` using an `S3 URL`:

  <details>
    <summary>yaml configuration</summary>

  ```yaml
    wazuh_deployment:
      type: package
      target: manager
      s3_package_url: https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-manager/wazuh-manager_4.2.3-1_amd64.deb
      installation_files_path: /tmp
      wazuh_install_path: /var/ossec # This is an optional field
      health_check: true # This is an optional field
  ```
</details>

- Provisioning an instance with a `Wazuh manager` using a `local existent package`:

  <details>
    <summary>yaml configuration</summary>

  ```yaml
    wazuh_deployment:
      type: package
      target: manager
      local_package_path: /tmp/wazuh-manager_4.2.3-1_amd64.deb
      installation_files_path: /tmp
  ```
</details>

- Provisioning an instance with a `Wazuh manager` obtained from the `sources of the Wazuh repository`:

  <details>
    <summary>yaml configuration</summary>

  ```yaml
    wazuh_deployment:
      type: sources
      target: manager
      wazuh_branch: master # Will be used for obtaining the wazuh installation files
      installation_files_path: /tmp
  ```
</details>


- Provisioning an instance with a `Wazuh manager` using a specific `system`, `version`, `revision` and `repository`:

  <details>
    <summary>yaml configuration</summary>

  ```yaml
    wazuh_deployment:
      type: package
      target: manager
      system: deb
      version: 4.2.4
      revision: 0.10557
      repository: test
      installation_files_path: /tmp
  ```
</details>

- Provisioning an instance with `Wazuh agent`:

  <details>
    <summary>yaml configuration</summary>

  ```yaml
    wazuh_deployment:
      type: package
      target: agent
      manager_ip: 10.150.50.2
      s3_package_url: https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-manager/wazuh-agent.2.3-1_amd64.deb
      installation_files_path: /tmp
  ```
</details>

> **Important note**: For provisioning a host with a Wazuh agent, the `target` field needs to be changed to `agent` and there will be a new field required called `manager_ip` that indicates the IP of the manager that the new agent will be connected with.

#### QA framework section

This section is used for provisioning the host with the Framework of QA. The example given below is a generic one that will use the `master` branch for getting the repository files, and locate them in the path specified in the `qa_workdir` field.

  <details>
    <summary>yaml configuration</summary>

  ```yaml
    qa_framework:
      wazuh_qa_branch: master
      qa_workdir: /tmp/wazuh_qa_ctl
  ```
</details>

### Testing section
The Testing is composed with an always required [`host_info`](#host-info-section) section that contains the same fields as the `Provisioning` stage `host_info` section. Plus, there is a section called `test` with the fields needed for launching a test.

- Run a pytest module test called `test_cors`
   <details>
    <summary>yaml configuration</summary>

  ```yaml
    test:
      type: pytest # As for now, this is the only available type
      path:
        test_files_path: /wazuh-qa/tests/integration/test_api/test_config/test_cors/test_cors.py # Full location path of the test 
        run_tests_dir_path: /wazuh-qa/test/integration                                           # Path to the folder where to run the tests
        test_results_path: /tmp/wazuh_qa_ctl/test_general_settings_enabled_resutls/              # Path where the results of the tests will be stored
      wazuh_install_path: /var/ossec
      systen: linux
      component: manager
      modules:
      - api
  ```
</details>

> **Note**: The fields `test_files_path` and `run_test_dir_path` are paths that are going to be used in the VM instance, whereas the `test_results_path` is a path that is going to be used in the host machine where qa-ctl was launched.


### Full YAML configuration files examples
Here you can find some examples of YAML configuration files that are fully completed on every section and ready to use.

- Run  a pytest test module called `test cache`. This test aims for Wazuh manager.
   <details>
    <summary>yaml configuration</summary>

  ```yaml
    deployment:
      host_1:
        provider:
          vagrant:
            enabled: true
            vagrantfile_path: /tmp/wazuh_qa_ctl
            vagrant_box: qactl/ubuntu_20_04
            vm_memory: 1024
            vm_cpu: 1
            vm_name: manager_test_cache_1635415018.925661
            vm_system: linux
            label: manager_test_cache_1635415018.925661
            vm_ip: 10.150.50.2
    provision:
      hosts:
        host_1:
          host_info:
            ansible_connection: ssh
            ansible_user: vagrant
            ansible_password: vagrant
            ansible_port: 22
            ansible_python_interpreter: /usr/bin/python3
            system: deb
            installation_files_path: /tmp
            host: 10.150.50.6
          wazuh_deployment:
            type: package
            target: manager
            s3_package_url: https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-manager/wazuh-manager_4.2.4-1_amd64.deb
            installation_files_path: /tmp
            health_check: true
          qa_framework:
            wazuh_qa_branch: 2023-qa-ctl-documented-test-validation
            qa_workdir: /tmp/wazuh_qa_ctl
    tests:
      host_1:
        host_info:
          ansible_connection: ssh
          ansible_user: vagrant
          ansible_password: vagrant
          ansible_port: 22
          ansible_python_interpreter: /usr/bin/python3
          system: deb
          installation_files_path: /tmp
          host: 10.150.50.6
        test:
          type: pytest
          path:
            test_files_path: /tmp/wazuh_qa_ctl/wazuh-qa/tests/integration/test_api/test_config/test_cache/test_cache.py
            run_tests_dir_path: /tmp/wazuh_qa_ctl/wazuh-qa/tests/integration
            test_results_path: /tmp/wazuh_qa_ctl/test_cache_1635415018.92588
          wazuh_install_path: /var/ossec
          system: linux
          component: manager
          modules:
          - api

  ```
</details>




- Run a pytest test called `test execd restart`. This test is designed for testing a `Wazuh agent`. For this purpose, `qa-ctl` will generate two VMs, one of them will be provisioned with `wazuh manager` and the other will be provisioned with `wazuh agent`.
   <details>
    <summary>yaml configuration</summary>

  ```yaml
    deployment:
      host_1:
        provider:
          vagrant:
            enabled: true
            vagrantfile_path: /tmp/wazuh_qa_ctl
            vagrant_box: qactl/ubuntu_20_04
            vm_memory: 1024
            vm_cpu: 1
            vm_name: agent_test_execd_restart_1635415139.489418
            vm_system: linux
            label: agent_test_execd_restart_1635415139.489418
            vm_ip: 10.150.50.3
      host_2:
        provider:
          vagrant:
            enabled: true
            vagrantfile_path: /tmp/wazuh_qa_ctl
            vagrant_box: qactl/ubuntu_20_04
            vm_memory: 1024
            vm_cpu: 1
            vm_name: manager_test_execd_restart_1635415139.489418
            vm_system: linux
            label: manager_test_execd_restart_1635415139.489418
            vm_ip: 10.150.50.4
    provision:
      hosts:
        host_1:
          host_info:
            ansible_connection: ssh
            ansible_user: vagrant
            ansible_password: vagrant
            ansible_port: 22
            ansible_python_interpreter: /usr/bin/python3
            system: deb
            installation_files_path: /tmp
            host: 10.150.50.3
          wazuh_deployment:
            type: package
            target: agent
            s3_package_url: https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.2.4-1_amd64.deb
            installation_files_path: /tmp
            health_check: true
            manager_ip: 10.150.50.4
          qa_framework:
            wazuh_qa_branch: 2023-qa-ctl-documented-test-validation
            qa_workdir: /tmp/wazuh_qa_ctl
        host_2:
          host_info:
            ansible_connection: ssh
            ansible_user: vagrant
            ansible_password: vagrant
            ansible_port: 22
            ansible_python_interpreter: /usr/bin/python3
            system: rpm
            installation_files_path: /tmp
            host: 10.150.50.4
          wazuh_deployment:
            type: package
            target: manager
            s3_package_url: https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-manager/wazuh-manager_4.2.4-1_amd64.deb
            installation_files_path: /tmp
            health_check: true
          qa_framework:
            wazuh_qa_branch: 2023-qa-ctl-documented-test-validation
            qa_workdir: /tmp/wazuh_qa_ctl
    tests:
      host_1:
        host_info:
          connection_method: ssh
          user: vagrant
          password: vagrant
          connection_port: 22
          ansible_python_interpreter: /usr/bin/python3
          system: deb
          installation_files_path: /tmp
          host: 10.150.50.3
        test:
          type: pytest
          path:
            test_files_path: /tmp/wazuh_qa_ctl/wazuh-qa/tests/integration/test_active_response/test_execd/test_execd_restart.py
            run_tests_dir_path: /tmp/wazuh_qa_ctl/wazuh-qa/tests/integration
            test_results_path: /tmp/wazuh_qa_ctl/test_execd_restart_1635415139.489715


  ```
</details>