# Installing WSL2 on Windows


`WSL 2` is a new version of the Windows Subsystem for Linux architecture that enables the Windows Subsystem for Linux to run Linux binaries on Windows. This tool is needed by `qa-ctl` for integrating Ansible in Windows, as there isn't any native support for this tool in Windows. We are using `WSL2` as is the version that is able to work with docker.
Two different installation options are depending on the Windows version that you are currently using. These two different ways are:
  - [Installing WSL2 in Windows build 19041 or higher](#installing-wsl2-in-windows-build-19041-or-higher)
  - [Installing WSL2 in Windows with older builds](#installing-wsl2-in-windows-with-older-builds)

### Installing `WSL2` in Windows build 19041 or higher

> **Note**: To be able to use this method, you need to have your Windows system updated as this command is only available with the latest update packages.

If your own a Windows system with a higher build than `19041`, you only need to run this single command:
```
wsl --install
```
After the command execution is done, reboot your PC and you'll have `wsl` installed and the `Ubuntu` Linux distribution. 

If this method seems to not work with your system, follow the [Installing WSL2 in Windows with older builds](##installing-wsl2-in-windows-with-older-builds) section.


### Installing `WSL2` in Windows with older builds

If you are using a system with an inferior build version, there are a few steps that you have to follow to get `WSL2` on your system

#### 1. Enabling Windows Subsystem for Linux

Before you can get `WSL2`, you need to have `WSL`.
Open PowerShell as administrator and enter this command:
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

#### 2. Enabling Virtual Machine

`WSL2` is a tiny virtual machine, so `Windows` needs to be prepared for that:
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
After running the command above, reboot your PC.

#### 3. Downloading and installing `WSL2`
  - Download the [WSL2 Kernel update](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi) and run the installer. When prompted for elevated permissions, click `yes`. Once the installer process is over you'll have `WSL2` successfully installed on your PC. 
  - Now you have to set WSL2 as your default version. That can be done by running the next command:
```
wsl --set-default-version 2
```
  - After all the steps above are completed, you can proceed to download
the `Ubuntu on Windows` application on the [store](https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab
).

## Creating the `wslconfig` file
You can add a file named `.wslconfig` to your Windows home directory to control global WSL options across Linux distributions. 

In our case, this file is created to limit the resources that `wls` consumes for the Virtual machines, mainly the assigned memory. We can specify the amount of memory that `wsl` is allowed to take from Windows. The lines specified below forces `wsl` to consume 3GB as a max memory value, without a swap memory and allowing forwarding localhost ports connection. If you can learn more about the allowed parameters in this file, check this [link](https://docs.microsoft.com/en-us/windows/wsl/wsl-config#options-for-wslconfig)

```
[wsl2]
memory=3GB
swap=0
localhostForwarding=true
```

## Setting up Ubuntu subsystem

Once `WSL2` has been installed and configured, you need to configure the downloaded Ubuntu subsystem for Windows. To do so, follow the next steps:
  1. Open `Windows Powershell` and type `wsl`
  2. Enter the desired `username` and `password` for your subsystem.


