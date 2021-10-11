# Installing WSL2 on Windows


WSL 2 is a new version of the Windows Subsystem for Linux architecture that enables the Windows Subsystem for Linux to run Linux binaries on Windows. This tool is needed by `qa-ctl` for integrating Ansible in Windows, as there isn't any native support for this tool in the system. We are using the second version of this tool as is the version that is able to work with docker.
There are two different installation options depending on the Windows version that you are currently using. These two different ways are:
  - [Build 19041 or higher](#build-19041-or-higher)
  - [Older builds](#older-builds)

### Build 19041 or higher
> **Note**: To be able to use this method, you need to have your Windows system updated as this command is only available after some update packages from Windows.

If your current OS is `Windows11`, `Windows 10 version 2004` or any Windows system with a superior build than `19041`, you only need to run this single command:
```
wsl --install
```
After the command execution is done, reboot your PC and you'll have `wsl` installed and the `Ubuntu` Linux distribution. 

If this method seems to not work with your system, follow the [Older builds](#older-builds) section.


### Older builds

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

#### 3. Downloading and installing WSL2
  - Download the [WSL2 Kernel update](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi) and run the installer. When prompted for elevated permissions, click `yes`. Once the installer process is over you'll have `WSL2` successfully installed on your PC. 
  - Now you have to set WSL2 as your default version. That can be done by running the next command:
```
wsl --set-default-version 2
```
  - After all the steps above are completed, you can proceed to download
the `Ubuntu on Windows` application on the [store](https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab
).

## wslconfig file
Finally, on your home folder, create the file `.wslconfig` and add the following lines:

```
[wsl2]
memory=3GB
swap=0
localhostForwarding=true
```


