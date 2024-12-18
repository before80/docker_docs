+++
title = "docker container unpause"
date = 2024-10-23T14:54:43+08:00
weight = 190
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/unpause/](https://docs.docker.com/reference/cli/docker/container/unpause/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container unpause

| Description | Unpause all processes within one or more containers |
| :---------- | --------------------------------------------------- |
| Usage       | `docker container unpause CONTAINER [CONTAINER...]` |
| Aliases     | `docker unpause`                                    |

## Description

The `docker unpause` command un-suspends all processes in the specified containers. On Linux, it does this using the freezer cgroup.

​	`docker unpause` 命令会取消暂停指定容器中的所有进程。在 Linux 上，它通过使用冻结的 cgroup 来实现。

See the [freezer cgroup documentation](https://www.kernel.org/doc/Documentation/cgroup-v1/freezer-subsystem.txt) for further details.

​	有关更多详细信息，请参见 [freezer cgroup 文档](https://www.kernel.org/doc/Documentation/cgroup-v1/freezer-subsystem.txt)。

## Examples



```console
$ docker unpause my_container
my_container
```
