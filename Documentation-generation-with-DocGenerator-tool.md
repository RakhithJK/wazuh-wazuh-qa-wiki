### Basic use of `DocGenerator.py`

#### Setup

This tool is found in `wazuh-qa/docs/DocGenerator/`, and to run it, you must first install the requirements found in the `requirements.txt` file in the same directory:

```
pip install -r requirements.txt
```

The `DocGenerator` configuration can be found in the `config.yaml` file. This already contains a pre-established configuration for generating test documentation. The settings to be considered when creating new documentation are as follows:

```yaml
Project path: String with the project path

Output path: String with the path where you want to locate a folder with the doc parsed

Include paths: List of folders to parse 

Include regex: List of regular expressions to parse specific files

Group files: List of group files

Function regex: List of regular expressions to parse specific functions

Ignore paths: List of folders to ignore

Output fields:
  Module:
    Mandatory: List of mandatory fields
    Optional: List of optional fields
  Test:
    Mandatory: List of mandatory fields
    Optional: List of optional fields

Test cases field: Key to identify a Test Case list
```

  - Section `Include paths`: If they do not exist, the routes to the tests to be developed/migrated will be added here. 
  - Section `Ignore paths`: This section contains paths to directories found in the `Include paths` hierarchy but does not contain tests with documentation, i. e. `data` folders.


For more details about the configuration options see `README.md`.

<details><summary>`config.yaml` example</summary>

```yaml
Project path: "../../tests/integration"

Output path: "../output"

Include paths:
  - "../../tests/integration/test_wazuh_db"
  - "../../tests/integration/test_vulnerability_detector"

Include regex:
  - "^test_.*py$"

Group files:
  - "README.md"

Function regex:
  - "^test_"

Ignore paths:
  - "../../tests/integration/test_wazuh_db/data"

Output fields:
  Module:
    Mandatory:
      - brief
      - metadata:
        - modules
        - daemons
        - component
        - operating_system
        - tiers
    Optional:
      - tags
  Test:
    Mandatory:
      - test_logic
      - checks
    Optional:
      - expected_result
      - test_cases

Test cases field: test_cases

```

</details>

---

#### Build documentation

usage: `DocGenerator.py [-h] [-s] [-v] [-t] [-d] [-i INDEX_NAME] [-l LAUNCH_INDEX_NAME]`

optional arguments:
 - `-h`, `--help`     show this help message and exit
 - `-s`             Run a sanity check
 - `-v`             Print version
 - `-t`             Test configuration
 - `-d`             Enable debug messages.
 - `-i` INDEX_NAME  Indexes the data to elasticsearch.
 - `-l` LAUNCH_INDEX_NAME  Indexes the data and launch the application.

When you run it using `-i INDEX_NAME`, the JSONs previously generated are indexed in elasticsearch. You can index them and run SearchUI simultaneously using `-l INDEX_NAME`. If you want to generate the doc, you need to use a `config.yaml` file that is used when you parse the tests.



To generate the documentation in `YAML` and `JSON` formats, just run the tool without arguments:
```
python3 DocGenerator.py
```

The documentation is generated in `Output path` from `config.yaml`.

---

## Examples of documentation generated

- vulnerability detector

<details><summary>test_scan_nvd_feed.json</summary>

