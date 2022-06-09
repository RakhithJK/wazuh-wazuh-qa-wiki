# Introduction
The deployer pipeline provides an easy way to deploy multiple hosts simultaneously.

# User guide

## Parameters
  
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

### **Service**
The `service` label specifies the type of instance to be launched. The supported services are:

#### EC2

Amazon EC2 provides a wide selection of instance types optimized to fit different use cases. Instance types comprise varying combinations of CPU, memory, storage, and networking capacity and give you the flexibility to choose the appropriate mix of resources for your applications. Each instance type includes one or more instance sizes, allowing you to scale your resources to the requirements of your target workload.


> Note: For more information about EC2, visit the [AWS official documentation](https://aws.amazon.com/ec2/instance-types/)

#### ECS
  
An Amazon ECS container instance is an Amazon EC2 instance that is running the Amazon ECS container agent and has been registered into an Amazon ECS cluster. When you run tasks with Amazon ECS using the EC2 launch type or an Auto Scaling group capacity provider, your tasks are placed on your active container instances.
	
> Note: For more information about ECS, visit the [AWS official documentation](https://aws.amazon.com/es/ecs/)


#### Vagrant

Vagrant is an open-source software product for building and maintaining portable virtual software development environments; e.g., for VirtualBox, KVM, Hyper-V, Docker containers, VMware, and AWS. It tries to simplify the software configuration management of virtualization in order to increase development productivity.

> Note: For more information about vagrant, visit the [vagrant official documentation](https://www.vagrantup.com/))


### Instances
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

</details>

  
### Resources

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



**Example deployment configuration with resources**


####  Groups

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

</details>

  

