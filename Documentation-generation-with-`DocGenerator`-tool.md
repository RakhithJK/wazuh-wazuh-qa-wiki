### Basic use of `DocGenerator.py`

#### Setup

This tool is found in `wazuh-qa/docs/DocGenerator/`, and to run it, you must first install the requirements found in the `requirements.txt` file in the same directory:

```
pip install -r requirements.txt
```

The `DocGenerator` configuration can be found in the `config.yaml` file. This already contains a pre-established configuration for generating test documentation. For more details about the configuration options see `README.md`.


#### Build documentation

To make the documentation in `YAML` and `JSON` formats, just run the tool without arguments:
```
python3 DocGenerator.py
```

The documentation is found in the `output` directory, which will be created in the same place where `DocGenerator.py` is located. 
Example documentation for `test_wazuh_db.py`:

<details><summary>output/test_wazuh_db/test_wazuh_db.json</summary>
<p>

```json
{
    "brief": "Module description",
    "metadata": {
        "component": [
            "Manager"
        ],
        "modules": [
            "Wazuh DB"
        ],
        "daemons": [
            "wazuh_db"
        ],
        "operating_system": [
            "Windows",
            "Ubuntu"
        ],
        "tiers": [
            0,
            1
        ],
        "tags": [
            "Enrollment"
        ]
    },
    "name": "test_wazuh_db.py",
    "id": 2,
    "group_id": 1,
    "tests": [
        {
            "test_logic": "Check that every input message in wazuh-db socket generates the adequate output to wazuh-db socket",
            "name": "test_wazuh_db_messages"
        },
        {
            "test_logic": "Check that Wazuh DB creates the agent database when a query with a new agent ID is sent. Also...\nBut also...",
            "checks": [
                "The received output must match with...",
                "The received output with regex must match with..."
            ],
            "name": "test_wazuh_db_create_agent"
        }
    ]
}
```
</p>
</details>


<details><summary>output/test_wazuh_db/test_wazuh_db.json</summary>
<p>

```yml
brief: Module description
group_id: 1
id: 2
metadata:
  component:
  - Manager
  daemons:
  - wazuh_db
  modules:
  - Wazuh DB
  operating_system:
  - Windows
  - Ubuntu
  tags:
  - Enrollment
  tiers:
  - 0
  - 1
name: test_wazuh_db.py
tests:
- name: test_wazuh_db_messages
  test_logic: Check that every input message in wazuh-db socket generates the adequate
    output to wazuh-db socket
- checks:
  - The received output must match with...
  - The received output with regex must match with...
  name: test_wazuh_db_create_agent
  test_logic: 'Check that Wazuh DB creates the agent database when a query with a
    new agent ID is sent. Also...

    But also...'
```
</p>
</details>