```json
{
    "copyright": "Copyright (C) 2015-2021, Wazuh Inc.\nCreated by Wazuh, Inc. <info@wazuh.com>.\nThis program is free software; you can redistribute it and/or modify it under the terms of GPLv2",
    "type": "Integration",
    "description": "These tests will mock RedHat, Canonical, Debian, and Windows systems, and insert custom vulnerabilities and vulnerable packages to check if Vulnerability Detector generates the vulnerability alerts from NVD feed.",
    "tiers": [
        0
    ],
    "component": "Server",
    "platform": [
        "Linux, RHEL5",
        "Linux, RHEL6",
        "Linux, RHEL7",
        "Linux, RHEL8",
        "Linux, Amazon Linux 1",
        "Linux, Amazon Linux 2",
        "Linux, Debian BUSTER",
        "Linux, Debian STRETCH",
        "Linux, Debian WHEEZY",
        "Linux, Ubuntu BIONIC",
        "Linux, Ubuntu XENIAL",
        "Linux, Ubuntu TRUSTY",
        "Linux, Arch Linux"
    ],
    "checks": [
        "There are as many NVD alerts as vulnerable packages.",
        "There are 0 NVD vulnerability alerts for RedHat provider.",
        "The alerts are produced by the NVD provider."
    ],
    "references": [
        "https://documentation.wazuh.com/current/user-manual/capabilities/vulnerability-detection/index.html"
    ],
    "tags": [
        "linux",
        "modulesd",
        "vulnerability_detector"
    ],
    "name": "test_scan_nvd_feed.py",
    "id": 16,
    "group_id": 2,
    "tests": [
        {
            "description": "Check if inserted vulnerable packages are reported by vulnerability detector using the NVD feed.",
            "tier": 0,
            "min_version": 4.1,
            "parameters": [
                "get_configuration (fixture), Get configurations from the module.",
                "configure_environment (fixture), Configure a custom environment for testing.",
                "restart_modulesd (fixture), Restart the wazuh-modulesd daemon.",
                "check_cve_db (fixture), Check if the CVE database exists and its tables are created",
                "mock_vulnerability_scan (fixture), Mock the vulnerability scan inserting custom packages, feeds and changing the host system."
            ],
            "use_cases": "Several vulnerable packages with their respective vulnerabilities are inserted into a simulated agent to generate the corresponding alerts from NVD feed.",
            "expected_output": [
                "r\"Agent '000' is vulnerable to '{cve}'. Condition, '{patch} patch is not installed.'\" (if agent OS is Windows).",
                "r\"The '{package}' package .* from agent .* is vulnerable to '{cve}'\" (for no Windows agents).",
                "r\"The NVD found a total of '{vulnerabilities_number}' potential vulnerabilities for agent .*\""
            ],
            "tags": [
                "nvd",
                "cve"
            ],
            "name": "test_vulnerabilities_report",
            "test_cases": [
                "scan_nvd_configuration-RHEL8",
                "scan_nvd_configuration-RHEL7",
                "scan_nvd_configuration-RHEL6",
                "scan_nvd_configuration-RHEL5",
                "scan_nvd_configuration-BIONIC",
                "scan_nvd_configuration-XENIAL",
                "scan_nvd_configuration-TRUSTY",
                "scan_nvd_configuration-BUSTER",
                "scan_nvd_configuration-STRETCH"
            ]
        }
    ]
}
```
</details>

<details><summary>test_general_settings_ignore_time.json</summary>

```json
{
    "copyright": "Copyright (C) 2015-2021, Wazuh Inc.\nCreated by Wazuh, Inc. <info@wazuh.com>.\nThis program is free software; you can redistribute it and/or modify it under the terms of GPLv2",
    "type": "Integration",
    "description": "The tests will modify the value of ignore_time tag in ossec.conf, set different times and check the result in ossec.log.",
    "tiers": [
        0
    ],
    "component": "Server",
    "platform": [
        "Linux, RHEL5",
        "Linux, RHEL6",
        "Linux, RHEL7",
        "Linux, RHEL8",
        "Linux, Amazon Linux 1",
        "Linux, Amazon Linux 2",
        "Linux, Debian BUSTER",
        "Linux, Debian STRETCH",
        "Linux, Debian WHEEZY",
        "Linux, Ubuntu BIONIC",
        "Linux, Ubuntu XENIAL",
        "Linux, Ubuntu TRUSTY",
        "Linux, Arch Linux"
    ],
    "checks": [
        "Vulnerabilities alerts are not generated before ignore_time time set.",
        "Vulnerabilities alerts are generated after ignore_time time set."
    ],
    "references": [
        "https://documentation.wazuh.com/current/user-manual/capabilities/vulnerability-detection/index.html"
    ],
    "tags": [
        "linux",
        "modulesd",
        "wazuh-db",
        "vulnerability_detector"
    ],
    "name": "test_general_settings_ignore_time.py",
    "id": 5,
    "group_id": 2,
    "tests": [
        {
            "description": "Check if an alert is not fired during the ignore time  interval.",
            "tier": 0,
            "min_version": 4.1,
            "parameters": [
                "get_configuration (fixture), Get configurations from the module.",
                "configure_environment (fixture), Configure a custom environment for testing.",
                "restart_modulesd (fixture), Restart the wazuh-modulesd daemon.",
                "prepare_agent (fixture), Creates a mock agent with a vulnerability for testing purposes.",
                "custom_callback_vulnerability (lambda), Create a callback function from a text pattern."
            ],
            "use_cases": "Different time intervals are used in which alerts are to be ignored.",
            "expected_output": [
                "'{vd.DEFAULT_PACKAGE_NAME}'.+is vulnerable to '{vd.DEFAULT_VULNERABILITY_ID}'"
            ],
            "tags": [
                "time_travel"
            ],
            "name": "test_ignore_time",
            "test_cases": [
                "get_configuration0",
                "get_configuration1",
                "get_configuration2"
            ]
        }
    ]
}
```
</details>

