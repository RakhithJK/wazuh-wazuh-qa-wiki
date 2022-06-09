# Introduction
The deployer pipeline provides an easy way to deploy multiple hosts simultaneously.

# Parameters
  
  **`JENKINS REFERENCE`**: Wazuh-jenkins repository branch

  **`DEPLOYMENT_CONFIGURATION`**: YAML multiline string with the list of instances to deploy
  
  **`DESTROY_INSTANCES`**: Enable automatical instances destruction after an 8 hours interval.
  
  **`DEBUG`**: Enable debug mode
  

# Deployment configuration

The deployment configuration is made up of one or more deployment blocks. These will describe the number, operating system, resources, and groups of instances. 

### Deployment block structure

The basic structure for a deployment is shown below:

```
- service: <SERVICE>

  instances:

     - <OS1>

     - <OS2>

  resources:

    - cpu: <CPU1>

      memory: <MEMORY1>

    - cpu: <CPU2>

      memory: <MEMORY2>

  groups: ['GROUP1', 'GROUP2']
```

## **Service**
The `service` label specifies the type of instance to be launched. The supported services are:

### EC2

Amazon EC2 provides a wide selection of instance types optimized to fit different use cases. Instance types comprise varying combinations of CPU, memory, storage, and networking capacity and give you the flexibility to choose the appropriate mix of resources for your applications. Each instance type includes one or more instance sizes, allowing you to scale your resources to the requirements of your target workload.


