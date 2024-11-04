+++
title = "docker container update"
date = 2024-10-23T14:54:43+08:00
weight = 200
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/update/](https://docs.docker.com/reference/cli/docker/container/update/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container update

| Description | Update configuration of one or more containers               |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container update [OPTIONS] CONTAINER [CONTAINER...]` |
| Aliases     | `docker update`                                              |

## Description

The `docker update` command dynamically updates container configuration. You can use this command to prevent containers from consuming too many resources from their Docker host. With a single command, you can place limits on a single container or on many. To specify more than one container, provide space-separated list of container names or IDs.

​	`docker update` 命令动态更新容器配置。您可以使用此命令防止容器从其 Docker 主机消耗过多资源。通过一个命令，您可以对单个容器或多个容器设置限制。要指定多个容器，请提供以空格分隔的容器名称或 ID 列表。

With the exception of the `--kernel-memory` option, you can specify these options on a running or a stopped container. On kernel version older than 4.6, you can only update `--kernel-memory` on a stopped container or on a running container with kernel memory initialized.

​	除了 `--kernel-memory` 选项外，您可以在运行中的容器或已停止的容器上指定这些选项。在内核版本低于 4.6 的情况下，您只能在已停止的容器或具有初始化内核内存的运行中容器上更新 `--kernel-memory`。

> 

> **Warning**
>
> The `docker update` and `docker container update` commands are not supported for Windows containers.
>
> ​	`docker update` 和 `docker container update` 命令不支持 Windows 容器。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `--blkio-weight`                                             |         | 块 I/O（相对权重），介于 10 和 1000 之间，0 表示禁用（默认 0）Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0) |
| `--cpu-period`                                               |         | 限制 CPU CFS（完全公平调度程序）周期 Limit CPU CFS (Completely Fair Scheduler) period |
| `--cpu-quota`                                                |         | 限制 CPU CFS（完全公平调度程序）配额 Limit CPU CFS (Completely Fair Scheduler) quota |
| `--cpu-rt-period`                                            |         | API 1.25+ 限制 CPU 实时周期（以微秒为单位） API 1.25+ Limit the CPU real-time period in microseconds |
| `--cpu-rt-runtime`                                           |         | API 1.25+ 限制 CPU 实时运行时间（以微秒为单位） API 1.25+ Limit the CPU real-time runtime in microseconds |
| [`-c, --cpu-shares`](https://docs.docker.com/reference/cli/docker/container/update/#cpu-shares) |         | CPU 份额（相对权重） CPU shares (relative weight)            |
| `--cpus`                                                     |         | API 1.29+ CPU 数量 API 1.29+ Number of CPUs                  |
| `--cpuset-cpus`                                              |         | 允许执行的 CPU（0-3, 0,1） CPUs in which to allow execution (0-3, 0,1) |
| `--cpuset-mems`                                              |         | 允许执行的内存（0-3, 0,1） MEMs in which to allow execution (0-3, 0,1) |
| [`-m, --memory`](https://docs.docker.com/reference/cli/docker/container/update/#memory) |         | 内存限制 Memory limit                                        |
| `--memory-reservation`                                       |         | 内存软限制 Memory soft limit                                 |
| `--memory-swap`                                              |         | 交换限制等于内存加交换：-1 表示启用无限交换 Swap limit equal to memory plus swap: -1 to enable unlimited swap |
| `--pids-limit`                                               |         | API 1.40+ 调整容器 PIDs 限制（设置 -1 表示无限） API 1.40+ Tune container pids limit (set -1 for unlimited) |
| [`--restart`](https://docs.docker.com/reference/cli/docker/container/update/#restart) |         | 容器退出时应用的重启策略 Restart policy to apply when a container exits |

## Examples

The following sections illustrate ways to use this command.

​	以下部分展示了使用此命令的方法。

### 更新容器的 cpu-shares (`--cpu-shares`) Update a container's cpu-shares (`--cpu-shares`)

To limit a container's cpu-shares to 512, first identify the container name or ID. You can use `docker ps` to find these values. You can also use the ID returned from the `docker run` command. Then, do the following:

​	要将容器的 cpu-shares 限制为 512，首先识别容器名称或 ID。您可以使用 `docker ps` 来查找这些值。您也可以使用 `docker run` 命令返回的 ID。然后，执行以下操作：



```console
$ docker update --cpu-shares 512 abebf7571666
```

### 更新容器的 cpu-shares 和内存 (`-m`, `--memory`) Update a container with cpu-shares and memory (`-m`, `--memory`)

To update multiple resource configurations for multiple containers:

​	要同时更新多个容器的多个资源配置：



```console
$ docker update --cpu-shares 512 -m 300M abebf7571666 hopeful_morse
```

### 更新容器的内核内存限制 (`--kernel-memory`) Update a container's kernel memory constraints (`--kernel-memory`)

You can update a container's kernel memory limit using the `--kernel-memory` option. On kernel version older than 4.6, this option can be updated on a running container only if the container was started with `--kernel-memory`. If the container was started without `--kernel-memory` you need to stop the container before updating kernel memory.

​	您可以使用 `--kernel-memory` 选项更新容器的内核内存限制。在内核版本低于 4.6 的情况下，只有当容器使用 `--kernel-memory` 启动时，才能在运行中的容器上更新此选项。如果容器在未使用 `--kernel-memory` 启动的情况下启动，则需要在更新内核内存之前停止容器。



> **Note**
>
> The `--kernel-memory` option has been deprecated since Docker 20.10.
>
> ​	自 Docker 20.10 起，`--kernel-memory` 选项已被弃用。

For example, if you started a container with this command:

​	例如，如果您使用以下命令启动了一个容器：

```console
$ docker run -dit --name test --kernel-memory 50M ubuntu bash
```

You can update kernel memory while the container is running:

​	您可以在容器运行时更新内核内存：

```console
$ docker update --kernel-memory 80M test
```

If you started a container without kernel memory initialized:

​	如果您启动了一个未初始化内核内存的容器：

```console
$ docker run -dit --name test2 --memory 300M ubuntu bash
```

Update kernel memory of running container `test2` will fail. You need to stop the container before updating the `--kernel-memory` setting. The next time you start it, the container uses the new value.

​	更新正在运行的容器 `test2` 的内核内存将失败。您需要在更新 `--kernel-memory` 设置之前停止容器。下次启动时，容器将使用新值。

Kernel version newer than (include) 4.6 does not have this limitation, you can use `--kernel-memory` the same way as other options.

​	内核版本高于（包括）4.6 不存在此限制，您可以像其他选项一样使用 `--kernel-memory`。

### 更新容器的重启策略 (`--restart`) Update a container's restart policy (`--restart`)

You can change a container's restart policy on a running container. The new restart policy takes effect instantly after you run `docker update` on a container.

​	您可以在运行中的容器上更改容器的重启策略。新的重启策略在您对容器运行 `docker update` 后立即生效。

To update restart policy for one or more containers:

​	要更新一个或多个容器的重启策略：

```console
$ docker update --restart=on-failure:3 abebf7571666 hopeful_morse
```

Note that if the container is started with `--rm` flag, you cannot update the restart policy for it. The `AutoRemove` and `RestartPolicy` are mutually exclusive for the container.

​	请注意，如果容器是使用 `--rm` 标志启动的，您将无法更新其重启策略。`AutoRemove` 和 `RestartPolicy` 对容器是互斥的。
