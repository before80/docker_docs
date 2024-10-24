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

## Options

| Option        | Default | Description                                |
| ------------- | ------- | ------------------------------------------ |
| `-f, --force` |         | Do not error if the network does not exist |

## Examples

### Remove a network

To remove the network named 'my-network':



```console
$ docker network rm my-network
```

### Remove multiple networks

To delete multiple networks in a single `docker network rm` command, provide multiple network names or ids. The following example deletes a network with id `3695c422697f` and a network named `my-network`:



```console
$ docker network rm 3695c422697f my-network
```

When you specify multiple networks, the command attempts to delete each in turn. If the deletion of one network fails, the command continues to the next on the list and tries to delete that. The command reports success or failure for each deletion.