> Note: For more information about EC2, visit the [AWS official documentation](https://aws.amazon.com/ec2/instance-types/)

### ECS
  
An Amazon ECS container instance is an Amazon EC2 instance that is running the Amazon ECS container agent and has been registered into an Amazon ECS cluster. When you run tasks with Amazon ECS using the EC2 launch type or an Auto Scaling group capacity provider, your tasks are placed on your active container instances.
	
> Note: For more information about ECS, visit the [AWS official documentation](https://aws.amazon.com/es/ecs/)


### Vagrant

Vagrant is an open-source software product for building and maintaining portable virtual software development environments; e.g., for VirtualBox, KVM, Hyper-V, Docker containers, VMware, and AWS. It tries to simplify the software configuration management of virtualization in order to increase development productivity.

> Note: For more information about vagrant, visit the [vagrant official documentation](https://www.vagrantup.com/))


## Instances
The `instances` label indicates the operating system to be deployed in the AWS instance. Operating systems supported by service:

<details>

<summary> EC2 </summary>


- ***amazonlinux_2*** for Amazon Linux 2
	
- ***centos_8*** for Centos 8
	
- ***debian_10*** for Debian 10
	
- ***ubuntu_22*** for Ubuntu 22
	
- ***windows_server_2016*** for Windows Server 2016

- ***windows_server_2022*** for Windows Server 2022


</details>

<details>

<summary> ECS </summary>

- ***amazonlinux_2*** for Amazon Linux 2
	
- ***centos_8*** for Centos 8
	
- ***reddhat_8*** for Redhat 8
	
- ***ubuntu_22*** for Ubuntu 22
	
- ***debian_11*** for Debian 11

</details>

<details>

<summary> Vagrant </summary>

- ***macos_1015*** for Macos

- ***solaris_10*** for Solaris 10

- ***solaris_11*** for Solaris 11

</details>

> Note: The `os` label is built with the OS name underscore (`_`) OS version.


When the `os` label does not specify the version number, each instance has its own OS by default.

<details>

<summary> Default OS </summary>

- ***amazonlinux*** for Amazon Linux 2

- ***centos*** for Centos 8

- ***debian*** for Debian 10

- ***ubuntu*** for Ubuntu 22

- ***windows*** for Windows Server 2016

- ***reddhat*** for Redhat 8

- ***macos*** for Macos

- ***solaris*** for Solaris 11

</details>


<details>

<summary>Example deployment configuration with different instances</summary>

```
- service: EC2
  instances:
     - ubuntu
     - ubuntu_22
     - amazonlinux
     - windows
     - windows_server_2022
     - debian
```

</details>

  
## Resources

The `resources` label indicates the resources, `cpu` and `memory`, of the instance to be launched.  Both fields are mandatory in the `resources` block.

> Note: In case any of the resources do not match the available list of resources per instance, the instance with the upper closer resources will be launched.


If the `number` label is greater than 1 and `instances` and `resources` labels are equal to 1, all the instances will be deployed with the same resources.

> Note: If the number of `resources` does not match with the number of `instances`, an exception will be thrown.


Available resources are service dependent:

<details>

<summary> EC2 </summary>


| Instance Type | CPU | Memory |
|---------------|-----|--------|
|     T2_MICRO          |   1  |    1024    |
|     T3_MEDIUM          |  2   |  4096       |
|     C5_LARGE          |    2 |    8192    |
|     C5_XLARGE          |   4  |   16384     |
|     C5_2XLARGE          |   8  |    32768    |


</details>

> Note: Default resources are `cpu`: 1 and `memory`: 1024.

<details>

<summary> ECS </summary>


| CPU | Memory |
|---------------|-----|
|   1  |    1024    |
|   1  |    2048    |
|   1  |    3072    |
|   2   |  4096       |
|    2 |    16384    |
|   4  |   4096     |
|   4  |   16384     |


</details>

> Note: Default resources are `cpu`: 2 and `memory`: 1024.

  <details>

<summary> Vagrant </summary>


| CPU | Memory |
|---------------|-----|
|   2  |    6144    |


</details>

> Note: Currently multiple resources are not supported for vagrant instances. The real supported resources are 2 CPU and  6144MB of memory for macOS and 2 CPU and 2048MB of memory for Solaris

<details>

<summary>Example deployment configuration with resources </summary>

```
- service: EC2
  instances:
     - ubuntu
     - amazonlinux
  resources:
    - cpu: 1
      memory: 1024
    - cpu: 3
      memory: 2048
```


</details>


##  Groups

The group tag is used to group instances in the deployment inventory. 

The instances will be automatically added to the defaults groups:

- OS Based groups:
	- `linux`
	- `windows`
	- `macos`
	- `solaris`
- Service based groups:
	- `ecs`
	- `ec2`
	- `vagrant`


In addition to these groups, all instances of a configuration block will be added to each of the groups.



<details>

<summary>Example deployment configuration with groups </summary>


```
- service: EC2
  instances:
     - ubuntu
  groups: ['group1', 'testing', 'manager']
```

</details>



## Artifacts

The deployer pipeline produces two artifacts: Connection information and deployment inventory

### Inventory
Ansible inventory is created using specified groups and default service and OS groups.

<details>

<summary> Example </summary>

```
  

ec2:

   hosts:

      PoC_environment_launcher_VR_485_20220524120930_centos_0_0:

         ansible_host: 172.31.12.49

         ansible_user: qa

         ansible_connection: ssh

   vars:

      service: ec2

linux:

   hosts:

       PoC_environment_launcher_VR_485_20220524120930_centos_0_0:

          ansible_host: 172.31.12.49

          ansible_user: qa

          ansible_connection: ssh

vars:

    os: linux

all:

   vars:

       ansible_ssh_common_args: -o StrictHostKeyChecking=no
```


</details>


### Connection artifact

Instances connection information file. Connection information is generated depending on the service and OS of the instance.

If it is an `EC2` Windows instance, to access the machine it is recommended to download the Remmina client.

Remmina is a remote desktop client for POSIX-based computer operating systems (https://remmina.org/).

In addition, it is necessary to install the RDP plugin. See an [example](https://community.linuxmint.com/software/view/remmina-plugin-rdp) here.

The configuration required for the connection is as follows:

```

- Protocol: RDP

- Server: <IP ADDRESS>

- Username: <DEFAULT USER>

- Password: wazuhqa

- Resolution: Custom(1280x960)

- Depth of colour: True color(24 ppp)

```

If it is an `EC2` OR `ecs` Linux instance, to access the machine it need the following command:

```

ssh -i <LINUX_PRIVATE_KEY> <DEFAULT USER>@<IP ADDRESS>

```

For a `vagrant` deployment to access the machine it need the following command:

```

ssh vagrant@<IP ADDRESS> -p <PORT>

password: vagrant

```

  

## Global Instance parameters

- **Name**:  `<PIPELINE_NAME><JOB_NUMBER>_<TIMESTAMP>_<OS>_<BLOCK_INDEX>_<INSTANCE_INDEX>`


## Images

### EC2


| AMI | System 
|---|---|
| ami-07efb5be4a4f36912 | Amazon Linux 2 | 
| ami-035d6ac4014f95a1f | Centos 8 |
| ami-070ed7bf83e673bea | Debian 10 |
| ami-05f84c8ee1f23b186 | Ubuntu 22 | 
| ami-0ef2463f7ca02ccea | Windows Server 2016 | 
| ami-09a0b558ea45c57f2 | Windows Server 2022 | 

<details>

<summary> Dependencies </summary>

- `python 3.10`
- `pip 21.3.1`
- `ip` -> `iproute2`
- `ifconfig` -> `net-tools`
- editors: `vim, nano`
- `sudo`, `visudo`
- `git` 

</details>
<details>

<summary> Connection information</summary>

- User: QA
- Password: wazuhqa

</details>

### ECR

| ECR | System 
|---|---|
| qa/amazonlinux | Amazon Linux 2 | 
| qa/centos | Centos 8 |
| qa/redhat |  RedHat 8|
| qa/ubuntu | Ubuntu 22 | 
| qa/debian | Debian 10 | 


<details>

<summary> Dependencies </summary>

- `python 3.10`
- `pip 21.3.1`
- `ip` -> `iproute2`
- `ifconfig` -> `net-tools`
- editors: `vim, nano`
- `sudo`, `visudo`
- `git` 

</details>
<details>

<summary> Connection information</summary>

- User: QA
- Password: wazuhqa

</details>





### Examples of deployment configurations

  

#### Example 1

```

- service: EC2

  instances:

     - ubuntu

```

  

The `inventory.yaml` that it gets is the following:

  

<details>

  

```

ec2:

hosts:

PoC_environment_launcher_VR_543_20220525163513_ubuntu_0_0:

ansible_host: 172.31.3.82

ansible_user: qa

ansible_connection: ssh

vars:

service: ec2

linux:

hosts:

PoC_environment_launcher_VR_543_20220525163513_ubuntu_0_0:

ansible_host: 172.31.3.82

ansible_user: qa

ansible_connection: ssh

vars:

os: linux

all:

vars:

ansible_ssh_common_args: -o StrictHostKeyChecking=no

```

</details>

  
  

#### Example 2

```

- service: ECS

  instances:

    - amazonlinux_2

```

  

The `inventory.yaml` that it gets is the following:

  

<details>

  

```

ecs:

hosts:

PoC_environment_launcher_VR_530_20220525142517_amazonlinux_2_0_0:

ansible_host: 172.31.35.220

ansible_user: qa

ansible_connection: ssh

vars:

service: ecs

linux:

hosts:

PoC_environment_launcher_VR_530_20220525142517_amazonlinux_2_0_0:

ansible_host: 172.31.35.220

ansible_user: qa

ansible_connection: ssh

vars:

os: linux

all:

vars:

ansible_ssh_common_args: -o StrictHostKeyChecking=no

```

</details>

  
  

#### Example 3

```

- service: vagrant

  instances:

  - solaris_10

```

  

The `inventory.yaml` that it gets is the following:

  

<details>

  

```

vagrant:

hosts:

PoC_environment_launcher_VR_523_20220525140739_solaris_10_0_0:

ansible_host: 10.10.0.251

ansible_port: 13300

ansible_password: vagrant

ansible_user: vagrant

vars: {}

solaris:

hosts:

PoC_environment_launcher_VR_523_20220525140739_solaris_10_0_0:

ansible_host: 10.10.0.251

ansible_port: 13300

ansible_password: vagrant

ansible_user: vagrant

vars: {}

all:

vars:

ansible_ssh_common_args: -o StrictHostKeyChecking=no

```

</details>

  

#### Example 4

  

For the following `DEPLOYMENT_FILE_CONTENT` field:

  

```

- service: EC2

  instances:

    - ubuntu

  groups: ['managers']

```

  

The `inventory.yaml` that it gets is the following:

  

<details>

  

```

managers:

hosts:

PoC_environment_launcher_VR_547_20220525164852_ubuntu_0_0:

ansible_host: 172.31.14.63

ansible_user: qa

ansible_connection: ssh

ec2:

hosts:

PoC_environment_launcher_VR_547_20220525164852_ubuntu_0_0:

ansible_host: 172.31.14.63

ansible_user: qa

ansible_connection: ssh

vars:

service: ec2

linux:

hosts:

PoC_environment_launcher_VR_547_20220525164852_ubuntu_0_0:

ansible_host: 172.31.14.63

ansible_user: qa

ansible_connection: ssh

vars:

os: linux

all:

vars:

ansible_ssh_common_args: -o StrictHostKeyChecking=no

```

</details>

  

#### Example 5

```

- service: EC2

  instances:

    - centos

  resources:

  -

    cpu: 1

    memory: 1024

```

  

The `inventory.yaml` that it gets is the following:

  

<details>

  

```

ec2:

hosts:

PoC_environment_launcher_VR_485_20220524120930_centos_0_0:

ansible_host: 172.31.12.49

ansible_user: qa

ansible_connection: ssh

vars:

service: ec2

linux:

hosts:

PoC_environment_launcher_VR_485_20220524120930_centos_0_0:

ansible_host: 172.31.12.49

ansible_user: qa

ansible_connection: ssh

vars:

os: linux

all:

vars:

ansible_ssh_common_args: -o StrictHostKeyChecking=no

```

</details>

  
  

#### Example 6
```

- service: ECS
  instances:
    - ubuntu
	- ubuntu

  resources:
    - cpu: 1
      memory: 1024
    - cpu: 2
      memory: 1024

```

  

The `inventory.yaml` that it gets is the following:

  

<details>

  

```

ecs:

hosts:

PoC_environment_launcher_VR_488_20220524121834_ubuntu_0_0:

ansible_host: 172.31.76.105

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_488_20220524121834_ubuntu_0_1:

ansible_host: 172.31.41.210

ansible_user: qa

ansible_connection: ssh

vars:

service: ecs

linux:

hosts:

PoC_environment_launcher_VR_488_20220524121834_ubuntu_0_0:

ansible_host: 172.31.76.105

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_488_20220524121834_ubuntu_0_1:

ansible_host: 172.31.41.210

ansible_user: qa

ansible_connection: ssh

vars:

os: linux

all:

vars:

ansible_ssh_common_args: -o StrictHostKeyChecking=no

```

</details>

  

#### Example 7

```

- service: ECS

  instances:

    - amazonlinux

- service: EC2

  instances:

    - centos

```

  

The `inventory.yaml` that it gets is the following:

  

<details>

  

```

ecs:

hosts:

PoC_environment_launcher_VR_490_20220524121906_amazonlinux_0_0:

ansible_host: 172.31.44.231

ansible_user: qa

ansible_connection: ssh

vars:

service: ecs

linux:

hosts:

PoC_environment_launcher_VR_490_20220524121906_amazonlinux_0_0:

ansible_host: 172.31.44.231

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_490_20220524121906_centos_1_0:

ansible_host: 172.31.4.67

ansible_user: qa

ansible_connection: ssh

vars:

os: linux

ec2:

hosts:

PoC_environment_launcher_VR_490_20220524121906_centos_1_0:

ansible_host: 172.31.4.67

ansible_user: qa

ansible_connection: ssh

vars:

service: ec2

all:

vars:

ansible_ssh_common_args: -o StrictHostKeyChecking=no

```

</details>

  

#### Example 8

```

- service: ECS

  instances:

    - amazonlinux

  resources:

    - cpu: 2

      memory: 3000

  groups: ['agent']

  

- service: ECS

  instances:

    - ubuntu

   groups: ['manager', 'group-1']


- service: EC2

  instances:

    - centos

  groups: ['wazuh-dashboard', 'wazuh-indexer']

  

- service: EC2

  instances:

    - amazonlinux

  groups: ['wazuh-indexer', 'group-3']

  

- service: vagrant

  instances:

    - macos

  groups: ['agent']

```

  

The `inventory.yaml` that it gets is the following:

  

<details>

  

```

agent:

hosts:

PoC_environment_launcher_VR_483_20220524115358_amazonlinux_0_0:

ansible_host: 172.31.85.111

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_483_20220524115358_amazonlinux_0_1:

ansible_host: 172.31.26.24

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_483_20220524115358_macos_4_0:

ansible_host: 10.10.0.251

ansible_port: 64183

ansible_password: vagrant

ansible_user: vagrant

vars: {}

ecs:

hosts:

PoC_environment_launcher_VR_483_20220524115358_amazonlinux_0_0:

ansible_host: 172.31.85.111

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_483_20220524115358_amazonlinux_0_1:

ansible_host: 172.31.26.24

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_483_20220524115358_ubuntu_1_0:

ansible_host: 172.31.51.152

ansible_user: qa

ansible_connection: ssh

vars:

service: ecs

linux:

hosts:

PoC_environment_launcher_VR_483_20220524115358_amazonlinux_0_0:

ansible_host: 172.31.85.111

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_483_20220524115358_amazonlinux_0_1:

ansible_host: 172.31.26.24

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_483_20220524115358_ubuntu_1_0:

ansible_host: 172.31.51.152

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_483_20220524115358_centos_2_0:

ansible_host: 172.31.8.232

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_483_20220524115358_amazonlinux_3_0:

ansible_host: 172.31.9.224

ansible_user: qa

ansible_connection: ssh

vars:

os: linux

manager:

hosts:

PoC_environment_launcher_VR_483_20220524115358_ubuntu_1_0:

ansible_host: 172.31.51.152

ansible_user: qa

ansible_connection: ssh

group-1:

hosts:

PoC_environment_launcher_VR_483_20220524115358_ubuntu_1_0:

ansible_host: 172.31.51.152

ansible_user: qa

ansible_connection: ssh

wazuh-dashboard:

hosts:

PoC_environment_launcher_VR_483_20220524115358_centos_2_0:

ansible_host: 172.31.8.232

ansible_user: qa

ansible_connection: ssh

wazuh-indexer:

hosts:

PoC_environment_launcher_VR_483_20220524115358_centos_2_0:

ansible_host: 172.31.8.232

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_483_20220524115358_amazonlinux_3_0:

ansible_host: 172.31.9.224

ansible_user: qa

ansible_connection: ssh

vars: {}

ec2:

hosts:

PoC_environment_launcher_VR_483_20220524115358_centos_2_0:

ansible_host: 172.31.8.232

ansible_user: qa

ansible_connection: ssh

PoC_environment_launcher_VR_483_20220524115358_amazonlinux_3_0:

ansible_host: 172.31.9.224

ansible_user: qa

ansible_connection: ssh

vars:

service: ec2

group-3:

hosts:

PoC_environment_launcher_VR_483_20220524115358_amazonlinux_3_0:

ansible_host: 172.31.9.224

ansible_user: qa

ansible_connection: ssh

vagrant:

hosts:

PoC_environment_launcher_VR_483_20220524115358_macos_4_0:

ansible_host: 10.10.0.251

ansible_port: 64183

ansible_password: vagrant

ansible_user: vagrant

vars: {}

macos:

hosts:

PoC_environment_launcher_VR_483_20220524115358_macos_4_0:

ansible_host: 10.10.0.251

ansible_port: 64183

ansible_password: vagrant

ansible_user: vagrant

vars: {}

all:

vars:

ansible_ssh_common_args: -o StrictHostKeyChecking=no

```

</details>


# Schema blocks

The below tables show the labels allowed for the deployment structure discussed above, along with the data type and example values for these labels:


| Name | Type | Requirement | Description | Example case |
|:-:|:-:|:-:|:-:|:-:|
| service | String | Mandatory | Type of instance to be launched. | service: EC2, ECS, or vagrant |
| instances | String list | Mandatory | Operating system to be deployed in the AWS instance. | os: - ubuntu_22 - centos_8 |
| resources | Map | Optional | Specify the resources (cpu and memory), of the instance to be deployed. | resources: [cpu: 2, memory: 4096] |
| cpu | int | Optional | Number of cpu to be deployed. | cpu: 1, 2, or ... |
| memory | int | Optional | Number of memory to be deployed. | memory: 1024, 2048, or ... |
| groups | String list | Optional | List of groups to be created in the inventory file | groups: ['agent', ...] |
  
  




