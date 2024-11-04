+++
title = "docker network rm"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/network/rm/](https://docs.docker.com/reference/cli/docker/network/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker network rm

| Description | Remove one or more networks              |
| :---------- | ---------------------------------------- |
| Usage       | `docker network rm NETWORK [NETWORK...]` |
| Aliases     | `docker network remove`                  |

## Description

Removes one or more networks by name or identifier. To remove a network, you must first disconnect any containers connected to it.

​	删除一个或多个网络，可以通过名称或标识符删除网络。要删除一个网络，必须首先断开任何连接到它的容器。

## Options

| Option        | Default | Description                                                  |
| ------------- | ------- | ------------------------------------------------------------ |
| `-f, --force` |         | 如果网络不存在则不报错 Do not error if the network does not exist |

## Examples

### 删除一个网络 Remove a network

To remove the network named 'my-network':

​	要删除名为 'my-network' 的网络：



```console
$ docker network rm my-network
```

### 删除多个网络 Remove multiple networks

To delete multiple networks in a single `docker network rm` command, provide multiple network names or ids. The following example deletes a network with id `3695c422697f` and a network named `my-network`:

​	要在单个 `docker network rm` 命令中删除多个网络，提供多个网络名称或 ID。以下示例删除 ID 为 `3695c422697f` 的网络和名为 `my-network` 的网络：



```console
$ docker network rm 3695c422697f my-network
```

When you specify multiple networks, the command attempts to delete each in turn. If the deletion of one network fails, the command continues to the next on the list and tries to delete that. The command reports success or failure for each deletion.

​	当指定多个网络时，命令会依次尝试删除每个网络。如果删除某个网络失败，命令将继续尝试删除列表中的下一个网络。命令会报告每次删除的成功或失败情况。
