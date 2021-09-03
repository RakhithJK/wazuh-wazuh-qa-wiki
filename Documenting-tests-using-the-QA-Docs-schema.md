### Introduction

This section describes the steps to add the documentation code to the Python modules containing the developed tests. From this code, it will be possible to generate the documentation in different formats through `DocGenerator` tool. Thanks to this solution, it will be possible to standardize the documentation for all tests and, at the same time, fit the project's requirements.

DocGenerator uses the [third proposal](https://github.com/wazuh/wazuh-qa/issues/1694#issuecomment-906194448) from [QADOCS schema 2.0](https://github.com/wazuh/wazuh-qa/issues/1694), when we refine and get the definitive documentation schema, it will be updated.

&nbsp;


### Structure

The documentation code is organized in two parts:
  - **Module block**: It will be placed at the top of the Python module, and it will contain the common information for all the tests that may be in that module. 
  - **Test block**: It will be located just after the definition of each test, before its code, and will contain the information related to that test. 

Here is an example of the documentation code:
`test_agentd_reconnection.py`


<details><summary>Show documentation</summary>
<p>

```python

# Module block
'''
copyright:
    Copyright (C) 2015-2021, Wazuh Inc.

    Created by Wazuh, Inc. <info@wazuh.com>.

    This program is free software; you can redistribute it and/or modify it under the terms of GPLv2

type: 
    integration

description: 
    These tests will check if, during enrollment, the agent re-establishes communication with the manager
    under different situations that interrupt it.
    The objective is to check that, with different states in the clients.key file, the agent
    successfully enrolls after losing connection with remoted.

tiers: 
    - 0

component: 
    agent

path:
    tests/integration/test_agentd/

daemons:
    - agentd

os_support:
    - Linux, RHEL5
    - Linux, RHEL6
    - Linux, RHEL7
    - Linux, RHEL8
    - Linux, Amazon Linux 1
    - Linux, Amazon Linux 2
    - Linux, Debian BUSTER
    - Linux, Debian STRETCH
    - Linux, Debian WHEEZY
    - Linux, Ubuntu BIONIC
    - Linux, Ubuntu XENIAL
    - Linux, Ubuntu TRUSTY
    - Linux, Arch Linux
    - Windows, 7
    - Windows, 8
    - Windows, 10
    - Windows, Server 2003
    - Windows, Server 2012
    - Windows, Server 2016

coverage:
    33

tags:
    - linux
    - agentd
'''
.
.
.
def test_agentd_connection_retries_pre_enrollment(configure_authd_server, configure_environment, get_configuration):
    # Test block
    '''
    description: 
        Check how the agent behaves when losing communication with remoted and a new enrollment is sent to authd.
        For this, the agent starts without keys and perform multiple enrollment requests
        to authd before getting the new key to communicate with remoted.

    wazuh_min_version: 
        4.1

    parameters:
        - configure_authd_server (fixture), Initialize a simulated authd connection.
        - configure_environment (fixture), Configure a custom environment for testing.
        - get_configuration (fixture), Get configurations from the module.

    assertions:
        - The agent has keys, loses communication with remoted, and performs multiple enrollment requests.
    
    test_input:
        Requests are made using TCP and UDP protocols together with a empty client.keys file.

    logging:
        - ossec.log, "Sending keep alive:"
        - ossec.log, "Requesting a key" (four times)
        - ossec.log, "Valid key received"
        - ossec.log, "Sending keep alive:"

    tags:
        - remoted_simulator
    '''
```
</p>
</details>


&nbsp;


The below tables show the allowed fields for these blocks along with the data type and example values for these fields:
#### Module block

| Name | Type | Requirement | Description | Example case |
|:-:|:-:|:-:|:-:|:-:|
| copyright    | String | Mandatory | Module copyright                                                 | Copyright (C) 2015-2021...                                                         |
| type *       | String | Mandatory | Type of tests included in the module                             | integration                                                                        |
| description  | String | Mandatory | Overview of what the module does   	                       | Checks the components involved in feed management of Vulnerability Detector module |
| tiers        | List   | Mandatory | Tiers covered by the module                                      | 0, 1, 2                                                                            |
| component *  | String | Mandatory | Wazuh component used by the module (server/agent)                | server                                                                             |
| path         | String | Mandatory | Relative path to the test                                        | tests/integration/test_vulnerability_detector/test_scan_results/                   |
| daemons *    | List   | Mandatory | Daemons running during the test	                               | wazuh-db, modulesd                                                                 |
| os_support * | List   | Mandatory | List of pairs(os_name,os_version) that that identifies the operating system | Linux, Debian Buster                                                    |
| coverage     | Int    | Optional  | % coverage, represented as an integer                            | 33                                                                                 |
| pytest_args  | List   | Optional  | PyTest arguments that should be used to run the module.          | --fim_mode="realtime", --fim_mode="whodata"                                        |
| tags *       | List   | Optional  | Pre-defined labels to help identify the module                   | NVD, feeds, mock                                                                   |


#### Test block

| Name | Type | Requirement | Description | Example case |
|:-:|:-:|:-:|:-:|:-:|
| description       | String | Mandatory | The main description of what the test does   | Check if vulnerability detector behaves as expected when importing Debian OVAL feed with extra tags. |
| wazuh_min_version * | String | Mandatory | Wazuh minimal version                        | 4.1                                                                                                  |
| parameters        | List   | Optional  | List of pairs(name (type),brief) that describe the test parameters | type: fixture, brief: Modify the Debian OVAL feed, setting a test tag value.   |
| assertions        | List   | Mandatory | A list of what the module checks              | Feeds URL's, download, fields content, extra and missing tags                                       |
| test_input        | String | Mandatory | Input values evaluated by the test           | Multiple feeds in XML format with extra tags added.                                                  |       
| logging           | List   | Mandatory | List of pairs(file_name,message) with the output data the test expects | ossec.log, "INFO: \(\d+\): The update of the Debian Buster feed finished successfully." |
| tags *             | List   | Optional  | Pre-defined labels to help identify the test | debian                                                                                               |

#### Pre-defined values

To avoid duplicating the content of some fields with slightly different values (use of capital letters, hyphens, etc.), the fields marked with **(*)** accept a set of predefined values:

| Field name | Allowed values |
|:-:|:-:|
| type               | `integration`, `performance`, `system`, `unit` |
| os_support         | `linux, centos 5`; `linux, centos 6`; `linux, centos 7`; `linux, centos 8`; `linux, rhel5`; `linux, rhel6`; `linux, rhel7`; `linux, rhel8`; `linux, amazon linux 1`; `linux, amazon linux 2`; `linux, debian buster`; `linux, debian stretch`; `linux, debian wheezy`; `linux, ubuntu bionic`; `linux, ubuntu xenial`; `linux, ubuntu trusty`; `linux, arch linux`; `windows, 7`; `windows, 8`; `windows, 10`; `windows, server 2003`; `windows, server 2012`; `windows, server 2016`; `macos, catalina`; `macos, server` |
| component          | `agent`, `manager` |
| daemons            | `agentd`, `agentlessd`, `analysisd`, `authd`, `apid`, `clusterd`, `csyslogd`, `wazuh-db`, `dbd`, `execd`, `integratord`, `logcollector`, `maild`, `monitord`, `modules`, `remoted`, `reportd`, `syscheckd` |
| wazuh_min_version  | `2.1`, `3.0`, `3.1`, `3.2`, `3.3`, `3.4`, `3.5`, `3.6`, `3.7`, `3.8`, `3.9`, `3.10`, `3.11`, `3.12`, `3.13`, `4.0`, `4.1`, `4.2`, `4.3` |
| tags               | `active_response`, `alert`, `api`, `aws`, `brute_force_attack`, `cache`, `cors`, `cpe`, `dos_attack`, `download`, `enrollment`, `experimental`, `feeds`, `fim`, `gcloud`, `github`, `integrity`, `keys`, `logs`, `mitre`, `msu`, `nvd`, `oval`, `rbac`, `rules`, `scan`, `simulator`, `ssl`, `stats_file`, `vulnerability` |
