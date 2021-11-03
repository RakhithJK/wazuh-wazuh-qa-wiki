# Installing ElasticSearch

First of all, this is the official [download page](https://www.elastic.co/downloads/elasticsearch) where you can find the official documentation to install ES for all systems.

## Debian

```
sudo apt install curl
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add - && \
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-7.x.list && \
apt update && \
apt install -y elasticsearch
```

## ArchLinux
You can get the ES package within the [community repo](https://archlinux.org/packages/?name=elasticsearch) or:
```
pacman -S elasticsearch
```

## CentOS
You can follow the official [installation page](https://www.elastic.co/guide/en/elasticsearch/reference/7.15/rpm.html#rpm-repo)
```
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
yum install elasticsearch-oss.x86_64
```

## Windows

You can use the official [download page](https://www.elastic.co/downloads/elasticsearch) to download it or use [Chocolatey](https://chocolatey.org/install).

```
choco install elasticsearch
```

### Set the JVM heap size

The JVM itself requires some memory, so it is normal for ELasticSearch to use more memory than the limit configured with the options below. To set 1GB, `- Xms1g` and `- Xmx1g` must be written in `jvm.options`. It is located in `etc/elasticsearch` and `/config` for windows inside the ES installation.

#### Windows
By default, ES sets the JVM heap size, so it is necessary to modify it using these options:
- `Xms`
- `Xmx`

#### Linux

```
echo "-Xms1g" >> /etc/elasticsearch/jvm.options
echo "-Xmx1g" >> /etc/elasticsearch/jvm.options
```

or add these lines to `etc/elasticsearch/jvm.options` or `/configjvm.options`:

```
-Xms1g
-Xmx1g
```

`-XmsAm` defines the max allocation of RAM to ES JVM Heap, where A is the amount of MBs that it uses. It is possible to use `-XmsAg` to specify GBs. It is recommended to use 1GB as the limit. In some cases, it can raise a time out if 256MB/512MB is used.

When the size is set, start the ES service by `systemctl start elasticsearch.service` or `bin/elasticsearch.bat` within the ES installation in windows.

### Remove ES security warnings

#### Windows
Add to the `elasticsearch.yml` within the installation directory `/config`:
```
xpack.security.enabled: false
```

### Linux
```
echo "xpack.security.enabled: false" >> /etc/elasticsearch/elasticsearch.yml
```