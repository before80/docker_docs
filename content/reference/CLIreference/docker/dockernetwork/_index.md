+++
title = "docker network"
date = 2024-10-23T14:54:43+08:00
weight = 150
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/network/](https://docs.docker.com/reference/cli/docker/network/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker network

| Description | Manage networks  |
| :---------- | ---------------- |
| Usage       | `docker network` |

## Description

Manage networks. You can use subcommands to create, inspect, list, remove, prune, connect, and disconnect networks.

​	管理网络。可以使用子命令来创建、检查、列出、删除、清理、连接和断开网络。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker network connect`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkconnect" >}}) | 将容器连接到网络 Connect a container to a network            |
| [`docker network create`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkcreate" >}}) | 创建一个网络 Create a network                                |
| [`docker network disconnect`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkdisconnect" >}}) | 将容器从网络中断开 Disconnect a container from a network     |
| [`docker network inspect`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkinspect" >}}) | 显示一个或多个网络的详细信息 Display detailed information on one or more networks |
| [`docker network ls`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkls" >}}) | 列出网络 List networks                                       |
| [`docker network prune`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkprune" >}}) | 删除所有未使用的网络 Remove all unused networks              |
| [`docker network rm`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkrm" >}}) | 删除一个或多个网络 Remove one or more networks               |

