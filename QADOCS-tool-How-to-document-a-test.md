# Introduction

This section describes the steps to add the documentation code to the Python modules containing the developed tests. From this code, it will be possible to generate the documentation in different formats through the **qa-docs** tool. Thanks to this solution, it will be possible to standardize the documentation for all tests and, at the same time, fit the project's requirements.

To build the documentation, `qa-docs` uses the schema defined in the issue: [QADOCS schema 2.0](https://github.com/wazuh/wazuh-qa/issues/1694). Below is a description of how such a schema is structured and how to use it to document tests.


&nbsp;


# Documentation structure

The documentation code is organized in two parts:
  - **Module block**: It will be placed at the top of the Python module, and it will contain the common information for all the tests that may be in that module. 
  - **Test block**: It will be located just after the definition of each test, before its code, and will contain the information related to that test. 

Here is an example of how the `test_basic_usage_changes.py` from `test_fim` is documented:


<details><summary>Show documentation code</summary>
<p>

```python
# Module block
'''
copyright: Copyright (C) 2015-2021, Wazuh Inc.

           Created by Wazuh, Inc. <info@wazuh.com>.

           This program is free software; you can redistribute it and/or modify it under the terms of GPLv2

type: integration

brief: File Integrity Monitoring (FIM) system watches selected files and triggering alerts when
       these files are modified. In particular, these tests will check if common operations
       ('add', 'modify', and 'delete') on monitored directories are correctly detected.
       The FIM capability is managed by the 'wazuh-syscheckd' daemon, which checks configured
       files for changes to the checksums, permissions, and ownership.

tier: 0

modules:
    - fim

components:
    - agent
    - manager

daemons:
    - wazuh-syscheckd

os_platform:
    - linux
    - windows
    - macos
    - solaris

os_version:
    - Arch Linux
    - Amazon Linux 2
    - Amazon Linux 1
    - CentOS 8
    - CentOS 7
    - CentOS 6
    - Ubuntu Focal
    - Ubuntu Bionic
    - Ubuntu Xenial
    - Ubuntu Trusty
    - Debian Buster
    - Debian Stretch
    - Debian Jessie
    - Debian Wheezy
    - Red Hat 8
    - Red Hat 7
    - Red Hat 6
    - Windows 10
    - Windows 8
    - Windows 7
    - Windows Server 2019
    - Windows Server 2016
    - Windows Server 2012
    - Windows Server 2003
    - Windows XP
    - macOS Catalina
    - Solaris 10
    - Solaris 11

references:
    - https://documentation.wazuh.com/current/user-manual/capabilities/file-integrity/index.html
    - https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/syscheck.html

pytest_args:
    - fim_mode:
        realtime: Enable real-time monitoring on Linux (using the 'inotify' system calls) and Windows systems.
        whodata: Implies real-time monitoring but adding the 'who-data' information.
    - tier:
        0: Only level 0 tests are performed, they check basic functionalities and are quick to perform.
        1: Only level 1 tests are performed, they check functionalities of medium complexity.
        2: Only level 2 tests are performed, they check advanced functionalities and are slow to perform.

tags:
    - fim_basic_usage
'''
.
.
.
def test_regular_file_changes(folder, name, encoding, checkers, tags_to_apply,
                              get_configuration, configure_environment,
                              restart_syscheckd, wait_for_fim_start):
    '''
    description: Check if the 'wazuh-syscheckd' daemon detects regular file changes (add, modify, delete).
                 For this purpose, the test uses different character encodings in the names of the testing
                 directories and files and performs operations on them. Finally, it verifies that
                 the FIM events have been generated properly.

    wazuh_min_version: 4.2.0

    parameters:
        - folder:
            type: str
            brief: Path to the monitored testing directory.
        - name:
            type: str
            brief: Name used for the testing files.
        - encoding:
            type: str
            brief: Character encoding used for the directory and testing files.
        - checkers:
            type: dict
            brief: Syscheck checkers (check_all).
        - tags_to_apply:
            type: set
            brief: Run test if match with a configuration identifier, skip otherwise.
        - get_configuration:
            type: fixture
            brief: Get configurations from the module.
        - configure_environment:
            type: fixture
            brief: Configure a custom environment for testing.
        - restart_syscheckd:
            type: fixture
            brief: Clear the 'ossec.log' file and start a new monitor.
        - wait_for_fim_start:
            type: fixture
            brief: Wait for realtime start, whodata start, or end of initial FIM scan.

    assertions:
        - Verify that all FIM events are generated for the operations performed,
          and these contain all 'check_' fields specified in the configuration.

    input_description: A test case (ossec_conf) is contained in external YAML file (wazuh_conf.yaml)
                       which includes configuration settings for the 'wazuh-syscheckd' daemon and, it
                       is combined with the testing directories to be monitored defined in this module.

    expected_output:
        - r'.*Sending FIM event: (.+)$' (Initial scan when restarting Wazuh)
        - Multiple FIM events logs of the monitored directories.

    tags:
        - scheduled
        - time_travel
    '''
```
</p>
</details>


&nbsp;


# Schema blocks

The below tables show the fields allowed for the blocks discussed above, along with the data type and example values for these fields:

## Module block

| Name | Type | Requirement | Description | Example case |
|:-:|:-:|:-:|:-:|:-:|
| copyright    | String | Mandatory | Module copyright.                                                | Copyright (C) 2015-2021...                                                         |
| type         | String | Mandatory | Type of tests included in the module (**predefined list**).      | integration, system, ...                                                           |
| brief        | String | Mandatory | Overview of what the module does.   	                       | Checks the components involved in feed management of Vulnerability Detector module |
| tier         | int    | Mandatory | Tier covered by the module.                                      | 0, 1, 2, ...                                                                       |
| modules      | List   | Mandatory | Modules tested (**predefined list**).                            | vulnerability_detector, api, active_response, ...                                  |
| components   | List   | Mandatory | Wazuh components used by the module (**predefined list**).       | agent, manager                                                                     |
| path         | String | Auto      | Relative path to the module.                                     | tests/integration/test_api/test_config/test_rbac/test_rbac_mode.py                 |
| daemons      | List   | Mandatory | Daemons running during the test (**predefined list**).           | wazuh-db, wazuh-modulesd, ...                                                      |
| os_platform  | List   | Mandatory | Platform where the tests should be run (**predefined list**).    | linux, windows, ...                                                                |
| os_version   | List   | Mandatory | Name and version of the operating system (**predefined list**).  | Ubuntu Trusty, Centos 5, Windows Server 2016, ...                                  |
| references   | List   | Optional  | URLs with additional information.                                | documentation, issues, ...                                                         |
| pytest_args  | List   | Optional  | List of dictionaries(name: value: brief) explaining the PyTest arguments that should be used when running the module. | **- fim_mode**: <br/> `realtime`: `Uses real-time file monitoring...`  <br/> `whodata`: `Implies real-time monitoring but...` |
| tags         | List   | Optional  | Labels to help identify the module (**predefined list**).        | nvd, feeds, rbac, ...                                                              |


&nbsp;


## Test block

| Name | Type | Requirement | Description | Example case |
|:-:|:-:|:-:|:-:|:-:|
| description       | String | Mandatory | The main description of what the test does.             | Check if vulnerability detector behaves as expected when importing...                             |
| wazuh_min_version | String | Mandatory | Wazuh minimal version (**predefined list**).            | 4.2.0                                                                                               |
| parameters        | List   | Mandatory  | List of dictionaries(name: type, brief) that describe the test parameters. | **configure_environment**: <br/> type: `fixture` <br/> brief: `Configure a custom environment for testing.` |
| assertions        | List   | Mandatory | A list of what the module checks.                       | Verify that the user-role relationship is removed.                                                |
| inputs            | List   |  Auto/Manual | Automatically or manually generated list of objects containing the data received by the test. | `- tags: - experimental_enabled configuration: experimental_features: true`, `- tags: - experimental_disabled configuration: experimental_features: false` |
| input_description | String | Mandatory | Description of the data received by the test.           | Different test cases are contained in an external `YAML` file (conf.yaml) which includes API configuration parameters (IPs and ports). |
| expected_output   | List   | Mandatory | List of strings that the test expects to find in log files or other objects for a successful finish. | `r'.*Sending FIM event: (.+)$' (Initial scan when restarting Wazuh)` |
| tags              | List   | Optional  | Labels to help identify the test (**predefined list**). | active_response, dos_attack, ...                                                                   |


&nbsp;


### Remarks

There are two fields whose `requirement` field contains the value `auto`:

- **path**: This field does not need to be added, since it will be automatically generated by `DocGenerator` when building the documentation. 
- **inputs**: Unlike the previous one, the content of this field can be generated automatically or manually. If this field is missing in the documentation block of a test, `DocGenerator` will execute a dry run of `PyTest` to automatically find all the test cases of the test and append this information with the key specified. Instead, if this field exists in the test documentation block, its content will be used to build the documentation.

The `expected_output` field is structured as follows:

- `r'.*Sending FIM event: (.+)$' (Initial scan when restarting Wazuh)`.

The string containing the raw string (r'.*Sending...') indicates that this is the message that the test expects to be generated on the system. If additional description is desired, it is written in parenthesis next to the raw string (Initial scan when...).


&nbsp;


## Pre-defined values

To avoid duplicating the content of some fields with slightly different values (use of capital letters, hyphens, etc.), certain fields should only include values that are previously defined:

| Field name | Allowed values |
|:-:|:-:|
| type               | `integration`, `performance`, `system`, `unit` |
| modules            | `active_response`, `agentd`, `analysisd`, `api`, `authd`, `cluster`, `fim`, `gcloud`, `github`, `logcollector`, `logtest`, `office365`, `remoted`, `rids`, `rootcheck`, `vulnerability_detector`, `wazuh_db`, `wpk` |
| os_platform        | `aix`, `linux`, `hp-ux`, `macos`, `solaris`, `windows` |
| os_version         | `Amazon Linux 1`, `Amazon Linux 2`, `Arch Linux`, `Debian Buster`, `Debian Stretch`, `Debian Jessie`, `Debian Wheezy`, `CentOS 6`, `CentOS 7`, `CentOS 8`, `Fedora 31`, `Fedora 32`, `Fedora 33`, `Fedora 34`, `openSUSE 42`, `Oracle 6`, `Oracle 7`, `Oracle 8`, `Red Hat 6`, `Red Hat 7`, `Red Hat 8`, `Solaris 10`, `Solaris 11`, `SUSE 12`, `SUSE 13`, `SUSE 14`, `SUSE 15`, `Ubuntu Bionic`, `Ubuntu Trusty`, `Ubuntu Xenial`, `Ubuntu Focal`, `macOS Server`, `macOS Catalina`, `macOS Sierra`, `Windows XP`, `Windows 7`, `Windows 8`, `Windows 10`, `Windows Server 2003`, `Windows Server 2012`, `Windows Server 2016`, `Windows Server 2019` |
| components         | `agent`, `manager` |
| daemons            | `ossec-agentd`, `ossec-agentlessd`, `ossec-analysisd`, `ossec-authd`, `ossec-csyslogd`, `ossec-dbd`, `ossec-execd`, `ossec-integratord`, `ossec-logcollector`, `ossec-maild`, `ossec-monitord`, `ossec-remoted`, `ossec-reportd`, `ossec-syscheckd`, `wazuh-agentd`, `wazuh-agentlessd`, `wazuh-analysisd`, `wazuh-authd`, `wazuh-csyslogd`, `wazuh-apid`, `wazuh-clusterd`, `wazuh-db`, `wazuh-dbd`, `wazuh-execd`, `wazuh-integratord`, `wazuh-logcollector`, `wazuh-maild`, `wazuh-monitord`, `wazuh-modulesd`, `wazuh-remoted`, `wazuh-reportd`, `wazuh-syscheckd` |
| wazuh_min_version  | `2.1.0`, `3.0.0`, `3.1.0`, `3.2.0`, `3.3.0`, `3.4.0`, `3.5.0`, `3.6.0`, `3.7.0`, `3.8.0`, `3.9.0`, `3.10.0`, `3.11.0`, `3.12.0`, `3.13.0`, `4.0.0`, `4.1.0`, `4.2.0`, `4.3.0` |
| tags               | `active_response`, `agentd`, `alerts`, `analysisd`, `api`, `ar_analysisd`, `ar_execd`, `auditd`, `audit_keys`, `audit_rules`, `authd`, `aws`, `brute_force_attack`, `cache`, `cluster`, `cors`, `cpe`, `diff`, `disk_quota`, `dos_attack`, `download`, `enrollment`, `errors`, `events`, `experimental`, `feeds`, `fernet`, `fim`, `fim_ambiguous_confs`, `fim_audit`, `fim_basic_usage`, `fim_benchmark`, `fim_checks`, `fim_env_variables`, `fim_file_limit`, `fim_follow_symbolic_link`, `fim_ignore`, `fim_inotify`, `fim_invalid`, `fim_max_eps`, `fim_max_files_per_second`, `fim_moving_files`, `fim_multiple_dirs`, `fim_nodiff`, `fim_prefilter_cmd`, `fim_report_changes`, `fim_process_priority`, `fim_recursion_level`, `fim_restrict`, `fim_scan`, `fim_skip`, `fim_stats_integrity_sync`, `fim_tags`, `fim_timezone_changes`, `fim_wildcards_complex`, `fim_windows_audit_interval`, `fim_registry_ambiguous_confs`, `fim_registry_basic_usage`, `fim_registry_checks`, `fim_registry_ignore`, `fim_registry_nodiff`, `fim_registry_file_limit`, `fim_registry_multiple_registries`, `fim_registry_recursion_level`, `fim_registry_report_changes`, `fim_registry_restrict`, `fim_registry_tags`, `fim_synchronization`, `gcloud`, `gcloud_configuration`, `gcloud_functionality`, `github`, `github_configuration`, `integrity`, `invalid_settings`, `keys`, `key_polling`, `logcollector`, `logcollector_age`, `logcollector_cmd_exec`, `logcollector_configuration`, `logcollector_keep_running`, `logcollector_location`, `logcollector_location_cust_sockets`, `logcollector_log_format`, `logcollector_macos`, `logcollector_only_future_events`, `logcollector_options`, `logcollector_reconnect_time`, `logcollector_statistics`, `logs`, `logtest`, `man_in_the_middle`, `master`, `mitre`, `msu`, `nvd`, `office365_configuration`, `office365`, `oval`, `prelink`, `rbac`, `realtime`, `remoted_active_response`, `remoted`, `rids`, `rootcheck`, `rules`, `scan`, `scheduled`, `settings`, `simulator`, `ssl`, `stats_file`, `time_travel`, `token`, `vulnerability`, `vulnerability_detector`, `wazuh_db`, `wdb_socket`, `who_data`, `worker`, `wpk` |
