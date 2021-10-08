## Supported OS

**This tool must be run on a linux host system**. We are working to add support for Windows and macOS systems.


## Required dependencies

In order to install `qa-ctl` tool, you must have the following dependencies installed:

**System dependencies**

- `Build-Essentials`: [How to install](https://howtoprogram.xyz/2016/06/28/install-build-essentials-for-centos-rhel-and-ubuntu/)
- `Python` (>=3.6.0): [How to install](https://docs.python-guide.org/starting/install3/linux/)
- `Python-pip` (>=21.2.4): [How to install](https://www.tecmint.com/install-pip-in-linux/)
- `Python3-devtools`: [How to install](https://py-generic-project.readthedocs.io/en/latest/installing.html)
- `VirtualBox` (>=6.0.18): [How to install](https://www.virtualbox.org/wiki/Linux_Downloads)
- `Vagrant` (>=2.2.6): [How to install](https://www.vagrantup.com/docs/installation)
- `sshpass` (>=1.0.6): [How to install](https://www.tecmint.com/sshpass-non-interactive-ssh-login-shell-script-ssh-password/)


## How to install

1. Install and check the necessary dependencies (mentioned above).

2. Download the `wazuh-qa` repository, install python dependencies and the `wazuh-qa` framework

```bash
git clone https://github.com/wazuh/wazuh-qa --depth 1 --branch=master $(pwd)/wazuh-qa && \
cd wazuh-qa* && \
python3 -m pip install --upgrade pip && \
python3 -m pip install -r requirements.txt --no-cache-dir --upgrade --only-binary=:cryptography,grpcio: --ignore-installed && \
cd deps/wazuh_testing && \
python3 setup.py install
```

3. Check `qa-ctl` command tool

```
$ qa-ctl -h
```
