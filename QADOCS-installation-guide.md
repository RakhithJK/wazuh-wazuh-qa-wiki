## Supported OS
This tool has support for Linux and Windows(only parsing behavior tested) systems.

## Linux dependencies

### System dependencies

## Windows dependencies

### System dependencies

## How to install qa-docs

1. Install and check the necessary dependencies for your current OS (mentioned above).

2. Download the `wazuh-qa` repository, install python dependencies and the `wazuh-qa` framework

- For Linux

    In `Linux Terminal`, run the next commands:
    ```bash
    wget https://github.com/wazuh/wazuh-qa/archive/refs/heads/master.zip && \
    unzip master.zip && \
    rm master.zip && \
    cd wazuh-qa* && \
    python3 -m pip install --upgrade pip && \
    python3 -m pip install -r requirements.txt --no-cache-dir --upgrade --only-binary=:cryptography,grpcio: --ignore-installed && \
    cd deps/wazuh_testing && \
    python3 setup.py install
    ```
- For Windows

    Open `Windows Powershell` and run the next commands:
    ```bash
    git clone https://github.com/wazuh/wazuh-qa --depth 1 --branch=master
    cd wazuh-qa*
    python -m pip install --upgrade pip
    python -m pip install -r requirements.txt --no-cache-dir --upgrade --only-binary=:cryptography,grpcio: --ignore-installed
    cd deps\wazuh_testing
    python setup.py install
    ```

3. Check `qa-docs` command tool [use guide](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-use-guide) or run the following line:

```
$ qa-docs -h
```