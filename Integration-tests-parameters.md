# Integration tests parameters

The aim of this article is to show the necessary parameters to be able to launch all the integration tests of 
the `wazuh-qa` repository.

## Active response

- test_analysisd:
    - **Health-check**: ???
    - **Target**: manager
    - **Self-configured tests**: Yes   
    - **Custom configuration**:
        ``` 
        remoted.debug=2
        monitord.rotate_log=0
        ``` 

- test_execd:
    - **Health-check**: ???
    - **Target**: agent - Linux
    - **Self-configured tests**: Yes    
    - **Custom configuration**:
        ```
        monitord.rotate_log=0
        ```

---

## agentd

- **Health-check**: :red_circle:
- **Target**: agent - Linux, Windows
- **Self-configured tests**: Yes 
- **Custom configuration**:
    ```
    agent.debug=2
    execd.debug=2
    monitord.rotate_log=0
    ```

---

## analysisd

- **Health-check**: :green_circle:
- **Target**: manager
- **Self-configured tests**: No 
- **Custom configuration**:
    ```
    analysisd.debug=2
    monitord.rotate_log=0
    ```

---

## API

- **Health-check**: :red_circle:
- **Target**: manager
- **Self-configured tests**: ?? 
- **Custom configuration**:
    ```
    ???
    monitord.rotate_log=0
    ```

---

## authd
 
- **Health-check**: :green_circle:
- **Target**: manager
- **Self-configured tests**: No 
- **Custom configuration**:
    ```
    authd.debug=2 ????
    monitord.rotate_log=0
    ```

---

## cluster

- **Health-check**:  ???
- **Target**: manager
- **Self-configured tests**: ??
- **Custom configuration**:
    ```
    ????
    monitord.rotate_log=0
    ```

---

## FIM
These tests should be run using tiers 0,1 and 2 : `--tier 0 --tier 1 --tier 2` 

- Manager:
    - **Health-check**: ???
    - **Self-configured tests**: No
    - **Custom configuration**:
        ```
        syscheck.debug=2
        analysisd.debug=2
        monitord.rotate_log=0
        ```
    - **pytest_args**: `--fim_mode="realtime" --fim_mode="whodata"`
    
- Agent - Linux,Windows:
    - **Health-check**: ???
    - **Self-configured tests**: No
    - **Custom configuration**:
        ```
        agent.debug=2
        syscheck.debug=2
        monitord.rotate_log=0
        ```
    - **pytest_args**: `--fim_mode="realtime" --fim_mode="whodata"`

- Agent - macOS, Solaris:
    - **Health-check**: ???
    - **Self-configured tests**: No
    - **Custom configuration**:
        ```
        agent.debug=2
        syscheck.debug=2
        monitord.rotate_log=0
        ```

---

## gcloud

- **Health-check**: :red_circle:
- **Target**: manager or agent (manager preferred)
- **Self-configured tests**: No 
- **Custom configuration**:
  - `local_internal_options`:

    ``` 
    analysisd.debug=2
    wazuh_modules.debug=2
    monitord.rotate_log=0  
    ```
  - Add credentials to `tests/integration/test_gcloud/data/configuration.yaml` with the following template:

```
---
project_id: "your_project_id_here"
topic: "your_topic_name_here"
subscription: "your_subscription_name_here"
credential_path: 'gcp_credentials.json'
credentials: |
  {
  "credentials": "Your GCP credentials go here"
  }
```
---

## logcollector

- In development until 4.2.0 release

---

## logtest

- **Health-check**: :green_circle:
- **Target**: manager
- **Self-configured tests**: No 
- **Custom configuration**:
    ``` 
    analysisd.debug=2 
    ```

---

## remoted

- **Health-check**: :green_circle:
- **Target**: manager
- **Self-configured tests**: No 
- **Custom configuration**:
    ``` 
    remoted.debug=2
    wazuh_database.interval=1
    wazuh_db.commit_time=2
    wazuh_db.commit_time_max=3
    monitord.rotate_log=0
    ```

---

## RIDS

- **Health-check**: :green_circle:
- **Target**: manager
- **Self-configured tests**: Yes 
- **Custom configuration**: No

---

## rootcheck

- **Health-check**: :green_circle:
- **Target**: manager
- **Self-configured tests**: No 
- **Custom configuration**: No

---

## vulnerability_detector

- **Health-check**: :red_circle:
- **Target**: manager
- **Self-configured tests**: No 
- **Custom configuration**:
    ``` 
    wazuh_modules.debug=2
    monitord.rotate_log=0
    ```

---

## wazuh_db

- **Health-check**: :green_circle:
- **Target**: manager
- **Self-configured tests**: No 
- **Custom configuration**: No

---

## WPK

- **Health-check**: ???
- **Target**: agent and manager 
- **Self-configured tests**: No 
- **Custom configuration**: No
- **prerequisite**: Upload packages to `trash` repository
- **pytest_args**: `--wpk_version=<version>`