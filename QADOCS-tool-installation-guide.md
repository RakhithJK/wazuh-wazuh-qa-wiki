## Supported OS
This tool has support for Linux and Windows(only parsing behavior tested) systems.

## Linux dependencies

### System dependencies

- `Build-Essentials`: [How to install](https://howtoprogram.xyz/2016/06/28/install-build-essentials-for-centos-rhel-and-ubuntu/)
- `Python` (>=3.6.0): [How to install](https://docs.python-guide.org/starting/install3/linux/)
- `Python-pip` (>=21.2.3): [How to install](https://www.tecmint.com/install-pip-in-linux/)
- `Python3-devtools`: [How to install](https://py-generic-project.readthedocs.io/en/latest/installing.html)
- `ElasticSearch`: [How to install](https://www.elastic.co/downloads/elasticsearch)
Also, you can:
```
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add - && \
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-7.x.list && \
apt update && \
apt install -y elasticsearch
```

Also, you need to specify the RAM limit:
```
echo "-Xms1g" >> /etc/elasticsearch/jvm.options
echo "-Xmx1g" >> /etc/elasticsearch/jvm.options
```

- `npm (>=6.14.4)`:

To install npm on Ubuntu, Debian, and Linux Mint:

```
$ sudo apt install npm
```

To install npm on CentOS 8 (and newer), Fedora, and Red Hat:
```
$ sudo dnf install npm	# also installs nodejs
```

To install npm on Arch Linux and Manjaro:

```
$ sudo pacman -S npm # also installs nodejs
```

## Windows dependencies

### System dependencies

- `Microsoft Visual C++ 14` (or higher versions) : [How to install](https://newbedev.com/how-to-install-visual-c-build-tools)
- `Git` (>=2.33) [How to install](https://github.com/git-guides/install-git#install-git-on-windows)
- `Python` (>=3.6.0): [How to install](https://realpython.com/installing-python/#how-to-install-from-the-full-installer)
- `Python-pip` (>=21.2.3): [How to install](https://www.liquidweb.com/kb/install-pip-windows/)
- `WSL2` : [How to install](https://github.com/wazuh/wazuh-qa/wiki/installing-wsl2-on-windows)
- `Docker Desktop`: [How to install](https://docs.docker.com/desktop/windows/install/#install-docker-desktop-on-windows)
- `ElasticSearch`: [How to install](https://www.elastic.co/downloads/elasticsearch)

For the RAM limit, add the following lines to config/jvm.options:
```
-Xms1g
-Xmx1g
```

- `npm (>=6.14.4)`: [How to install](https://nodejs.org/en/download/)

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
    python3 -m pip install -r deps/wazuh_testing/wazuh_testing/qa_docs/requirements.txt && \
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
    python -m pip install -r deps/wazuh_testing/wazuh_testing/qa_docs/requirements.txt
    cd deps\wazuh_testing
    python setup.py install
    ```

3. Check `qa-docs` command tool [use guide](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-use-guide) or run the following line:

```
$ qa-docs -h
```