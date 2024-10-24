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

See the [freezer cgroup documentation](https://www.kernel.org/doc/Documentation/cgroup-v1/freezer-subsystem.txt) for further details.

## Examples



```console
$ docker pause my_container
```
