+++
title = "运行容器"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/containers/run/](https://docs.docker.com/engine/containers/run/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Running containers - 运行容器

Docker runs processes in isolated containers. A container is a process which runs on a host. The host may be local or remote. When you execute `docker run`, the container process that runs is isolated in that it has its own file system, its own networking, and its own isolated process tree separate from the host.

​	Docker 在隔离的容器中运行进程。容器是在主机上运行的一个进程，主机可以是本地或远程的。当您执行 `docker run` 命令时，运行的容器进程是隔离的，具有自己的文件系统、网络和独立于主机的进程树。

This page details how to use the `docker run` command to run containers.

​	本页详细介绍了如何使用 `docker run` 命令运行容器。

## 基本格式 General form

A `docker run` command takes the following form:

​	`docker run` 命令的格式如下：

```console
$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

The `docker run` command must specify an [image reference](https://docs.docker.com/engine/containers/run/#image-references) to create the container from.

​	`docker run` 命令必须指定一个[镜像引用](https://docs.docker.com/engine/containers/run/#image-references)来创建容器。

### 镜像引用 Image references

The image reference is the name and version of the image. You can use the image reference to create or run a container based on an image.

​	镜像引用是镜像的名称和版本。可以使用镜像引用基于镜像创建或运行容器。

- `docker run IMAGE[:TAG][@DIGEST]`

- `docker create IMAGE[:TAG][@DIGEST]`

An image tag is the image version, which defaults to `latest` when omitted. Use the tag to run a container from specific version of an image. For example, to run version `24.04` of the `ubuntu` image: `docker run ubuntu:24.04`.

​	镜像标签是镜像的版本，当省略时默认使用 `latest`。使用标签可以从镜像的特定版本运行容器。例如，要运行 `ubuntu` 镜像的 `24.04` 版本：`docker run ubuntu:24.04`。

#### 镜像摘要 Image digests

Images using the v2 or later image format have a content-addressable identifier called a digest. As long as the input used to generate the image is unchanged, the digest value is predictable.

​	使用 v2 或更高版本格式的镜像具有称为摘要的内容地址标识符。只要用于生成镜像的输入不变，摘要值是可预测的。

The following example runs a container from the `alpine` image with the `sha256:9cacb71397b640eca97488cf08582ae4e4068513101088e9f96c9814bfda95e0` digest:

​	以下示例使用摘要 `sha256:9cacb71397b640eca97488cf08582ae4e4068513101088e9f96c9814bfda95e0` 从 `alpine` 镜像运行一个容器：

```console
$ docker run alpine@sha256:9cacb71397b640eca97488cf08582ae4e4068513101088e9f96c9814bfda95e0 date
```

### Options

`[OPTIONS]` let you configure options for the container. For example, you can give the container a name (`--name`), or run it as a background process (`-d`). You can also set options to control things like resource constraints and networking.

​	`[OPTIONS]` 用于配置容器的选项。例如，可以为容器指定一个名称（`--name`），或将其作为后台进程运行（`-d`）。还可以设置选项来控制资源限制和网络等内容。

### Commands and arguments

You can use the `[COMMAND]` and `[ARG...]` positional arguments to specify commands and arguments for the container to run when it starts up. For example, you can specify `sh` as the `[COMMAND]`, combined with the `-i` and `-t` flags, to start an interactive shell in the container (if the image you select has an `sh` executable on `PATH`).

​	可以使用 `[COMMAND]` 和 `[ARG...]` 参数指定容器启动时运行的命令和参数。例如，可以将 `sh` 作为 `[COMMAND]`，结合 `-i` 和 `-t` 标志，在容器中启动交互式 shell（前提是所选镜像的 `PATH` 中有 `sh` 可执行文件）。

```console
$ docker run -it IMAGE sh
```

> **Note**
>
> Depending on your Docker system configuration, you may be required to preface the `docker run` command with `sudo`. To avoid having to use `sudo` with the `docker` command, your system administrator can create a Unix group called `docker` and add users to it. For more information about this configuration, refer to the Docker installation documentation for your operating system.
>
> ​	取决于 Docker 系统配置，可能需要在 `docker run` 命令前添加 `sudo`。为避免每次都使用 `sudo`，系统管理员可以创建一个名为 `docker` 的 Unix 组并将用户添加到该组。有关此配置的更多信息，请参考您的操作系统的 Docker 安装文档。

## Foreground and background

When you start a container, the container runs in the foreground by default. If you want to run the container in the background instead, you can use the `--detach` (or `-d`) flag. This starts the container without occupying your terminal window.

​	默认情况下，启动容器时它会在前台运行。如果希望容器在后台运行，可以使用 `--detach`（或 `-d`）标志。这会启动容器而不占用您的终端窗口。

```console
$ docker run -d IMAGE
```

While the container runs in the background, you can interact with the container using other CLI commands. For example, `docker logs` lets you view the logs for the container, and `docker attach` brings it to the foreground.

​	当容器在后台运行时，可以使用其他 CLI 命令与其交互。例如，`docker logs` 可查看容器的日志，`docker attach` 可将容器带到前台。



```console
$ docker run -d nginx
0246aa4d1448a401cabd2ce8f242192b6e7af721527e48a810463366c7ff54f1
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS     NAMES
0246aa4d1448   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 1 second   80/tcp    pedantic_liskov
$ docker logs -n 5 0246aa4d1448
2023/11/06 15:58:23 [notice] 1#1: start worker process 33
2023/11/06 15:58:23 [notice] 1#1: start worker process 34
2023/11/06 15:58:23 [notice] 1#1: start worker process 35
2023/11/06 15:58:23 [notice] 1#1: start worker process 36
2023/11/06 15:58:23 [notice] 1#1: start worker process 37
$ docker attach 0246aa4d1448
^C
2023/11/06 15:58:40 [notice] 1#1: signal 2 (SIGINT) received, exiting
...
```

For more information about `docker run` flags related to foreground and background modes, see:

​	有关前台和后台模式的 `docker run` 标志的更多信息，请参阅：

- [`docker run --detach`](https://docs.docker.com/reference/cli/docker/container/run/#detach): run container in background
  - [`docker run --detach`](https://docs.docker.com/reference/cli/docker/container/run/#detach)：在后台运行容器

- [`docker run --attach`](https://docs.docker.com/reference/cli/docker/container/run/#attach): attach to `stdin`, `stdout`, and `stderr`
  - [`docker run --attach`](https://docs.docker.com/reference/cli/docker/container/run/#attach)：附加到 `stdin`、`stdout` 和 `stderr`

- [`docker run --tty`](https://docs.docker.com/reference/cli/docker/container/run/#tty): allocate a pseudo-tty
  - [`docker run --tty`](https://docs.docker.com/reference/cli/docker/container/run/#tty)：分配一个伪终端

- [`docker run --interactive`](https://docs.docker.com/reference/cli/docker/container/run/#interactive): keep `stdin` open even if not attached
  - [`docker run --interactive`](https://docs.docker.com/reference/cli/docker/container/run/#interactive)：即使未连接也保持 `stdin` 打开


For more information about re-attaching to a background container, see [`docker attach`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerattach" >}}).

​	有关重新附加到后台容器的更多信息，请参阅 `docker attach`。

## 容器标识 Container identification

You can identify a container in three ways:

​	您可以通过以下三种方式识别容器：

| Identifier type 标识符类型 | Example value 示例值                                         |
| :------------------------- | :----------------------------------------------------------- |
| UUID long identifier       | `f78375b1c487e03c9438c729345e54db9d20cfa2ac1fc3494b6eb60872e74778` |
| UUID short identifier      | `f78375b1c487`                                               |
| Name                       | `evil_ptolemy`                                               |

The UUID identifier is a random ID assigned to the container by the daemon.

​	UUID 标识符是由守护进程分配给容器的随机 ID。

The daemon generates a random string name for containers automatically. You can also defined a custom name using [the `--name` flag](https://docs.docker.com/reference/cli/docker/container/run/#name). Defining a `name` can be a handy way to add meaning to a container. If you specify a `name`, you can use it when referring to the container in a user-defined network. This works for both background and foreground Docker containers.

​	守护进程会自动生成一个随机字符串作为容器的名称。您还可以使用 [`--name` 标志](https://docs.docker.com/reference/cli/docker/container/run/#name) 自定义容器名称。定义 `name` 可以为容器赋予意义。指定 `name` 后，您可以在用户定义的网络中引用容器，无论是前台还是后台的 Docker 容器都可以使用此方法。

A container identifier is not the same thing as an image reference. The image reference specifies which image to use when you run a container. You can't run `docker exec nginx:alpine sh` to open a shell in a container based on the `nginx:alpine` image, because `docker exec` expects a container identifier (name or ID), not an image.

​	容器标识符与镜像引用不同。镜像引用用于指定运行容器时要使用的镜像。您不能使用 `docker exec nginx:alpine sh` 打开基于 `nginx:alpine` 镜像的容器的 shell，因为 `docker exec` 期望一个容器标识符（名称或 ID），而不是镜像。

While the image used by a container is not an identifier for the container, you find out the IDs of containers using an image by using the `--filter` flag. For example, the following `docker ps` command gets the IDs of all running containers based on the `nginx:alpine` image:

​	尽管容器使用的镜像不是容器的标识符，但您可以使用 `--filter` 标志根据镜像找到容器的 ID。例如，以下 `docker ps` 命令获取所有基于 `nginx:alpine` 镜像的运行容器的 ID：

```console
$ docker ps -q --filter ancestor=nginx:alpine
```

For more information about using filters, see [Filtering](https://docs.docker.com/config/filter/).

​	有关使用过滤器的更多信息，请参阅 [过滤器](https://docs.docker.com/config/filter/)。

## 容器网络 Container networking

Containers have networking enabled by default, and they can make outgoing connections. If you're running multiple containers that need to communicate with each other, you can create a custom network and attach the containers to the network.

​	默认情况下，容器启用网络，并且可以进行出站连接。如果您运行的多个容器需要相互通信，可以创建自定义网络并将容器连接到该网络。

When multiple containers are attached to the same custom network, they can communicate with each other using the container names as a DNS hostname. The following example creates a custom network named `my-net`, and runs two containers that attach to the network.

​	当多个容器连接到同一自定义网络时，它们可以使用容器名称作为 DNS 主机名相互通信。以下示例创建一个名为 `my-net` 的自定义网络，并运行两个连接到该网络的容器：



```console
$ docker network create my-net
$ docker run -d --name web --network my-net nginx:alpine
$ docker run --rm -it --network my-net busybox
/ # ping web
PING web (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.326 ms
64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.257 ms
64 bytes from 172.18.0.2: seq=2 ttl=64 time=0.281 ms
^C
--- web ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.257/0.288/0.326 ms
```

For more information about container networking, see [Networking overview](https://docs.docker.com/network/)

​	有关容器网络的更多信息，请参阅 [网络概述](https://docs.docker.com/network/)。

## 文件系统挂载 Filesystem mounts

By default, the data in a container is stored in an ephemeral, writable container layer. Removing the container also removes its data. If you want to use persistent data with containers, you can use filesystem mounts to store the data persistently on the host system. Filesystem mounts can also let you share data between containers and the host.

​	默认情况下，容器中的数据存储在临时的、可写的容器层中。删除容器也会删除其数据。如果您希望容器使用持久数据，可以使用文件系统挂载在主机系统上持久存储数据。文件系统挂载还可以让容器和主机之间共享数据。

Docker supports two main categories of mounts:

​	Docker 支持两种主要类型的挂载：

- Volume mounts 卷挂载

- Bind mounts 绑定挂载

Volume mounts are great for persistently storing data for containers, and for sharing data between containers. Bind mounts, on the other hand, are for sharing data between a container and the host.

​	卷挂载非常适合为容器持久存储数据以及在容器之间共享数据。另一方面，绑定挂载适用于在容器和主机之间共享数据。

You can add a filesystem mount to a container using the `--mount` flag for the `docker run` command.

​	可以使用 `docker run` 命令的 `--mount` 标志向容器添加文件系统挂载。

The following sections show basic examples of how to create volumes and bind mounts. For more in-depth examples and descriptions, refer to the section of the [storage section](https://docs.docker.com/storage/) in the documentation.

​	以下部分展示了如何创建卷和绑定挂载的基本示例。有关更深入的示例和描述，请参阅文档中的 [存储部分](https://docs.docker.com/storage/)。

### 卷挂载 Volume mounts

To create a volume mount:

​	创建卷挂载：

```console
$ docker run --mount source=VOLUME_NAME,target=[PATH] [IMAGE] [COMMAND...]
```

The `--mount` flag takes two parameters in this case: `source` and `target`. The value for the `source` parameter is the name of the volume. The value of `target` is the mount location of the volume inside the container. Once you've created the volume, any data you write to the volume is persisted, even if you stop or remove the container:

​	在这种情况下，`--mount` 标志需要两个参数：`source` 和 `target`。`source` 参数的值是卷的名称，`target` 是容器内卷的挂载位置。一旦创建了卷，写入该卷的任何数据都会被持久化，即使停止或删除容器：

```console
$ docker run --rm --mount source=my_volume,target=/foo busybox \
  echo "hello, volume!" > /foo/hello.txt
$ docker run --mount source=my_volume,target=/bar busybox
  cat /bar/hello.txt
hello, volume!
```

The `target` must always be an absolute path, such as `/src/docs`. An absolute path starts with a `/` (forward slash). Volume names must start with an alphanumeric character, followed by `a-z0-9`, `_` (underscore), `.` (period) or `-` (hyphen).

​	`target` 必须始终是绝对路径，例如 `/src/docs`。绝对路径以 `/`（正斜杠）开头。卷名称必须以字母数字字符开头，后跟 `a-z0-9`、`_`（下划线）、`.`（点）或 `-`（连字符）。

### 绑定挂载 Bind mounts

To create a bind mount:

​	创建绑定挂载：

```console
$ docker run -it --mount type=bind,source=[PATH],target=[PATH] busybox
```

In this case, the `--mount` flag takes three parameters. A type (`bind`), and two paths. The `source` path is a the location on the host that you want to bind mount into the container. The `target` path is the mount destination inside the container.

​	在这种情况下，`--mount` 标志需要三个参数：一种类型（`bind`）和两个路径。`source` 路径是主机上要绑定到容器的路径，而 `target` 路径是容器内部的挂载目标。

Bind mounts are read-write by default, meaning that you can both read and write files to and from the mounted location from the container. Changes that you make, such as adding or editing files, are reflected on the host filesystem:

​	默认情况下，绑定挂载是读写的，这意味着可以从容器中读取和写入文件，所做的更改（如添加或编辑文件）会反映在主机文件系统上：

```console
$ docker run -it --mount type=bind,source=.,target=/foo busybox
/ # echo "hello from container" > /foo/hello.txt
/ # exit
$ cat hello.txt
hello from container
```

## 退出状态 Exit status

The exit code from `docker run` gives information about why the container failed to run or why it exited. The following sections describe the meanings of different container exit codes values.

​	`docker run` 的退出代码提供了有关容器未能运行或退出的原因的信息。以下部分描述了不同容器退出代码的含义。

### 125

Exit code `125` indicates that the error is with Docker daemon itself.

​	退出代码 `125` 表示错误发生在 Docker 守护进程本身：



```console
$ docker run --foo busybox; echo $?

flag provided but not defined: --foo
See 'docker run --help'.
125
```

### 126

Exit code `126` indicates that the specified contained command can't be invoked. The container command in the following example is: `/etc; echo $?`.

​	退出代码 `126` 表示指定的容器命令无法调用。以下示例中的容器命令是：`/etc; echo $?`。



```console
$ docker run busybox /etc; echo $?

docker: Error response from daemon: Container command '/etc' could not be invoked.
126
```

### 127

Exit code `127` indicates that the contained command can't be found.

​	退出代码 `127` 表示找不到容器命令。



```console
$ docker run busybox foo; echo $?

docker: Error response from daemon: Container command 'foo' not found or does not exist.
127
```

### 其他退出代码 Other exit codes

Any exit code other than `125`, `126`, and `127` represent the exit code of the provided container command.

​	任何其他退出代码（除 `125`、`126` 和 `127` 外）表示提供的容器命令的退出代码：



```console
$ docker run busybox /bin/sh -c 'exit 3'
$ echo $?
3
```

## 运行时资源限制 Runtime constraints on resources

The operator can also adjust the performance parameters of the container:

​	操作人员还可以调整容器的性能参数：

| Option                     | Description 描述                                             |
| :------------------------- | :----------------------------------------------------------- |
| `-m`, `--memory=""`        | 内存限制（格式：`<number>[<unit>]`）。`number` 是正整数，`unit` 可以是 `b`、`k`、`m` 或 `g`。最小值为 6M。 Memory limit (format: `<number>[<unit>]`). Number is a positive integer. Unit can be one of `b`, `k`, `m`, or `g`. Minimum is 6M. |
| `--memory-swap=""`         | 总内存限制（内存+交换，格式：`<number>[<unit>]`）。`number` 是正整数，`unit` 可以是 `b`、`k`、`m` 或 `g`。 Total memory limit (memory + swap, format: `<number>[<unit>]`). Number is a positive integer. Unit can be one of `b`, `k`, `m`, or `g`. |
| `--memory-reservation=""`  | 内存软限制（格式：`<number>[<unit>]`）。`number` 是正整数，`unit` 可以是 `b`、`k`、`m` 或 `g`。 Memory soft limit (format: `<number>[<unit>]`). Number is a positive integer. Unit can be one of `b`, `k`, `m`, or `g`. |
| `--kernel-memory=""`       | 内核内存限制（格式：`<number>[<unit>]`）。`number` 是正整数，`unit` 可以是 `b`、`k`、`m` 或 `g`。最小值为 4M。 Kernel memory limit (format: `<number>[<unit>]`). Number is a positive integer. Unit can be one of `b`, `k`, `m`, or `g`. Minimum is 4M. |
| `-c`, `--cpu-shares=0`     | CPU 共享权重（相对权重） CPU shares (relative weight)        |
| `--cpus=0.000`             | CPU 数量。`number` 是小数，0.000 表示无限制。 Number of CPUs. Number is a fractional number. 0.000 means no limit. |
| `--cpu-period=0`           | 限制 CPU CFS（完全公平调度器）周期 Limit the CPU CFS (Completely Fair Scheduler) period |
| `--cpuset-cpus=""`         | 允许执行的 CPU（如 0-3，0,1） CPUs in which to allow execution (0-3, 0,1) |
| `--cpuset-mems=""`         | 允许执行的内存节点（如 0-3，0,1）。仅在 NUMA 系统上有效。 Memory nodes (MEMs) in which to allow execution (0-3, 0,1). Only effective on NUMA systems. |
| `--cpu-quota=0`            | 限制 CPU CFS 配额 Limit the CPU CFS (Completely Fair Scheduler) quota |
| `--cpu-rt-period=0`        | 限制 CPU 实时周期（微秒）。需要设置父 cgroups，且不能超过父级。还需检查 rtprio ulimits。 Limit the CPU real-time period. In microseconds. Requires parent cgroups be set and cannot be higher than parent. Also check rtprio ulimits. |
| `--cpu-rt-runtime=0`       | 限制 CPU 实时运行时间（微秒）。需要设置父 cgroups，且不能超过父级。还需检查 rtprio ulimits。 Limit the CPU real-time runtime. In microseconds. Requires parent cgroups be set and cannot be higher than parent. Also check rtprio ulimits. |
| `--blkio-weight=0`         | 块 IO 权重（相对权重），接受 10 到 1000 之间的权重值。 Block IO weight (relative weight) accepts a weight value between 10 and 1000. |
| `--blkio-weight-device=""` | 块 IO 权重（相对设备权重，格式：`DEVICE_NAME:WEIGHT`） Block IO weight (relative device weight, format: `DEVICE_NAME:WEIGHT`) |
| `--device-read-bps=""`     | 限制设备的读取速率（格式：`<device-path>:<number>[<unit>]`）。`number` 是正整数，`unit` 可以是 `kb`、`mb` 或 `gb`。 Limit read rate from a device (format: `<device-path>:<number>[<unit>]`). Number is a positive integer. Unit can be one of `kb`, `mb`, or `gb`. |
| `--device-write-bps=""`    | 限制设备的写入速率（格式：`<device-path>:<number>[<unit>]`）。`number` 是正整数，`unit` 可以是 `kb`、`mb` 或 `gb`。 Limit write rate to a device (format: `<device-path>:<number>[<unit>]`). Number is a positive integer. Unit can be one of `kb`, `mb`, or `gb`. |
| `--device-read-iops=""`    | 限制设备的读取 IOPS（每秒 IO 操作数）（格式：`<device-path>:<number>`）。`number` 是正整数。 Limit read rate (IO per second) from a device (format: `<device-path>:<number>`). Number is a positive integer. |
| `--device-write-iops=""`   | 限制设备的写入 IOPS（格式：`<device-path>:<number>`）。`number` 是正整数。 Limit write rate (IO per second) to a device (format: `<device-path>:<number>`). Number is a positive integer. |
| `--oom-kill-disable=false` | 是否禁用容器的 OOM 杀手。 Whether to disable OOM Killer for the container or not. |
| `--oom-score-adj=0`        | 调整容器的 OOM 优先级（-1000 到 1000）。 Tune container's OOM preferences (-1000 to 1000) |
| `--memory-swappiness=""`   | 调整容器的内存交换行为，接受 0 到 100 之间的整数。 Tune a container's memory swappiness behavior. Accepts an integer between 0 and 100. |
| `--shm-size=""`            | `/dev/shm` 的大小。格式为 `<number><unit>`。`number` 必须大于 `0`。`unit` 可选，可以是 `b`（字节）、`k`（千字节）、`m`（兆字节）或 `g`（千兆字节）。如果省略单位，系统默认使用字节。如果完全省略大小，系统默认使用 `64m`。 Size of `/dev/shm`. The format is `<number><unit>`. `number` must be greater than `0`. Unit is optional and can be `b` (bytes), `k` (kilobytes), `m` (megabytes), or `g` (gigabytes). If you omit the unit, the system uses bytes. If you omit the size entirely, the system uses `64m`. |

### 用户内存限制 User memory constraints

We have four ways to set user memory usage:

​	我们有四种方式设置用户内存使用限制：

| Option                                    | Result 结果                                                  |
| ----------------------------------------- | ------------------------------------------------------------ |
| **memory=inf, memory-swap=inf** (default) | 容器没有内存限制，可以根据需要使用尽可能多的内存。 There is no memory limit for the container. The container can use as much memory as needed. |
| **memory=L<inf, memory-swap=inf**         | （指定内存并将 memory-swap 设置为 `-1`）容器最多只能使用 L 字节内存，但可以根据需要使用尽可能多的交换内存（如果主机支持交换内存）。 (specify memory and set memory-swap as `-1`) The container is not allowed to use more than L bytes of memory, but can use as much swap as is needed (if the host supports swap memory). |
| **memory=L<inf, memory-swap=2\*L**        | （指定内存但不指定 memory-swap）容器最多可以使用 L 字节内存，交换加上内存使用量为该值的两倍。(specify memory without memory-swap) The container is not allowed to use more than L bytes of memory, swap *plus* memory usage is double of that. |
| **memory=L<inf, memory-swap=S<inf, L<=S** | （同时指定内存和 memory-swap）容器最多可以使用 L 字节内存，交换加上内存使用量被限制为 S。 (specify both memory and memory-swap) The container is not allowed to use more than L bytes of memory, swap *plus* memory usage is limited by S. |

Examples:

​	示例：

```console
$ docker run -it ubuntu:24.04 /bin/bash
```

We set nothing about memory, this means the processes in the container can use as much memory and swap memory as they need.

​	未设置任何内存限制，这意味着容器中的进程可以使用所需的内存和交换内存。



```console
$ docker run -it -m 300M --memory-swap -1 ubuntu:24.04 /bin/bash
```

We set memory limit and disabled swap memory limit, this means the processes in the container can use 300M memory and as much swap memory as they need (if the host supports swap memory).

​	我们设置了内存限制并禁用了交换内存限制，这意味着容器中的进程可以使用 300M 内存，并且可以根据需要使用交换内存（如果主机支持交换内存）。

```console
$ docker run -it -m 300M ubuntu:24.04 /bin/bash
```

We set memory limit only, this means the processes in the container can use 300M memory and 300M swap memory, by default, the total virtual memory size (--memory-swap) will be set as double of memory, in this case, memory + swap would be 2*300M, so processes can use 300M swap memory as well.

​	仅设置内存限制，这意味着容器中的进程可以使用 300M 内存和 300M 交换内存。默认情况下，总虚拟内存大小（--memory-swap）将被设置为内存的两倍，在这种情况下，内存+交换为 2*300M，因此进程可以使用 300M 交换内存。

```console
$ docker run -it -m 300M --memory-swap 1G ubuntu:24.04 /bin/bash
```

We set both memory and swap memory, so the processes in the container can use 300M memory and 700M swap memory.

​	我们同时设置了内存和交换内存，因此容器中的进程可以使用 300M 内存和 700M 交换内存。

Memory reservation is a kind of memory soft limit that allows for greater sharing of memory. Under normal circumstances, containers can use as much of the memory as needed and are constrained only by the hard limits set with the `-m`/`--memory` option. When memory reservation is set, Docker detects memory contention or low memory and forces containers to restrict their consumption to a reservation limit.

​	内存预留是一种内存软限制，允许更大的内存共享。在正常情况下，容器可以根据需要使用尽可能多的内存，仅受 `-m`/`--memory` 选项设置的硬限制约束。当设置了内存预留时，Docker 检测到内存争用或内存不足时，会强制容器限制其消耗量在预留限制之内。

Always set the memory reservation value below the hard limit, otherwise the hard limit takes precedence. A reservation of 0 is the same as setting no reservation. By default (without reservation set), memory reservation is the same as the hard memory limit.

​	始终将内存预留值设置为低于硬限制，否则硬限制优先于预留。预留为 0 等同于未设置预留。默认情况下（未设置预留），内存预留等于硬内存限制。

Memory reservation is a soft-limit feature and does not guarantee the limit won't be exceeded. Instead, the feature attempts to ensure that, when memory is heavily contended for, memory is allocated based on the reservation hints/setup.

​	内存预留是一种软限制特性，并不保证不会超过限制。相反，该特性尝试确保在内存高度竞争时，基于预留提示/设置分配内存。

The following example limits the memory (`-m`) to 500M and sets the memory reservation to 200M.

​	以下示例将内存限制（`-m`）设置为 500M，并将内存预留设置为 200M：

```console
$ docker run -it -m 500M --memory-reservation 200M ubuntu:24.04 /bin/bash
```

Under this configuration, when the container consumes memory more than 200M and less than 500M, the next system memory reclaim attempts to shrink container memory below 200M.

​	在此配置下，当容器消耗的内存超过 200M 且小于 500M 时，下次系统内存回收会尝试将容器内存减少到 200M 以下。

The following example set memory reservation to 1G without a hard memory limit.

​	以下示例在没有硬内存限制的情况下将内存预留设置为 1G：

```console
$ docker run -it --memory-reservation 1G ubuntu:24.04 /bin/bash
```

The container can use as much memory as it needs. The memory reservation setting ensures the container doesn't consume too much memory for long time, because every memory reclaim shrinks the container's consumption to the reservation.

​	容器可以根据需要使用尽可能多的内存。内存预留设置确保容器不会长期消耗过多内存，因为每次内存回收都会将容器的消耗量减少到预留值。

By default, kernel kills processes in a container if an out-of-memory (OOM) error occurs. To change this behaviour, use the `--oom-kill-disable` option. Only disable the OOM killer on containers where you have also set the `-m/--memory` option. If the `-m` flag is not set, this can result in the host running out of memory and require killing the host's system processes to free memory.

​	默认情况下，如果发生内存不足（OOM）错误，内核会终止容器中的进程。要更改此行为，请使用 `--oom-kill-disable` 选项。仅在已设置 `-m/--memory` 选项的容器上禁用 OOM 杀手。如果未设置 `-m` 标志，这可能导致主机内存不足，并需要终止主机系统进程以释放内存。

The following example limits the memory to 100M and disables the OOM killer for this container:

​	以下示例将内存限制为 100M 并为此容器禁用了 OOM 杀手：



```console
$ docker run -it -m 100M --oom-kill-disable ubuntu:24.04 /bin/bash
```

The following example, illustrates a dangerous way to use the flag:

​	以下示例演示了禁用 OOM 杀手的危险用法：



```console
$ docker run -it --oom-kill-disable ubuntu:24.04 /bin/bash
```

The container has unlimited memory which can cause the host to run out memory and require killing system processes to free memory. The `--oom-score-adj` parameter can be changed to select the priority of which containers will be killed when the system is out of memory, with negative scores making them less likely to be killed, and positive scores more likely.

​	该容器具有无限制的内存，这可能导致主机内存耗尽，并需要终止系统进程以释放内存。可以更改 `--oom-score-adj` 参数来选择系统内存不足时将被终止的容器优先级，负分数使其不易被终止，正分数则更易被终止。

### 内核内存限制 Kernel memory constraints

Kernel memory is fundamentally different than user memory as kernel memory can't be swapped out. The inability to swap makes it possible for the container to block system services by consuming too much kernel memory. Kernel memory includes：

​	内核内存与用户内存有根本的不同，因为内核内存不能被交换出去。无法交换内存使得容器可能会通过消耗过多的内核内存而阻塞系统服务。内核内存包括：

- stack pages

- slab pages
- sockets memory pressure 套接字内存压力
- tcp memory pressure

You can setup kernel memory limit to constrain these kinds of memory. For example, every process consumes some stack pages. By limiting kernel memory, you can prevent new processes from being created when the kernel memory usage is too high.

​	您可以设置内核内存限制来限制这些类型的内存。例如，每个进程会消耗一些堆栈页。通过限制内核内存，可以防止当内核内存使用过高时创建新进程。

Kernel memory is never completely independent of user memory. Instead, you limit kernel memory in the context of the user memory limit. Assume "U" is the user memory limit and "K" the kernel limit. There are three possible ways to set limits:

​	内核内存永远不会完全独立于用户内存。相反，您可以在用户内存限制的上下文中限制内核内存。假设 "U" 是用户内存限制，"K" 是内核限制。有三种可能的限制设置：

| Option                        | Result 结果                                                  |
| ----------------------------- | ------------------------------------------------------------ |
| **U != 0, K = inf** (default) | 这是在使用内核内存之前已经存在的标准内存限制机制，完全忽略内核内存。 This is the standard memory limitation mechanism already present before using kernel memory. Kernel memory is completely ignored. |
| **U != 0, K < U**             | 内核内存是用户内存的一个子集。这种设置适用于每个 cgroup 的内存总量超额分配的部署。强烈不建议超额分配内核内存限制，因为系统仍可能耗尽不可回收的内存。在这种情况下，可以配置 K，使所有组的总和不超过总内存。然后，您可以自由设置 U，但需以系统服务质量为代价。 Kernel memory is a subset of the user memory. This setup is useful in deployments where the total amount of memory per-cgroup is overcommitted. Overcommitting kernel memory limits is definitely not recommended, since the box can still run out of non-reclaimable memory. In this case, you can configure K so that the sum of all groups is never greater than the total memory. Then, freely set U at the expense of the system's service quality. |
| **U != 0, K > U**             | 由于内核内存使用量也会被计入用户计数器，并对容器的两种内存触发回收，因此这种配置为管理员提供了统一的内存视图。对于那些仅希望跟踪内核内存使用情况的用户来说也很有用。 Since kernel memory charges are also fed to the user counter and reclamation is triggered for the container for both kinds of memory. This configuration gives the admin a unified view of memory. It is also useful for people who just want to track kernel memory usage. |

Examples:

​	示例：

```console
$ docker run -it -m 500M --kernel-memory 50M ubuntu:24.04 /bin/bash
```

We set memory and kernel memory, so the processes in the container can use 500M memory in total, in this 500M memory, it can be 50M kernel memory tops.

​	我们设置了内存和内核内存，因此容器中的进程最多可以使用 500M 内存，其中最多可包含 50M 内核内存。

```console
$ docker run -it --kernel-memory 50M ubuntu:24.04 /bin/bash
```

We set kernel memory without **-m**, so the processes in the container can use as much memory as they want, but they can only use 50M kernel memory.

​	我们仅设置了内核内存，没有 **-m**，因此容器中的进程可以根据需要使用尽可能多的内存，但只能使用 50M 内核内存。

### 交换性限制 Swappiness constraint

By default, a container's kernel can swap out a percentage of anonymous pages. To set this percentage for a container, specify a `--memory-swappiness` value between 0 and 100. A value of 0 turns off anonymous page swapping. A value of 100 sets all anonymous pages as swappable. By default, if you are not using `--memory-swappiness`, memory swappiness value will be inherited from the parent.

​	默认情况下，容器的内核可以交换一部分匿名页面。要为容器设置此百分比，请指定一个介于 0 和 100 之间的 `--memory-swappiness` 值。值 0 表示关闭匿名页面交换，值 100 表示所有匿名页面都可交换。如果不使用 `--memory-swappiness`，则交换性值将继承自父级。

For example, you can set:

​	例如，可以设置：

```console
$ docker run -it --memory-swappiness=0 ubuntu:24.04 /bin/bash
```

Setting the `--memory-swappiness` option is helpful when you want to retain the container's working set and to avoid swapping performance penalties.

​	设置 `--memory-swappiness` 选项在希望保留容器的工作集并避免交换性能损失时非常有帮助。

### CPU 共享限制 CPU share constraint

By default, all containers get the same proportion of CPU cycles. This proportion can be modified by changing the container's CPU share weighting relative to the weighting of all other running containers.

​	默认情况下，所有容器获得相同的 CPU 周期比例。可以通过更改容器的 CPU 共享权重相对于所有其他运行中的容器来修改这一比例。

To modify the proportion from the default of 1024, use the `-c` or `--cpu-shares` flag to set the weighting to 2 or higher. If 0 is set, the system will ignore the value and use the default of 1024.

​	要从默认的 1024 修改比例，请使用 `-c` 或 `--cpu-shares` 标志将权重设置为 2 或更高。如果设置为 0，系统将忽略该值并使用默认的 1024。

The proportion will only apply when CPU-intensive processes are running. When tasks in one container are idle, other containers can use the left-over CPU time. The actual amount of CPU time will vary depending on the number of containers running on the system.

​	只有在 CPU 密集型进程运行时，才会应用该比例。当一个容器中的任务处于空闲状态时，其他容器可以使用剩余的 CPU 时间。实际 CPU 时间将根据系统上运行的容器数量而有所不同。

For example, consider three containers, one has a cpu-share of 1024 and two others have a cpu-share setting of 512. When processes in all three containers attempt to use 100% of CPU, the first container would receive 50% of the total CPU time. If you add a fourth container with a cpu-share of 1024, the first container only gets 33% of the CPU. The remaining containers receive 16.5%, 16.5% and 33% of the CPU.

​	例如，考虑三个容器，一个的 cpu-share 为 1024，其他两个的 cpu-share 设置为 512。当所有三个容器中的进程都尝试使用 100% 的 CPU 时，第一个容器将获得总 CPU 时间的 50%。如果添加一个 cpu-share 为 1024 的第四个容器，则第一个容器仅获得 33% 的 CPU，其他容器分别获得 16.5%、16.5% 和 33% 的 CPU。

On a multi-core system, the shares of CPU time are distributed over all CPU cores. Even if a container is limited to less than 100% of CPU time, it can use 100% of each individual CPU core.

​	在多核系统上，CPU 时间的共享分布在所有 CPU 核心上。即使容器的 CPU 时间限制低于 100%，它也可以使用每个单独 CPU 核心的 100%。

For example, consider a system with more than three cores. If you start one container `{C0}` with `-c=512` running one process, and another container `{C1}` with `-c=1024` running two processes, this can result in the following division of CPU shares:

​	例如，考虑一个拥有超过三个核心的系统。如果启动一个带有 `-c=512` 的容器 `{C0}` 运行一个进程，和另一个带有 `-c=1024` 的容器 `{C1}` 运行两个进程，可能会导致以下 CPU 共享分配：

```
PID    container	CPU	CPU share
100    {C0}		0	100% of CPU0
101    {C1}		1	100% of CPU1
102    {C1}		2	100% of CPU2
```

### CPU 周期限制 CPU period constraint

The default CPU CFS (Completely Fair Scheduler) period is 100ms. We can use `--cpu-period` to set the period of CPUs to limit the container's CPU usage. And usually `--cpu-period` should work with `--cpu-quota`.

​	默认的 CPU CFS（完全公平调度器）周期为 100 毫秒。可以使用 `--cpu-period` 设置 CPU 周期来限制容器的 CPU 使用量。通常需要与 `--cpu-quota` 一起使用。

Examples:

​	示例：

```console
$ docker run -it --cpu-period=50000 --cpu-quota=25000 ubuntu:24.04 /bin/bash
```

If there is 1 CPU, this means the container can get 50% CPU worth of run-time every 50ms.

​	如果有 1 个 CPU，则这意味着容器每 50 毫秒可以获得 50% 的 CPU 运行时间。

In addition to use `--cpu-period` and `--cpu-quota` for setting CPU period constraints, it is possible to specify `--cpus` with a float number to achieve the same purpose. For example, if there is 1 CPU, then `--cpus=0.5` will achieve the same result as setting `--cpu-period=50000` and `--cpu-quota=25000` (50% CPU).

​	除了使用 `--cpu-period` 和 `--cpu-quota` 设置 CPU 周期限制外，还可以使用浮点数指定 `--cpus` 来实现相同的效果。例如，如果有 1 个 CPU，那么 `--cpus=0.5` 将实现与设置 `--cpu-period=50000` 和 `--cpu-quota=25000`（50% CPU）相同的效果。

The default value for `--cpus` is `0.000`, which means there is no limit.

​	`--cpus` 的默认值为 `0.000`，表示无限制。

For more information, see the [CFS documentation on bandwidth limiting](https://www.kernel.org/doc/Documentation/scheduler/sched-bwc.txt).

​	有关更多信息，请参阅 [CFS 的带宽限制文档](https://www.kernel.org/doc/Documentation/scheduler/sched-bwc.txt)。

### Cpuset 限制 Cpuset constraint

We can set cpus in which to allow execution for containers.

​	可以设置容器允许执行的 CPU。

Examples:

​	示例：

```console
$ docker run -it --cpuset-cpus="1,3" ubuntu:24.04 /bin/bash
```

This means processes in container can be executed on cpu 1 and cpu 3.

​	这意味着容器中的进程只能在 CPU 1 和 CPU 3 上执行。



```console
$ docker run -it --cpuset-cpus="0-2" ubuntu:24.04 /bin/bash
```

This means processes in container can be executed on cpu 0, cpu 1 and cpu 2.

​	这意味着容器中的进程只能在 CPU 0、CPU 1 和 CPU 2 上执行。

We can set mems in which to allow execution for containers. Only effective on NUMA systems.

​	还可以设置容器允许使用的内存节点，仅在 NUMA 系统上有效。

Examples:



```console
$ docker run -it --cpuset-mems="1,3" ubuntu:24.04 /bin/bash
```

This example restricts the processes in the container to only use memory from memory nodes 1 and 3.

​	该示例将容器中的进程限制为只能使用内存节点 1 和 3。



```console
$ docker run -it --cpuset-mems="0-2" ubuntu:24.04 /bin/bash
```

This example restricts the processes in the container to only use memory from memory nodes 0, 1 and 2.

​	该示例将容器中的进程限制为只能使用内存节点 0、1 和 2。

### CPU 配额限制 CPU quota constraint

The `--cpu-quota` flag limits the container's CPU usage. The default 0 value allows the container to take 100% of a CPU resource (1 CPU). The CFS (Completely Fair Scheduler) handles resource allocation for executing processes and is default Linux Scheduler used by the kernel. Set this value to 50000 to limit the container to 50% of a CPU resource. For multiple CPUs, adjust the `--cpu-quota` as necessary. For more information, see the [CFS documentation on bandwidth limiting](https://www.kernel.org/doc/Documentation/scheduler/sched-bwc.txt).

​	`--cpu-quota` 标志限制容器的 CPU 使用量。默认值 0 允许容器占用 100% 的 CPU 资源（1 个 CPU）。CFS（完全公平调度器）处理执行进程的资源分配，并且是 Linux 默认的调度器。将该值设置为 50000 可将容器限制为 50% 的 CPU 资源。对于多个 CPU，可根据需要调整 `--cpu-quota`。有关更多信息，请参阅 [CFS 的带宽限制文档](https://www.kernel.org/doc/Documentation/scheduler/sched-bwc.txt)。

### 块 IO 带宽（Blkio）限制 Block IO bandwidth (Blkio) constraint

By default, all containers get the same proportion of block IO bandwidth (blkio). This proportion is 500. To modify this proportion, change the container's blkio weight relative to the weighting of all other running containers using the `--blkio-weight` flag.

​	默认情况下，所有容器获得相同比例的块 IO 带宽（blkio），比例为 500。可以使用 `--blkio-weight` 标志更改容器的 blkio 权重，相对于所有其他正在运行的容器。

> **Note**
>
> The blkio weight setting is only available for direct IO. Buffered IO is not currently supported.
>
> ​	blkio 权重设置仅适用于直接 IO。目前不支持缓冲 IO。

The `--blkio-weight` flag can set the weighting to a value between 10 to 1000. For example, the commands below create two containers with different blkio weight:

​	`--blkio-weight` 标志可以将权重设置为 10 到 1000 之间的值。例如，以下命令创建了两个具有不同 blkio 权重的容器：

```console
$ docker run -it --name c1 --blkio-weight 300 ubuntu:24.04 /bin/bash
$ docker run -it --name c2 --blkio-weight 600 ubuntu:24.04 /bin/bash
```

If you do block IO in the two containers at the same time, by, for example:

​	如果同时在两个容器中执行块 IO，例如：



```console
$ time dd if=/mnt/zerofile of=test.out bs=1M count=1024 oflag=direct
```

You'll find that the proportion of time is the same as the proportion of blkio weights of the two containers.

​	可以发现两个容器的时间比例与它们的 blkio 权重比例相同。

The `--blkio-weight-device="DEVICE_NAME:WEIGHT"` flag sets a specific device weight. The `DEVICE_NAME:WEIGHT` is a string containing a colon-separated device name and weight. For example, to set `/dev/sda` device weight to `200`:

​	`--blkio-weight-device="DEVICE_NAME:WEIGHT"` 标志设置特定设备的权重。`DEVICE_NAME:WEIGHT` 是包含设备名称和权重的冒号分隔的字符串。例如，将 `/dev/sda` 设备的权重设置为 `200`：

```console
$ docker run -it \
    --blkio-weight-device "/dev/sda:200" \
    ubuntu
```

If you specify both the `--blkio-weight` and `--blkio-weight-device`, Docker uses the `--blkio-weight` as the default weight and uses `--blkio-weight-device` to override this default with a new value on a specific device. The following example uses a default weight of `300` and overrides this default on `/dev/sda` setting that weight to `200`:

​	如果同时指定了 `--blkio-weight` 和 `--blkio-weight-device`，Docker 使用 `--blkio-weight` 作为默认权重，并使用 `--blkio-weight-device` 为特定设备覆盖此默认值。以下示例使用默认权重 `300`，并将 `/dev/sda` 的权重覆盖为 `200`：



```console
$ docker run -it \
    --blkio-weight 300 \
    --blkio-weight-device "/dev/sda:200" \
    ubuntu
```

The `--device-read-bps` flag limits the read rate (bytes per second) from a device. For example, this command creates a container and limits the read rate to `1mb` per second from `/dev/sda`:

​	`--device-read-bps` 标志限制从设备的读取速率（每秒字节数）。例如，此命令创建一个容器，并限制从 `/dev/sda` 的读取速率为 `1mb` 每秒：



```console
$ docker run -it --device-read-bps /dev/sda:1mb ubuntu
```

The `--device-write-bps` flag limits the write rate (bytes per second) to a device. For example, this command creates a container and limits the write rate to `1mb` per second for `/dev/sda`:

​	`--device-write-bps` 标志限制写入设备的速率（每秒字节数）。例如，此命令创建一个容器，并将 `/dev/sda` 的写入速率限制为 `1mb` 每秒：



```console
$ docker run -it --device-write-bps /dev/sda:1mb ubuntu
```

Both flags take limits in the `<device-path>:<limit>[unit]` format. Both read and write rates must be a positive integer. You can specify the rate in `kb` (kilobytes), `mb` (megabytes), or `gb` (gigabytes).

​	两个标志的限制格式为 `<device-path>:<limit>[unit]`。读写速率必须为正整数。可以使用 `kb`（千字节）、`mb`（兆字节）或 `gb`（千兆字节）来指定速率。

The `--device-read-iops` flag limits read rate (IO per second) from a device. For example, this command creates a container and limits the read rate to `1000` IO per second from `/dev/sda`:

​	`--device-read-iops` 标志限制从设备读取的速率（每秒 IO 次数）。例如，以下命令创建一个容器，并将 `/dev/sda` 的读取速率限制为每秒 `1000` IO：

​	

```console
$ docker run -it --device-read-iops /dev/sda:1000 ubuntu
```

The `--device-write-iops` flag limits write rate (IO per second) to a device. For example, this command creates a container and limits the write rate to `1000` IO per second to `/dev/sda`:

​	`--device-write-iops` 标志限制写入设备的速率（每秒 IO 次数）。例如，以下命令创建一个容器，并将 `/dev/sda` 的写入速率限制为每秒 `1000` IO：



```console
$ docker run -it --device-write-iops /dev/sda:1000 ubuntu
```

Both flags take limits in the `<device-path>:<limit>` format. Both read and write rates must be a positive integer.

​	两个标志的限制格式为 `<device-path>:<limit>`，且读写速率必须为正整数。

## 附加组 Additional groups



```console
--group-add: Add additional groups to run as
```

By default, the docker container process runs with the supplementary groups looked up for the specified user. If one wants to add more to that list of groups, then one can use this flag:

​	默认情况下，Docker 容器进程运行时，查找指定用户的辅助组。如果想要添加更多组，可以使用该标志：



```console
$ docker run --rm --group-add audio --group-add nogroup --group-add 777 busybox id

uid=0(root) gid=0(root) groups=10(wheel),29(audio),99(nogroup),777
```

## 运行时权限和 Linux 功能 Runtime privilege and Linux capabilities

| Option         | Description 描述                                             |
| :------------- | :----------------------------------------------------------- |
| `--cap-add`    | 添加 Linux 功能 Add Linux capabilities                       |
| `--cap-drop`   | 删除 Linux 功能 Drop Linux capabilities                      |
| `--privileged` | 为该容器提供扩展的权限 Give extended privileges to this container |
| `--device=[]`  | 允许在容器中运行设备，而不使用 `--privileged` 标志 Allows you to run devices inside the container without the `--privileged` flag. |

By default, Docker containers are "unprivileged" and cannot, for example, run a Docker daemon inside a Docker container. This is because by default a container is not allowed to access any devices, but a "privileged" container is given access to all devices (see the documentation on [cgroups devices](https://www.kernel.org/doc/Documentation/cgroup-v1/devices.txt)).

​	默认情况下，Docker 容器为“非特权模式”，因此不能在 Docker 容器内运行 Docker 守护进程。这是因为默认情况下容器不允许访问任何设备，而“特权”容器可以访问所有设备（参见 [cgroups 设备文档](https://www.kernel.org/doc/Documentation/cgroup-v1/devices.txt)）。

The `--privileged` flag gives all capabilities to the container. When the operator executes `docker run --privileged`, Docker enables access to all devices on the host, and reconfigures AppArmor or SELinux to allow the container nearly all the same access to the host as processes running outside containers on the host. Use this flag with caution. For more information about the `--privileged` flag, see the [`docker run` reference](https://docs.docker.com/reference/cli/docker/container/run/#privileged).

​	`--privileged` 标志为容器提供所有功能。当操作员执行 `docker run --privileged` 时，Docker 启用对主机上所有设备的访问，并重新配置 AppArmor 或 SELinux，以使容器几乎具有与在主机上运行的非容器进程相同的访问权限。使用此标志时需谨慎。有关 `--privileged` 标志的更多信息，请参见 [`docker run` 参考](https://docs.docker.com/reference/cli/docker/container/run/#privileged)。

If you want to limit access to a specific device or devices you can use the `--device` flag. It allows you to specify one or more devices that will be accessible within the container.

​	如果您想限制访问特定设备，可以使用 `--device` 标志。它允许您指定一个或多个设备，这些设备将在容器中可访问。



```console
$ docker run --device=/dev/snd:/dev/snd ...
```

By default, the container will be able to `read`, `write`, and `mknod` these devices. This can be overridden using a third `:rwm` set of options to each `--device` flag:

​	默认情况下，容器可以对这些设备执行 `read`、`write` 和 `mknod` 操作。可以为每个 `--device` 标志使用第三个 `:rwm` 选项来覆盖默认值：



```console
$ docker run --device=/dev/sda:/dev/xvdc --rm -it ubuntu fdisk  /dev/xvdc

Command (m for help): q
$ docker run --device=/dev/sda:/dev/xvdc:r --rm -it ubuntu fdisk  /dev/xvdc
You will not be able to write the partition table.

Command (m for help): q

$ docker run --device=/dev/sda:/dev/xvdc:w --rm -it ubuntu fdisk  /dev/xvdc
    crash....

$ docker run --device=/dev/sda:/dev/xvdc:m --rm -it ubuntu fdisk  /dev/xvdc
fdisk: unable to open /dev/xvdc: Operation not permitted
```

In addition to `--privileged`, the operator can have fine grain control over the capabilities using `--cap-add` and `--cap-drop`. By default, Docker has a default list of capabilities that are kept. The following table lists the Linux capability options which are allowed by default and can be dropped.

​	除了 `--privileged` 外，操作员还可以使用 `--cap-add` 和 `--cap-drop` 对功能进行细粒度控制。默认情况下，Docker 会保留一份默认的功能列表，可以将其移除。以下表格列出了默认允许但可以删除的 Linux 功能选项。

| Capability Key   | Capability Description 功能描述                              |
| :--------------- | :----------------------------------------------------------- |
| AUDIT_WRITE      | 写入记录到内核审计日志。 Write records to kernel auditing log. |
| CHOWN            | 对文件 UID 和 GID 进行任意更改（见 chown(2)）。 Make arbitrary changes to file UIDs and GIDs (see chown(2)). |
| DAC_OVERRIDE     | 绕过文件读、写和执行权限检查。 Bypass file read, write, and execute permission checks. |
| FOWNER           | 绕过操作权限检查，该操作通常要求文件系统 UID 匹配文件的 UID。 Bypass permission checks on operations that normally require the file system UID of the process to match the UID of the file. |
| FSETID           | 修改文件时不清除 set-user-ID 和 set-group-ID 权限位。 Don't clear set-user-ID and set-group-ID permission bits when a file is modified. |
| KILL             | 绕过发送信号的权限检查。 Bypass permission checks for sending signals. |
| MKNOD            | 使用 mknod(2) 创建特殊文件。 Create special files using mknod(2). |
| NET_BIND_SERVICE | 将套接字绑定到特权端口（端口号小于 1024）。 Bind a socket to internet domain privileged ports (port numbers less than 1024). |
| NET_RAW          | 使用 RAW 和 PACKET 套接字。 Use RAW and PACKET sockets.      |
| SETFCAP          | 设置文件功能。 Set file capabilities.                        |
| SETGID           | 对进程 GID 和补充 GID 列表进行任意更改。 Make arbitrary manipulations of process GIDs and supplementary GID list. |
| SETPCAP          | 修改进程功能。 Modify process capabilities.                  |
| SETUID           | 对进程 UID 进行任意更改。 Make arbitrary manipulations of process UIDs. |
| SYS_CHROOT       | 使用 chroot(2) 更改根目录。Use chroot(2), change root directory. |

The next table shows the capabilities which are not granted by default and may be added.

​	下表显示了默认不授予但可以添加的能力。

| Capability Key     | Capability Description 功能描述                              |
| :----------------- | :----------------------------------------------------------- |
| AUDIT_CONTROL      | 启用和禁用内核审计，更改审计过滤规则，检索审计状态和过滤规则。 Enable and disable kernel auditing; change auditing filter rules; retrieve auditing status and filtering rules. |
| AUDIT_READ         | 允许通过多播 netlink 套接字读取审计日志。 Allow reading the audit log via multicast netlink socket. |
| BLOCK_SUSPEND      | 允许阻止系统挂起。 Allow preventing system suspends.         |
| BPF                | 允许创建 BPF 映射，加载 BPF 类型格式 (BTF) 数据，检索 BPF 程序的 JITed 代码等。 Allow creating BPF maps, loading BPF Type Format (BTF) data, retrieve JITed code of BPF programs, and more. |
| CHECKPOINT_RESTORE | 允许检查点/恢复相关操作。 在内核 5.9 中引入。 Allow checkpoint/restore related operations. Introduced in kernel 5.9. |
| DAC_READ_SEARCH    | 绕过文件读权限检查和目录读、执行权限检查。 Bypass file read permission checks and directory read and execute permission checks. |
| IPC_LOCK           | 锁定内存（mlock(2)、mlockall(2)、mmap(2)、shmctl(2)）。Lock memory (mlock(2), mlockall(2), mmap(2), shmctl(2)). |
| IPC_OWNER          | 绕过 System V IPC 对象上的操作权限检查。Bypass permission checks for operations on System V IPC objects. |
| LEASE              | 在任意文件上建立租约（参见 fcntl(2)）。Establish leases on arbitrary files (see fcntl(2)). |
| LINUX_IMMUTABLE    | 设置 FS_APPEND_FL 和 FS_IMMUTABLE_FL i-node 标志。Set the FS_APPEND_FL and FS_IMMUTABLE_FL i-node flags. |
| MAC_ADMIN          | 允许 MAC 配置或状态更改。已实现 Smack LSM。Allow MAC configuration or state changes. Implemented for the Smack LSM. |
| MAC_OVERRIDE       | 覆盖强制访问控制（MAC）。已实现 Smack Linux 安全模块 (LSM)。Override Mandatory Access Control (MAC). Implemented for the Smack Linux Security Module (LSM). |
| NET_ADMIN          | 执行各种网络相关的操作。Perform various network-related operations. |
| NET_BROADCAST      | 执行套接字广播并侦听多播。Make socket broadcasts, and listen to multicasts. |
| PERFMON            | 允许使用 perf_events、i915_perf 和其他内核子系统进行系统性能和可观测性特权操作。Allow system performance and observability privileged operations using perf_events, i915_perf and other kernel subsystems |
| SYS_ADMIN          | 执行一系列系统管理操作。Perform a range of system administration operations. |
| SYS_BOOT           | 使用 reboot(2) 和 kexec_load(2)，重新启动并加载新的内核以供以后执行。Use reboot(2) and kexec_load(2), reboot and load a new kernel for later execution. |
| SYS_MODULE         | 加载和卸载内核模块。Load and unload kernel modules.          |
| SYS_NICE           | 提高进程的 nice 值（nice(2)、setpriority(2)），并更改任意进程的 nice 值。Raise process nice value (nice(2), setpriority(2)) and change the nice value for arbitrary processes. |
| SYS_PACCT          | 使用 acct(2)，切换进程记账开关。Use acct(2), switch process accounting on or off. |
| SYS_PTRACE         | 使用 ptrace(2) 跟踪任意进程。Trace arbitrary processes using ptrace(2). |
| SYS_RAWIO          | 执行 I/O 端口操作（iopl(2) 和 ioperm(2)）。Perform I/O port operations (iopl(2) and ioperm(2)). |
| SYS_RESOURCE       | 覆盖资源限制。Override resource Limits.                      |
| SYS_TIME           | 设置系统时钟（settimeofday(2)、stime(2)、adjtimex(2)）；设置实时（硬件）时钟。Set system clock (settimeofday(2), stime(2), adjtimex(2)); set real-time (hardware) clock. |
| SYS_TTY_CONFIG     | 使用 vhangup(2)；在虚拟终端上执行各种特权 ioctl(2) 操作。Use vhangup(2); employ various privileged ioctl(2) operations on virtual terminals. |
| SYSLOG             | 执行特权 syslog(2) 操作。Perform privileged syslog(2) operations. |
| WAKE_ALARM         | 触发可以唤醒系统的事件。Trigger something that will wake up the system. |

Further reference information is available on the [capabilities(7) - Linux man page](https://man7.org/linux/man-pages/man7/capabilities.7.html), and in the [Linux kernel source code](https://github.com/torvalds/linux/blob/124ea650d3072b005457faed69909221c2905a1f/include/uapi/linux/capability.h).

​	可以在 [capabilities(7) - Linux man page](https://man7.org/linux/man-pages/man7/capabilities.7.html) 和 [Linux kernel source code](https://github.com/torvalds/linux/blob/124ea650d3072b005457faed69909221c2905a1f/include/uapi/linux/capability.h) 中找到更多参考信息。

Both flags support the value `ALL`, so to allow a container to use all capabilities except for `MKNOD`:

​	两个标志都支持 `ALL` 值，因此可以允许容器使用所有能力，除 `MKNOD` 以外：



```console
$ docker run --cap-add=ALL --cap-drop=MKNOD ...
```

The `--cap-add` and `--cap-drop` flags accept capabilities to be specified with a `CAP_` prefix. The following examples are therefore equivalent:

​	`--cap-add` 和 `--cap-drop` 标志接受以 `CAP_` 前缀指定的能力。以下示例等效：



```console
$ docker run --cap-add=SYS_ADMIN ...
$ docker run --cap-add=CAP_SYS_ADMIN ...
```

For interacting with the network stack, instead of using `--privileged` they should use `--cap-add=NET_ADMIN` to modify the network interfaces.

​	要与网络栈交互，而不是使用 `--privileged`，应使用 `--cap-add=NET_ADMIN` 来修改网络接口。

```console
$ docker run -it --rm  ubuntu:24.04 ip link add dummy0 type dummy

RTNETLINK answers: Operation not permitted

$ docker run -it --rm --cap-add=NET_ADMIN ubuntu:24.04 ip link add dummy0 type dummy
```

To mount a FUSE based filesystem, you need to combine both `--cap-add` and `--device`:



​	要挂载基于 FUSE 的文件系统，您需要同时使用 `--cap-add` 和 `--device`：

```console
$ docker run --rm -it --cap-add SYS_ADMIN sshfs sshfs sven@10.10.10.20:/home/sven /mnt

fuse: failed to open /dev/fuse: Operation not permitted

$ docker run --rm -it --device /dev/fuse sshfs sshfs sven@10.10.10.20:/home/sven /mnt

fusermount: mount failed: Operation not permitted

$ docker run --rm -it --cap-add SYS_ADMIN --device /dev/fuse sshfs

# sshfs sven@10.10.10.20:/home/sven /mnt
The authenticity of host '10.10.10.20 (10.10.10.20)' can't be established.
ECDSA key fingerprint is 25:34:85:75:25:b0:17:46:05:19:04:93:b5:dd:5f:c6.
Are you sure you want to continue connecting (yes/no)? yes
sven@10.10.10.20's password:

root@30aa0cfaf1b5:/# ls -la /mnt/src/docker

total 1516
drwxrwxr-x 1 1000 1000   4096 Dec  4 06:08 .
drwxrwxr-x 1 1000 1000   4096 Dec  4 11:46 ..
-rw-rw-r-- 1 1000 1000     16 Oct  8 00:09 .dockerignore
-rwxrwxr-x 1 1000 1000    464 Oct  8 00:09 .drone.yml
drwxrwxr-x 1 1000 1000   4096 Dec  4 06:11 .git
-rw-rw-r-- 1 1000 1000    461 Dec  4 06:08 .gitignore
....
```

The default seccomp profile will adjust to the selected capabilities, in order to allow use of facilities allowed by the capabilities, so you should not have to adjust this.

​	默认的 seccomp 配置文件会根据所选能力进行调整，以便允许使用能力允许的设施，因此通常无需调整。

## 覆盖镜像默认值 Overriding image defaults

When you build an image from a [Dockerfile]({{< ref "/reference/Dockerfilereference" >}}), or when committing it, you can set a number of default parameters that take effect when the image starts up as a container. When you run an image, you can override those defaults using flags for the `docker run` command.

​	从 [Dockerfile]({{< ref "/reference/Dockerfilereference" >}}) 构建镜像或提交镜像时，可以设置一些默认参数，以便镜像启动为容器时生效。运行镜像时，可以使用 `docker run` 命令的标志覆盖这些默认值。

- [Default entrypoint](https://docs.docker.com/engine/containers/run/#default-entrypoint)

- [Default command and options](https://docs.docker.com/engine/containers/run/#default-command-and-options)
- [Expose ports](https://docs.docker.com/engine/containers/run/#exposed-ports)
- [Environment variables](https://docs.docker.com/engine/containers/run/#environment-variables)
- [Healthcheck](https://docs.docker.com/engine/containers/run/#healthchecks)
- [User](https://docs.docker.com/engine/containers/run/#user)
- [Working directory](https://docs.docker.com/engine/containers/run/#working-directory)

### Default command and options

The command syntax for `docker run` supports optionally specifying commands and arguments to the container's entrypoint, represented as `[COMMAND]` and `[ARG...]` in the following synopsis example:

​	`docker run` 命令语法支持可选指定命令和参数，以传递给容器的入口点，这在以下示例中表示为 `[COMMAND]` 和 `[ARG...]`：

```console
$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

This command is optional because whoever created the `IMAGE` may have already provided a default `COMMAND`, using the Dockerfile `CMD` instruction. When you run a container, you can override that `CMD` instruction just by specifying a new `COMMAND`.

​	此命令是可选的，因为创建 `IMAGE` 的用户可能已通过 Dockerfile `CMD` 指令提供了默认的 `COMMAND`。当您运行容器时，只需指定一个新 `COMMAND` 即可覆盖该 `CMD` 指令。

If the image also specifies an `ENTRYPOINT` then the `CMD` or `COMMAND` get appended as arguments to the `ENTRYPOINT`.

​	如果镜像还指定了 `ENTRYPOINT`，则 `CMD` 或 `COMMAND` 将作为参数附加到 `ENTRYPOINT`。

### Default entrypoint



```text
--entrypoint="": Overwrite the default entrypoint set by the image
```

The entrypoint refers to the default executable that's invoked when you run a container. A container's entrypoint is defined using the Dockerfile `ENTRYPOINT` instruction. It's similar to specifying a default command because it specifies, but the difference is that you need to pass an explicit flag to override the entrypoint, whereas you can override default commands with positional arguments. The defines a container's default behavior, with the idea that when you set an entrypoint you can run the container *as if it were that binary*, complete with default options, and you can pass in more options as commands. But there are cases where you may want to run something else inside the container. This is when overriding the default entrypoint at runtime comes in handy, using the `--entrypoint` flag for the `docker run` command.

​	入口点指的是运行容器时调用的默认可执行文件。容器的入口点是使用 Dockerfile `ENTRYPOINT` 指令定义的。类似于指定默认命令，但区别在于，覆盖入口点需要明确的标志，而可以通过位置参数覆盖默认命令。入口点定义了容器的默认行为，使其可以作为该二进制文件运行，带有默认选项，并可以传入更多选项作为命令。然而，有时您可能想要在容器中运行其他内容。在这种情况下，可以使用 `docker run` 命令中的 `--entrypoint` 标志在运行时覆盖默认入口点。

The `--entrypoint` flag expects a string value, representing the name or path of the binary that you want to invoke when the container starts. The following example shows you how to run a Bash shell in a container that has been set up to automatically run some other binary (like `/usr/bin/redis-server`):

​	`--entrypoint` 标志需要一个字符串值，表示您希望在容器启动时调用的二进制文件的名称或路径。以下示例展示了如何在已设置为自动运行其他二进制文件（如 `/usr/bin/redis-server`）的容器中运行 Bash shell：



```console
$ docker run -it --entrypoint /bin/bash example/redis
```

The following examples show how to pass additional parameters to the custom entrypoint, using the positional command arguments:

​	以下示例展示了如何传递额外参数给自定义入口点，使用位置命令参数：



```console
$ docker run -it --entrypoint /bin/bash example/redis -c ls -l
$ docker run -it --entrypoint /usr/bin/redis-cli example/redis --help
```

You can reset a containers entrypoint by passing an empty string, for example:

​	可以通过传递一个空字符串来重置容器的入口点，例如：

```console
$ docker run -it --entrypoint="" mysql bash
```

> **Note**
>
> Passing `--entrypoint` clears out any default command set on the image. That is, any `CMD` instruction in the Dockerfile used to build it.
>
> ​	传递 `--entrypoint` 将清除镜像中设置的任何默认命令。也就是说，Dockerfile 中用于构建的任何 `CMD` 指令都将被清除。

### Exposed ports

By default, when you run a container, none of the container's ports are exposed to the host. This means you won't be able to access any ports that the container might be listening on. To make a container's ports accessible from the host, you need to publish the ports.

​	默认情况下，当您运行容器时，容器的端口不会向主机暴露。这意味着您无法访问容器可能正在监听的任何端口。为了使容器的端口可以从主机访问，您需要发布这些端口。

You can start the container with the `-P` or `-p` flags to expose its ports:

​	可以使用 `-P` 或 `-p` 标志启动容器以暴露端口：

- The `-P` (or `--publish-all`) flag publishes all the exposed ports to the host. Docker binds each exposed port to a random port on the host.
  - `-P`（或 `--publish-all`）标志会将所有暴露的端口发布到主机上。Docker 会将每个暴露的端口绑定到主机上的随机端口。

- The `-P` flag only publishes port numbers that are explicitly flagged as exposed, either using the Dockerfile `EXPOSE` instruction or the `--expose` flag for the `docker run` command.
  - `-P` 标志仅发布显式标记为暴露的端口号，这可以通过 Dockerfile 中的 `EXPOSE` 指令或 `docker run` 命令的 `--expose` 标志来完成。
  
- The `-p` (or `--publish`) flag lets you explicitly map a single port or range of ports in the container to the host.
  - `-p`（或 `--publish`）标志允许您将容器中的单个端口或端口范围显式映射到主机上。


The port number inside the container (where the service listens) doesn't need to match the port number published on the outside of the container (where clients connect). For example, inside the container an HTTP service might be listening on port 80. At runtime, the port might be bound to 42800 on the host. To find the mapping between the host ports and the exposed ports, use the `docker port` command.

​	容器内的端口号（服务监听的端口）不必与发布到容器外部的端口号（客户端连接的端口）相匹配。例如，容器内部的 HTTP 服务可能监听端口 80。而在运行时，该端口可能会绑定到主机上的 42800 端口。可以使用 `docker port` 命令查找主机端口与暴露端口之间的映射关系。

### Environment variables

Docker automatically sets some environment variables when creating a Linux container. Docker doesn't set any environment variables when creating a Windows container.

​	Docker 在创建 Linux 容器时会自动设置一些环境变量。而创建 Windows 容器时不会设置任何环境变量。

The following environment variables are set for Linux containers:

​	以下环境变量会为 Linux 容器设置：

| Variable   | Value                                                        |
| :--------- | :----------------------------------------------------------- |
| `HOME`     | 根据 `USER` 的值设置 Set based on the value of `USER`        |
| `HOSTNAME` | 与容器关联的主机名 The hostname associated with the container |
| `PATH`     | 包含一些常用目录，如 `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin` - Includes popular directories, such as `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin` |
| `TERM`     | 如果容器分配了伪终端，值为 `xterm` - `xterm` if the container is allocated a pseudo-TTY |

Additionally, you can set any environment variable in the container by using one or more `-e` flags. You can even override the variables mentioned above, or variables defined using a Dockerfile `ENV` instruction when building the image.

​	此外，可以使用一个或多个 `-e` 标志在容器中设置任意环境变量。可以覆盖上述变量，或者覆盖构建镜像时使用 Dockerfile `ENV` 指令定义的变量。

If the you name an environment variable without specifying a value, the current value of the named variable on the host is propagated into the container's environment:

​	如果命名了一个环境变量但未指定值，则该变量在主机上的当前值将被传递到容器的环境中：



```console
$ export today=Wednesday
$ docker run -e "deep=purple" -e today --rm alpine env

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=d2219b854598
deep=purple
today=Wednesday
HOME=/root
```



```powershell
PS C:\> docker run --rm -e "foo=bar" microsoft/nanoserver cmd /s /c set
ALLUSERSPROFILE=C:\ProgramData
APPDATA=C:\Users\ContainerAdministrator\AppData\Roaming
CommonProgramFiles=C:\Program Files\Common Files
CommonProgramFiles(x86)=C:\Program Files (x86)\Common Files
CommonProgramW6432=C:\Program Files\Common Files
COMPUTERNAME=C2FAEFCC8253
ComSpec=C:\Windows\system32\cmd.exe
foo=bar
LOCALAPPDATA=C:\Users\ContainerAdministrator\AppData\Local
NUMBER_OF_PROCESSORS=8
OS=Windows_NT
Path=C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Users\ContainerAdministrator\AppData\Local\Microsoft\WindowsApps
PATHEXT=.COM;.EXE;.BAT;.CMD
PROCESSOR_ARCHITECTURE=AMD64
PROCESSOR_IDENTIFIER=Intel64 Family 6 Model 62 Stepping 4, GenuineIntel
PROCESSOR_LEVEL=6
PROCESSOR_REVISION=3e04
ProgramData=C:\ProgramData
ProgramFiles=C:\Program Files
ProgramFiles(x86)=C:\Program Files (x86)
ProgramW6432=C:\Program Files
PROMPT=$P$G
PUBLIC=C:\Users\Public
SystemDrive=C:
SystemRoot=C:\Windows
TEMP=C:\Users\ContainerAdministrator\AppData\Local\Temp
TMP=C:\Users\ContainerAdministrator\AppData\Local\Temp
USERDOMAIN=User Manager
USERNAME=ContainerAdministrator
USERPROFILE=C:\Users\ContainerAdministrator
windir=C:\Windows
```

### Healthchecks

The following flags for the `docker run` command let you control the parameters for container healthchecks:

​	`docker run` 命令的以下标志让您可以控制容器健康检查的参数：

| Option                    | Description                                                  |
| :------------------------ | :----------------------------------------------------------- |
| `--health-cmd`            | 用于检查健康状况的命令 Command to run to check health        |
| `--health-interval`       | 检查间隔时间 Time between running the check                  |
| `--health-retries`        | 需要报告不健康状态的连续失败次数 Consecutive failures needed to report unhealthy |
| `--health-timeout`        | 每次检查允许的最长时间 Maximum time to allow one check to run |
| `--health-start-period`   | 在开始健康检查重试倒计时之前，用于容器初始化的时间 Start period for the container to initialize before starting health-retries countdown |
| `--health-start-interval` | 在启动阶段进行检查的时间间隔 Time between running the check during the start period |
| `--no-healthcheck`        | 禁用容器指定的 `HEALTHCHECK` - Disable any container-specified `HEALTHCHECK` |

Example:



```console
$ docker run --name=test -d \
    --health-cmd='stat /etc/passwd || exit 1' \
    --health-interval=2s \
    busybox sleep 1d
$ sleep 2; docker inspect --format='{{.State.Health.Status}}' test
healthy
$ docker exec test rm /etc/passwd
$ sleep 2; docker inspect --format='{{json .State.Health}}' test
{
  "Status": "unhealthy",
  "FailingStreak": 3,
  "Log": [
    {
      "Start": "2016-05-25T17:22:04.635478668Z",
      "End": "2016-05-25T17:22:04.7272552Z",
      "ExitCode": 0,
      "Output": "  File: /etc/passwd\n  Size: 334       \tBlocks: 8          IO Block: 4096   regular file\nDevice: 32h/50d\tInode: 12          Links: 1\nAccess: (0664/-rw-rw-r--)  Uid: (    0/    root)   Gid: (    0/    root)\nAccess: 2015-12-05 22:05:32.000000000\nModify: 2015..."
    },
    {
      "Start": "2016-05-25T17:22:06.732900633Z",
      "End": "2016-05-25T17:22:06.822168935Z",
      "ExitCode": 0,
      "Output": "  File: /etc/passwd\n  Size: 334       \tBlocks: 8          IO Block: 4096   regular file\nDevice: 32h/50d\tInode: 12          Links: 1\nAccess: (0664/-rw-rw-r--)  Uid: (    0/    root)   Gid: (    0/    root)\nAccess: 2015-12-05 22:05:32.000000000\nModify: 2015..."
    },
    {
      "Start": "2016-05-25T17:22:08.823956535Z",
      "End": "2016-05-25T17:22:08.897359124Z",
      "ExitCode": 1,
      "Output": "stat: can't stat '/etc/passwd': No such file or directory\n"
    },
    {
      "Start": "2016-05-25T17:22:10.898802931Z",
      "End": "2016-05-25T17:22:10.969631866Z",
      "ExitCode": 1,
      "Output": "stat: can't stat '/etc/passwd': No such file or directory\n"
    },
    {
      "Start": "2016-05-25T17:22:12.971033523Z",
      "End": "2016-05-25T17:22:13.082015516Z",
      "ExitCode": 1,
      "Output": "stat: can't stat '/etc/passwd': No such file or directory\n"
    }
  ]
}
```

The health status is also displayed in the `docker ps` output.

​	健康状态也会在 `docker ps` 输出中显示。

### User

The default user within a container is `root` (uid = 0). You can set a default user to run the first process with the Dockerfile `USER` instruction. When starting a container, you can override the `USER` instruction by passing the `-u` option.

​	容器内的默认用户是 `root`（uid = 0）。可以使用 Dockerfile `USER` 指令设置运行第一个进程的默认用户。启动容器时，可以通过传递 `-u` 选项覆盖 `USER` 指令。



```text
-u="", --user="": Sets the username or UID used and optionally the groupname or GID for the specified command.设置用于指定命令的用户名或 UID，并可选地指定组名或 GID。
```

The followings examples are all valid:

​	以下示例都是有效的：



```text
--user=[ user | user:group | uid | uid:gid | user:gid | uid:group ]
```

> **Note**
>
> If you pass a numeric user ID, it must be in the range of 0-2147483647. If you pass a username, the user must exist in the container.
>
> ​	如果传递了数值用户 ID，则必须在 0-2147483647 范围内。如果传递了用户名，则该用户必须存在于容器中。

### Working directory

The default working directory for running binaries within a container is the root directory (`/`). The default working directory of an image is set using the Dockerfile `WORKDIR` command. You can override the default working directory for an image using the `-w` (or `--workdir`) flag for the `docker run` command:

​	在容器中运行二进制文件的默认工作目录是根目录 (`/`)。可以使用 Dockerfile `WORKDIR` 命令设置镜像的默认工作目录。可以使用 `docker run` 命令的 `-w`（或 `--workdir`）标志覆盖镜像的默认工作目录：



```text
$ docker run --rm -w /my/workdir alpine pwd
/my/workdir
```

If the directory doesn't already exist in the container, it's created.

​	如果该目录在容器中尚不存在，则会自动创建。
