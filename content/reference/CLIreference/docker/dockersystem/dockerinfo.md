+++
title = "docker info"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/system/info/](https://docs.docker.com/reference/cli/docker/system/info/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker system info

| Description | Display system-wide information |
| :---------- | ------------------------------- |
| Usage       | `docker system info [OPTIONS]`  |
| Aliases     | `docker info`                   |

## Description

This command displays system wide information regarding the Docker installation. Information displayed includes the kernel version, number of containers and images. The number of images shown is the number of unique images. The same image tagged under different names is counted only once.

​	此命令显示有关 Docker 安装的系统范围信息，包括内核版本、容器数量和镜像数量。显示的镜像数量是唯一镜像的数量。同一镜像被标记为不同名称的情况仅计算一次。

If a format is specified, the given template will be executed instead of the default format. Go's [text/template](https://pkg.go.dev/text/template) package describes all the details of the format.

​	如果指定了格式，则将执行给定的模板，而不是默认格式。Go 的 [text/template](https://pkg.go.dev/text/template) 包描述了格式的所有详细信息。

Depending on the storage driver in use, additional information can be shown, such as pool name, data file, metadata file, data space used, total data space, metadata space used, and total metadata space.

​	根据正在使用的存储驱动，可以显示其他信息，例如池名、数据文件、元数据文件、数据空间使用量、总数据空间、元数据空间使用量和总元数据空间。

The data file is where the images are stored and the metadata file is where the meta data regarding those images are stored. When run for the first time Docker allocates a certain amount of data space and meta data space from the space available on the volume where `/var/lib/docker` is mounted.

​	数据文件存储镜像，元数据文件存储有关这些镜像的元数据。Docker 首次运行时会从挂载 `/var/lib/docker` 的卷的可用空间中分配一定的数据空间和元数据空间。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --format`](https://docs.docker.com/reference/cli/docker/system/info/#format) |         | 使用自定义模板格式化输出: 'json': 以 JSON 格式打印 'TEMPLATE': 使用指定的 Go 模板格式化输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |

## Examples

### 显示输出 Show output

The example below shows the output for a daemon running on Ubuntu Linux, using the `overlay2` storage driver. As can be seen in the output, additional information about the `overlay2` storage driver is shown:

​	下面的示例显示了在使用 `overlay2` 存储驱动的 Ubuntu Linux 上运行的守护进程的输出。如输出中所示，显示了有关 `overlay2` 存储驱动的附加信息：



```console
$ docker info

Client:
 Version:    25.0.0
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.12.1
    Path:     /usr/local/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.24.1
    Path:     /usr/local/libexec/docker/cli-plugins/docker-compose

Server:
 Containers: 14
  Running: 3
  Paused: 1
  Stopped: 10
 Images: 52
 Server Version: 25.0.0
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: runc io.containerd.runc.v2
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 71909c1814c544ac47ab91d2e8b84718e517bb99
 runc version: v1.1.11-0-g4bccb38
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.5.11-linuxkit
 Operating System: Alpine Linux v3.19
 OSType: linux
 Architecture: aarch64
 CPUs: 10
 Total Memory: 7.663GiB
 Name: 4a7ed206a70d
 ID: c20f7230-59a2-4824-a2f4-fda71c982ee6
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
```

### Format the output (`--format`)

You can also specify the output format:

​	您还可以指定输出格式：

```console
$ docker info --format '{{json .}}'

{"ID":"4cee4408-10d2-4e17-891c-a41736ac4536","Containers":14, ...}
```

### 在 Windows 上运行 `docker info` Run `docker info` on Windows

Here is a sample output for a daemon running on Windows Server:

​	以下是一个在 Windows Server 上运行的守护进程的示例输出：

```console
C:\> docker info

Client: Docker Engine - Community
 Version:    24.0.0
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.10.4
    Path:     C:\Program Files\Docker\cli-plugins\docker-buildx.exe
  compose: Docker Compose (Docker Inc.)
    Version:  v2.17.2
    Path:     C:\Program Files\Docker\cli-plugins\docker-compose.exe

Server:
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 17
 Server Version: 23.0.3
 Storage Driver: windowsfilter
 Logging Driver: json-file
 Plugins:
  Volume: local
  Network: ics internal l2bridge l2tunnel nat null overlay private transparent
  Log: awslogs etwlogs fluentd gcplogs gelf json-file local splunk syslog
 Swarm: inactive
 Default Isolation: process
 Kernel Version: 10.0 20348 (20348.1.amd64fre.fe_release.210507-1500)
 Operating System: Microsoft Windows Server Version 21H2 (OS Build 20348.707)
 OSType: windows
 Architecture: x86_64
 CPUs: 8
 Total Memory: 3.999 GiB
 Name: WIN-V0V70C0LU5P
 ID: 2880d38d-464e-4d01-91bd-c76f33ba3981
 Docker Root Dir: C:\ProgramData\docker
 Debug Mode: false
 Experimental: true
 Insecure Registries:
  myregistry:5000
  127.0.0.0/8
 Registry Mirrors:
   http://192.168.1.2/
   http://registry-mirror.example.com:5000/
 Live Restore Enabled: false
```
