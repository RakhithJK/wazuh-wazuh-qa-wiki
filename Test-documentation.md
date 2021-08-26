## Documenting tests using the `DocGenerator` tool

### Introduction

This section describes the steps to add the documentation code to the Python modules containing the developed tests. From this code, it will be possible to generate the documentation in different formats through `DocGenerator` tool. Thanks to this solution, it will be possible to standardize the documentation for all tests and, at the same time, fit the project's requirements.


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
    Integration

description: 
    These tests will check if, during enrollment, the agent re-establishes communication with the manager
    under different situations that interrupt it.
    The objective is to check that, with different states in the clients.key file, the agent
    successfully enrolls after losing connection with remoted.

tiers: 
    - 0

component: 
    Agent

path:
    tests/integration/test_agentd/

daemons:
    - wazuh-agentd

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