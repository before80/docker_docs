+++
title = "docker network disconnect"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/network/disconnect/](https://docs.docker.com/reference/cli/docker/network/disconnect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker network disconnect

| Description | Disconnect a container from a network                   |
| :---------- | ------------------------------------------------------- |
| Usage       | `docker network disconnect [OPTIONS] NETWORK CONTAINER` |

## Description

Disconnects a container from a network. The container must be running to disconnect it from the network.

## Options

| Option        | Default | Description                                      |
| ------------- | ------- | ------------------------------------------------ |
| `-f, --force` |         | Force the container to disconnect from a network |

## Examples



```console
$ docker network disconnect multi-host-network container1
```
