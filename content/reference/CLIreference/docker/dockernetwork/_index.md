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

## Subcommands

| Command                                                      | Description                                          |
| :----------------------------------------------------------- | :--------------------------------------------------- |
| [`docker network connect`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkconnect" >}}) | Connect a container to a network                     |
| [`docker network create`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkcreate" >}}) | Create a network                                     |
| [`docker network disconnect`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkdisconnect" >}}) | Disconnect a container from a network                |
| [`docker network inspect`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkinspect" >}}) | Display detailed information on one or more networks |
| [`docker network ls`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkls" >}}) | List networks                                        |
| [`docker network prune`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkprune" >}}) | Remove all unused networks                           |
| [`docker network rm`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkrm" >}}) | Remove one or more networks                          |
