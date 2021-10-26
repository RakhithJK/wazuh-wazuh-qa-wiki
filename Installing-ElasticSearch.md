# Installing ElasticSearch

First of all, this is the official [download page](https://www.elastic.co/downloads/elasticsearch).

## Debian

```
sudo apt install curl
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add - && \
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-7.x.list && \
apt update && \
apt install -y elasticsearch
```

## ArchLinux

```
pacman -S elasticsearch
```

## CentOS

```
yum install elasticsearch-oss.x86_64
```

## Windows

You can use the official [download page](https://www.elastic.co/downloads/elasticsearch) to download it or use [Chocolatey](https://chocolatey.org/install).

```
choco install elasticsearch
```

### Set the JVM heap size

By default, ES sets the JVM heap size, so it is necessary to modify it using these options:

- `Xms`
- `Xmx`

The JVM itself requires some memory, so it is normal for ELasticSearch to use more memory than the limit configured with the options below. To set 1GB, `- Xms1g` and `- Xmx1g` must be written in `jvm.options`. It is located in `etc/elasticsearch` and `/config` for windows inside the ES installation.

```
echo "-Xms1g" >> /etc/elasticsearch/jvm.options
echo "-Xmx1g" >> /etc/elasticsearch/jvm.options
```

or add these lines to `etc/elasticsearch/jvm.options` or `/configjvm.options`:

```
-Xms1g
-Xmx1g
```

`-XmsAm` defines the max allocation of RAM to ES JVM Heap, where A is the amount of MBs it uses. It is possible to use `-XmsAg` to specify GBs. It is recommended to use 1GB as the limit. In some cases, it can raise a time out if 256MB/512MB is used.

When the size is set, start the ES service by `systemctl start elasticsearch.service` or `bin/elasticsearch.bat` within the ES installation in windows.
