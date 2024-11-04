+++
title = "docker container pause"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/pause/](https://docs.docker.com/reference/cli/docker/container/pause/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container pause

| Description | Pause all processes within one or more containers |
| :---------- | ------------------------------------------------- |
| Usage       | `docker container pause CONTAINER [CONTAINER...]` |
| Aliases     | `docker pause`                                    |

## Description

The `docker pause` command suspends all processes in the specified containers. On Linux, this uses the freezer cgroup. Traditionally, when suspending a process the `SIGSTOP` signal is used, which is observable by the process being suspended. With the freezer cgroup the process is unaware, and unable to capture, that it is being suspended, and subsequently resumed. On Windows, only Hyper-V containers can be paused.

​	`docker pause` 命令暂停指定容器中的所有进程。在 Linux 上，这使用冷冻 cgroup。传统上，当暂停一个进程时，使用 `SIGSTOP` 信号，进程可以感知到自己被暂停。使用冷冻 cgroup 时，进程对此并不知情，无法捕获被暂停和随后恢复的情况。在 Windows 上，只有 Hyper-V 容器可以被暂停。

See the [freezer cgroup documentation](https://www.kernel.org/doc/Documentation/cgroup-v1/freezer-subsystem.txt) for further details.

​	有关更多详细信息，请参见 [冷冻 cgroup 文档](https://www.kernel.org/doc/Documentation/cgroup-v1/freezer-subsystem.txt)。

## Examples



```console
$ docker pause my_container
```
