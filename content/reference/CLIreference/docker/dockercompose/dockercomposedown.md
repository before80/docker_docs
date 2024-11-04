+++
title = "docker compose down"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/down/](https://docs.docker.com/reference/cli/docker/compose/down/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose down

| Description | Stop and remove containers, networks       |
| :---------- | ------------------------------------------ |
| Usage       | `docker compose down [OPTIONS] [SERVICES]` |

## Description

Stops containers and removes containers, networks, volumes, and images created by `up`.

​	停止容器并删除由 `up` 创建的容器、网络、卷和镜像。

By default, the only things removed are:

​	默认情况下，唯一被删除的内容是：

- Containers for services defined in the Compose file.
  - Compose 文件中定义的服务的容器。

- Networks defined in the networks section of the Compose file.
  - Compose 文件的网络部分中定义的网络。

- The default network, if one is used.
  - 使用的默认网络（如果有）。


Networks and volumes defined as external are never removed.

​	定义为外部的网络和卷永远不会被删除。

Anonymous volumes are not removed by default. However, as they don’t have a stable name, they are not automatically mounted by a subsequent `up`. For data that needs to persist between updates, use explicit paths as bind mounts or named volumes.

​	匿名卷默认不会被删除。然而，由于它们没有稳定名称，因此不会在后续的 `up` 中自动挂载。对于需要在更新之间持久化的数据，请使用显式路径作为绑定挂载或命名卷。

## Options

| Option             | Default | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| `--remove-orphans` |         | 删除在 Compose 文件中未定义的服务的容器 Remove containers for services not defined in the Compose file |
| `--rmi`            |         | 删除服务使用的镜像。“local” 只删除没有自定义标签的镜像 Remove images used by services. "local" remove only images that don't have a custom tag ("local"\|"all") |
| `-t, --timeout`    |         | 指定以秒为单位的关闭超时 Specify a shutdown timeout in seconds |
| `-v, --volumes`    |         | 删除 Compose 文件的 “volumes” 部分中声明的命名卷和附加到容器的匿名卷 Remove named volumes declared in the "volumes" section of the Compose file and anonymous volumes attached to containers |
