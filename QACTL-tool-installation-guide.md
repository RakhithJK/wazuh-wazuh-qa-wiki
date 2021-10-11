
## Supported OS

This tool has support for Linux systems and Windows systems.


## Linux systems required dependencies

In order to install `qa-ctl` tool, you must have the following dependencies installed:

### System dependencies

- `Build-Essentials`: [How to install](https://howtoprogram.xyz/2016/06/28/install-build-essentials-for-centos-rhel-and-ubuntu/)
- `Python` (>=3.6.0): [How to install](https://docs.python-guide.org/starting/install3/linux/)
- `Python-pip` (>=21.2.3): [How to install](https://www.tecmint.com/install-pip-in-linux/)
- `Python3-devtools`: [How to install](https://py-generic-project.readthedocs.io/en/latest/installing.html)
- `Vagrant` (>=2.2.6): [How to install](https://www.vagrantup.com/docs/installation)
- `VirtualBox` (>=6.0.18): [How to install](https://www.virtualbox.org/wiki/Linux_Downloads)
- `Ansible` (>=3.1.0): [How to install](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- `sshpass` (>=1.0.6): [How to install](https://www.tecmint.com/sshpass-non-interactive-ssh-login-shell-script-ssh-password/)

## Windows systems required dependencies

### System dependencies
- `Microsoft Visual C++ 14` (or higher versions) : [How to install](https://newbedev.com/how-to-install-visual-c-build-tools)
- `Git` (>=2.33) [How to install](https://github.com/git-guides/install-git#install-git-on-windows)
- `Python` (>=3.6.0): [How to install](https://realpython.com/installing-python/#how-to-install-from-the-full-installer)
- `Python-pip` (>=21.2.3): [How to install](https://www.liquidweb.com/kb/install-pip-windows/)
- `WSL2` : [How to install](https://github.com/wazuh/wazuh-qa/wiki/installing-wsl2-on-windows)
- `Vagrant` (>=2.2.6): [How to install](https://www.vagrantup.com/docs/installation)
- `VirtualBox` (>=6.0.18): [How to install](https://www.virtualbox.org/manual/ch02.html#installation_windows)
- `Docker Desktop`: [How to install](https://docs.docker.com/desktop/windows/install/#wsl-2-backend)

## How to install qa-ctl

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

3. Check `qa-ctl` command tool

```
$ qa-ctl -h
```