- remoted

<details><summary> test_request_agent_info.json</summary>

```json
{
    "copyright": "Copyright (C) 2015-2021, Wazuh Inc.\nCreated by Wazuh, Inc. <info@wazuh.com>.\nThis program is free software; you can redistribute it and/or modify it under the terms of GPLv2",
    "type": "Integration",
    "description": "Check that manager-agent communication through remoted socket works as expected.",
    "tiers": [
        0
    ],
    "component": "Server",
    "platform": [
        "Linux, RHEL5",
        "Linux, RHEL6",
        "Linux, RHEL7",
        "Linux, RHEL8",
        "Linux, Amazon Linux 1",
        "Linux, Amazon Linux 2",
        "Linux, Debian BUSTER",
        "Linux, Debian STRETCH",
        "Linux, Debian WHEEZY",
        "Linux, Ubuntu BIONIC",
        "Linux, Ubuntu XENIAL",
        "Linux, Ubuntu TRUSTY",
        "Linux, Arch Linux"
    ],
    "checks": [
        "The getconfig request.",
        "The getstate request.",
        "The getconfig request for a disconnected agent."
    ],
    "references": [
        "https://documentation.wazuh.com/current/user-manual/reference/daemons/ossec-remoted.html"
    ],
    "tags": [
        "linux",
        "remoted"
    ],
    "name": "test_request_agent_info.py",
    "id": 67,
    "group_id": 47,
    "tests": [
        {
            "description": "Writes (config/state) requests in $DIR/queue/ossec/request and check if remoted forwards it to the agent, collects the response, and writes it in the socket or returns an error message if the queried agent is disconnected.",
            "tier": 0,
            "min_version": 4.2,
            "parameters": [
                "get_configuration (fixture), Get configurations from the module.",
                "configure_environment (fixture), Configure a custom environment for testing.",
                "remove_shared_files (fixture), Temporary removes txt files from default agent group shared files.",
                "restart_remoted (fixture), Restart the wazuh-remoted daemon.",
                "command_request (String), Use case being evaluated.",
                "expected_answer (String), The expected response from the agent to the manager."
            ],
            "use_cases": "Several requests that are made by the manager to the agent.",
            "expected_output": [
                "Cannot send request",
                "r\"{\"client\":{\"config-profile\":\"centos8\",\"notify_time\":10,\"time-reconnect\":60}}\"",
                "r\"{\"error\":0,\"data\":{\"global\":{\"start\":\"2021-02-26, 06:41:26\",\"end\":\"2021-02-26 08:49:19\"}}}\""
            ],
            "tags": [
                "agent_simulator"
            ],
            "name": "test_request",
            "test_cases": [
                "udp,tcp-disconnected",
                "udp,tcp-get_config",
                "udp,tcp-get_state",
                "tcp-disconnected",
                "tcp-get_config",
                "tcp-get_state",
                "udp-disconnected",
                "udp-get_config",
                "udp-get_state"
            ]
        }
    ]
}
```
</details>