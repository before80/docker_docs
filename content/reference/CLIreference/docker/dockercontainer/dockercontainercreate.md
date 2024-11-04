+++
title = "docker container create"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/create/](https://docs.docker.com/reference/cli/docker/container/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container create

| Description | Create a new container                                       |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container create [OPTIONS] IMAGE [COMMAND] [ARG...]` |
| Aliases     | `docker create`                                              |

## Description

The `docker container create` (or shorthand: `docker create`) command creates a new container from the specified image, without starting it.

​	`docker container create`（或简写为 `docker create`）命令从指定的镜像创建一个新的容器，但不启动它。

When creating a container, the Docker daemon creates a writeable container layer over the specified image and prepares it for running the specified command. The container ID is then printed to `STDOUT`. This is similar to `docker run -d` except the container is never started. You can then use the `docker container start` (or shorthand: `docker start`) command to start the container at any point.

​	在创建容器时，Docker 守护进程会在指定镜像上创建一个可写的容器层，并准备好运行指定的命令。然后，容器 ID 将被打印到 `STDOUT`。这类似于 `docker run -d`，只是容器并未被启动。然后，您可以随时使用 `docker container start`（或简写为 `docker start`）命令启动该容器。

This is useful when you want to set up a container configuration ahead of time so that it's ready to start when you need it. The initial status of the new container is `created`.

​	当您希望提前设置容器配置，以便在需要时能够快速启动时，这非常有用。新容器的初始状态为 `created`。

The `docker create` command shares most of its options with the `docker run` command (which performs a `docker create` before starting it). Refer to the [`docker run` CLI reference]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}}) for details on the available flags and options.

​	`docker create` 命令的大部分选项与 `docker run` 命令共享（后者在启动容器之前会执行 `docker create`）。有关可用标志和选项的详细信息，请参考 [`docker run` CLI 参考]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}})。

## Options

| Option                    | Default   | Description                                                  |
| ------------------------- | --------- | ------------------------------------------------------------ |
| `--add-host`              |           | 添加自定义的主机到 IP 映射(host:ip)  Add a custom host-to-IP mapping (host:ip) |
| `--annotation`            |           | API 1.43+ 为容器添加注释（传递给 OCI 运行时）API 1.43+ Add an annotation to the container (passed through to the OCI runtime) |
| `-a, --attach`            |           | 附加到 STDIN、STDOUT 或 STDERR  - Attach to STDIN, STDOUT or STDERR |
| `--blkio-weight`          |           | 块 IO（相对权重），在 10 和 1000 之间，0 代表禁用（默认 0）  Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0) |
| `--blkio-weight-device`   |           | 块 IO 权重（相对设备权重） Block IO weight (relative device weight) |
| `--cap-add`               |           | 添加 Linux 功能 Add Linux capabilities                       |
| `--cap-drop`              |           | 删除 Linux 功能  Drop Linux capabilities                     |
| `--cgroup-parent`         |           | 容器的可选父 cgroup  - Optional parent cgroup for the container |
| `--cgroupns`              |           | API 1.41+ Cgroup 命名空间使用 (host\|private)   API 1.41+ Cgroup namespace to use (host\|private) <br />'host': Run the container in the Docker host's cgroup namespace<br />'host': 在 Docker 主机的 cgroup 命名空间中运行容器<br /> 'private': Run the container in its own private cgroup namespace <br />'private': 在其自己的私有 cgroup 命名空间中运行容器<br />'': Use the cgroup namespace as configured by the default-cgroupns-mode option on the daemon (default)<br />'': 使用守护进程上配置的 default-cgroupns-mode 选项的 cgroup 命名空间(default) |
| `--cidfile`               |           | 将容器 ID 写入文件 Write the container ID to the file        |
| `--cpu-count`             |           | CPU 数量（仅限 Windows） CPU count (Windows only)            |
| `--cpu-percent`           |           | CPU 百分比（仅限 Windows） CPU percent (Windows only)        |
| `--cpu-period`            |           | 限制 CPU CFS（完全公平调度器）周期 Limit CPU CFS (Completely Fair Scheduler) period |
| `--cpu-quota`             |           | 限制 CPU CFS（完全公平调度器）配额 Limit CPU CFS (Completely Fair Scheduler) quota |
| `--cpu-rt-period`         |           | API 1.25+ 限制 CPU 实时周期（微秒） API 1.25+ Limit CPU real-time period in microseconds |
| `--cpu-rt-runtime`        |           | API 1.25+ 限制 CPU 实时运行时（微秒） API 1.25+ Limit CPU real-time runtime in microseconds |
| `-c, --cpu-shares`        |           | CPU 份额（相对权重） CPU shares (relative weight)            |
| `--cpus`                  |           | API 1.25+ CPU 数量 API 1.25+ Number of CPUs                  |
| `--cpuset-cpus`           |           | 允许执行的 CPU（0-3，0,1） CPUs in which to allow execution (0-3, 0,1) |
| `--cpuset-mems`           |           | 允许执行的 MEM（0-3，0,1） MEMs in which to allow execution (0-3, 0,1) |
| `--device`                |           | 向容器添加主机设备 Add a host device to the container        |
| `--device-cgroup-rule`    |           | 向 cgroup 允许的设备列表添加规则 Add a rule to the cgroup allowed devices list |
| `--device-read-bps`       |           | 限制来自设备的读取速率（字节/秒） Limit read rate (bytes per second) from a device |
| `--device-read-iops`      |           | 限制来自设备的读取速率（IO/秒） Limit read rate (IO per second) from a device |
| `--device-write-bps`      |           | 限制写入设备的速率（字节/秒） Limit write rate (bytes per second) to a device |
| `--device-write-iops`     |           | 限制写入设备的速率（IO/秒） Limit write rate (IO per second) to a device |
| `--disable-content-trust` | `true`    | 跳过镜像验证 Skip image verification                         |
| `--dns`                   |           | 设置自定义 DNS 服务器 Set custom DNS servers                 |
| `--dns-option`            |           | 设置 DNS 选项 Set DNS options                                |
| `--dns-search`            |           | 设置自定义 DNS 搜索域 Set custom DNS search domains          |
| `--domainname`            |           | 容器 NIS 域名 Container NIS domain name                      |
| `--entrypoint`            |           | 覆盖镜像的默认 ENTRYPOINT - Overwrite the default ENTRYPOINT of the image |
| `-e, --env`               |           | 设置环境变量 Set environment variables                       |
| `--env-file`              |           | 从文件中读取环境变量 Read in a file of environment variables |
| `--expose`                |           | 暴露端口或端口范围 Expose a port or a range of ports         |
| `--gpus`                  |           | API 1.40+ 添加到容器的 GPU 设备（'all' 表示传递所有 GPU） API 1.40+ GPU devices to add to the container ('all' to pass all GPUs) |
| `--group-add`             |           | 添加要加入的额外组 Add additional groups to join             |
| `--health-cmd`            |           | 检查健康状况的命令 Command to run to check health            |
| `--health-interval`       |           | 运行检查之间的时间(ms\|s\|m\|h) (default 0s) -   Time between running the check (ms\|s\|m\|h) (default 0s) |
| `--health-retries`        |           | 报告不健康所需的连续失败次数 Consecutive failures needed to report unhealthy |
| `--health-start-interval` |           | API 1.44+ 启动期间运行检查之间的时间(ms\|s\|m\|h) (default 0s)   API 1.44+ Time between running the check during the start period (ms\|s\|m\|h) (default 0s) |
| `--health-start-period`   |           | API 1.29+ 容器初始化前的启动周期，以开始健康检查重试倒计时 API 1.29+ Start period for the container to initialize before starting health-retries countdown (ms\|s\|m\|h) (default 0s) |
| `--health-timeout`        |           | 允许一个检查运行的最大时间(ms\|s\|m\|h) (default 0s) -  Maximum time to allow one check to run (ms\|s\|m\|h) (default 0s) |
| `--help`                  |           | 打印用法 Print usage                                         |
| `-h, --hostname`          |           | 容器主机名 Container host name                               |
| `--init`                  |           | API 1.25+ 在容器内部运行一个 init，转发信号并回收进程 API 1.25+ Run an init inside the container that forwards signals and reaps processes |
| `-i, --interactive`       |           | 即使未附加也保持 STDIN 打开 Keep STDIN open even if not attached |
| `--io-maxbandwidth`       |           | 系统驱动器的最大 IO 带宽限制（仅限 Windows） Maximum IO bandwidth limit for the system drive (Windows only) |
| `--io-maxiops`            |           | 系统驱动器的最大 IOps 限制（仅限 Windows） Maximum IOps limit for the system drive (Windows only) |
| `--ip`                    |           | IPv4 地址（例如：172.30.100.104） IPv4 address (e.g., 172.30.100.104) |
| `--ip6`                   |           | IPv6 地址（例如：2001:db8::33） IPv6 address (e.g., 2001:db8::33) |
| `--ipc`                   |           | 要使用的 IPC 模式 IPC mode to use                            |
| `--isolation`             |           | 容器隔离技术 Container isolation technology                  |
| `--kernel-memory`         |           | 内核内存限制 Kernel memory limit                             |
| `-l, --label`             |           | 在容器上设置元数据 Set meta data on a container              |
| `--label-file`            |           | 从以行分隔的标签文件中读取 Read in a line delimited file of labels |
| `--link`                  |           | 添加链接到另一个容器 Add link to another container           |
| `--link-local-ip`         |           | 容器的 IPv4/IPv6 链接本地地址 Container IPv4/IPv6 link-local addresses |
| `--log-driver`            |           | 容器的日志驱动程序 Logging driver for the container          |
| `--log-opt`               |           | 日志驱动程序选项 Log driver options                          |
| `--mac-address`           |           | 容器 MAC 地址（例如：92:d0:c6:0a:29:33） Container MAC address (e.g., 92:d0:c6:0a:29:33) |
| `-m, --memory`            |           | 内存限制 Memory limit                                        |
| `--memory-reservation`    |           | 内存软限制 Memory soft limit                                 |
| `--memory-swap`           |           | 交换限制等于内存加上交换：'-1' 表示启用无限交换 Swap limit equal to memory plus swap: '-1' to enable unlimited swap |
| `--memory-swappiness`     | `-1`      | 调整容器的内存可交换性（0 到 100） Tune container memory swappiness (0 to 100) |
| `--mount`                 |           | 将文件系统挂载到容器 Attach a filesystem mount to the container |
| `--name`                  |           | 分配一个名称给容器 Assign a name to the container            |
| `--network`               |           | 将容器连接到网络 Connect a container to a network            |
| `--network-alias`         |           | 为容器添加网络作用域别名 Add network-scoped alias for the container |
| `--no-healthcheck`        |           | 禁用任何容器指定的 HEALTHCHECK  - Disable any container-specified HEALTHCHECK |
| `--oom-kill-disable`      |           | 禁用 OOM 杀手 Disable OOM Killer                             |
| `--oom-score-adj`         |           | 调整主机的 OOM 偏好（-1000 到 1000） Tune host's OOM preferences (-1000 to 1000) |
| `--pid`                   |           | PID 命名空间使用 PID namespace to use                        |
| `--pids-limit`            |           | 调整容器的 PID 限制（设置为 -1 表示无限制）Tune container pids limit (set -1 for unlimited) |
| `--platform`              |           | API 1.32+ 设置平台，如果服务器支持多平台 API 1.32+ Set platform if server is multi-platform capable |
| `--privileged`            |           | 给予该容器扩展权限 Give extended privileges to this container |
| `-p, --publish`           |           | 将容器的端口发布到主机上 Publish a container's port(s) to the host |
| `-P, --publish-all`       |           | 将所有公开端口发布到随机端口 Publish all exposed ports to random ports |
| `--pull`                  | `missing` | 创建之前拉取镜像（`always`, `|missing`, `never`）Pull image before creating (`always`, `|missing`, `never`) |
| `-q, --quiet`             |           | 抑制拉取输出 Suppress the pull output                        |
| `--read-only`             |           | 以只读方式挂载容器的根文件系统 Mount the container's root filesystem as read only |
| `--restart`               | `no`      | 容器退出时应用的重启策略 Restart policy to apply when a container exits |
| `--rm`                    |           | 容器退出时自动删除该容器及其相关的匿名卷 Automatically remove the container and its associated anonymous volumes when it exits |
| `--runtime`               |           | 此容器使用的运行时 Runtime to use for this container         |
| `--security-opt`          |           | 安全选项 Security Options                                    |
| `--shm-size`              |           | /dev/shm 的大小 Size of /dev/shm                             |
| `--stop-signal`           |           | 停止容器的信号 Signal to stop the container                  |
| `--stop-timeout`          |           | API 1.25+ 停止容器的超时时间（以秒为单位） API 1.25+ Timeout (in seconds) to stop a container |
| `--storage-opt`           |           | 容器的存储驱动选项 Storage driver options for the container  |
| `--sysctl`                |           | Sysctl 选项 Sysctl options                                   |
| `--tmpfs`                 |           | 挂载一个 tmpfs 目录 Mount a tmpfs directory                  |
| `-t, --tty`               |           | 分配一个伪终端 Allocate a pseudo-TTY                         |
| `--ulimit`                |           | Ulimit 选项 Ulimit options                                   |
| `-u, --user`              |           | 用户名或 UID -  Username or UID (format: <name\|uid>[:<group\|gid>]) |
| `--userns`                |           | 使用的用户命名空间 User namespace to use                     |
| `--uts`                   |           | 使用的 UTS 命名空间 UTS namespace to use                     |
| `-v, --volume`            |           | 绑定挂载一个卷 Bind mount a volume                           |
| `--volume-driver`         |           | 容器的可选卷驱动 Optional volume driver for the container    |
| `--volumes-from`          |           | 从指定的容器挂载卷 Mount volumes from the specified container(s) |
| `-w, --workdir`           |           | 容器内的工作目录 Working directory inside the container      |

## Examples

### 创建并启动一个容器 Create and start a container

The following example creates an interactive container with a pseudo-TTY attached, then starts the container and attaches to it:

​	以下示例创建一个带有伪终端的交互式容器，然后启动该容器并附加到它上面：



```console
$ docker container create -i -t --name mycontainer alpine
6d8af538ec541dd581ebc2a24153a28329acb5268abe5ef868c1f1a261221752

$ docker container start --attach -i mycontainer
/ # echo hello world
hello world
```

The above is the equivalent of a `docker run`:

​	以上相当于一个 `docker run`：



```console
$ docker run -it --name mycontainer2 alpine
/ # echo hello world
hello world
```

### 初始化卷 Initialize volumes

Container volumes are initialized during the `docker create` phase (i.e., `docker run` too). For example, this allows you to `create` the `data` volume container, and then use it from another container:

​	在 `docker create` 阶段（即 `docker run` 也一样）初始化容器卷。例如，这允许您创建 `data` 卷容器，然后从另一个容器使用它：



```console
$ docker create -v /data --name data ubuntu

240633dfbb98128fa77473d3d9018f6123b99c454b3251427ae190a7d951ad57

$ docker run --rm --volumes-from data ubuntu ls -la /data

total 8
drwxr-xr-x  2 root root 4096 Dec  5 04:10 .
drwxr-xr-x 48 root root 4096 Dec  5 04:11 ..
```

Similarly, `create` a host directory bind mounted volume container, which canhen be used from the subsequent container:

​	同样，创建一个绑定挂载主机目录的卷容器，然后可以从后续容器中使用：



```console
$ docker create -v /home/docker:/docker --name docker ubuntu

9aa88c08f319cd1e4515c3c46b0de7cc9aa75e878357b1e96f91e2c773029f03

$ docker run --rm --volumes-from docker ubuntu ls -la /docker

total 20
drwxr-sr-x  5 1000 staff  180 Dec  5 04:00 .
drwxr-xr-x 48 root root  4096 Dec  5 04:13 ..
-rw-rw-r--  1 1000 staff 3833 Dec  5 04:01 .ash_history
-rw-r--r--  1 1000 staff  446 Nov 28 11:51 .ashrc
-rw-r--r--  1 1000 staff   25 Dec  5 04:00 .gitconfig
drwxr-sr-x  3 1000 staff   60 Dec  1 03:28 .local
-rw-r--r--  1 1000 staff  920 Nov 28 11:51 .profile
drwx--S---  2 1000 staff  460 Dec  5 00:51 .ssh
drwxr-xr-x 32 1000 staff 1140 Dec  5 04:01 docker
```
