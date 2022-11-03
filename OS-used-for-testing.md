Due to the different tests executed, we have found the need to test on different OS.

And to avoid future search and investigation of OS that work, we will list them below:


| OS | OS version | Deployment                                    | Image/AMI | Notes |
|----|------------|-----------------------------------------------|-----------|-------|
| Linux   |  Centos8 | `LOCAL\| Vagrant` | [qactl/centos8](https://s3.amazonaws.com/ci.wazuh.com/qa/boxes/QACTL_centos_8.box) |       |
| Windows | Windows XP | `LOCAL\| Virtualbox`|  [WinXP ISO](https://ia801004.us.archive.org/0/items/WindowsXPProfessional64BitCorporateEdition/Windows%20XP%20Professional%2064-bit%20Corporate%20Edition%28CD%20Key%20VCFQD-V9FX9-46WVH-K3CD4-4J3JM%29.iso) |
| Windows | Windows 7 | `LOCAL\| Virtualbox` | corbob/Windows7 |
| Windows | Win 8.1 | `LOCAL\| Virtualbox` | [Win 8.1 ISO](https://www.microsoft.com/en-us/software-download/windows8ISO) | Serial: 3FCND-JTWFM-24VQ8-QXTMB-TXT67 |       
| Windows | Windows 10 | `LOCAL\| Virtualbox` | alex-ks02/win10homeN_1809Oct_Eng_x86 |      
| Windows | Windows 11 | `LOCAL\| Virtualbox` | Ravager/Windows11 |
| Windows | Windows Server 2008 R2 | `LOCAL\| Virtualbox` | [ISO](https://archive.org/details/WindowsServer2008withSP2x86) |      
| Windows | Windows Server 2008 R2 | `LOCAL\| Virtualbox` | diodonfrost/windows-2k8r2 |      
| Windows | Windows Server 2019 | `LOCAL\| Virtualbox` | [qactl/winserver19](https://s3.amazonaws.com/ci.wazuh.com/qa/boxes/QACTL_windows_server_2019.box) |
| ArchLinux | base-20221030.0.98412  | `LOCAL\| Docker` | [Docker Image](https://hub.docker.com/_/archlinux/) |