+++
title = "docker node"
date = 2024-10-23T14:54:43+08:00
weight = 160
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/node/](https://docs.docker.com/reference/cli/docker/node/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker node

| Description | Manage Swarm nodes |
| :---------- | ------------------ |
| Usage       | `docker node`      |

Swarm This command works with the Swarm orchestrator.

## Description

Manage nodes.

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker node demote`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodedemote" >}}) | Demote one or more nodes from manager in the swarm           |
| [`docker node inspect`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeinspect" >}}) | Display detailed information on one or more nodes            |
| [`docker node ls`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodels" >}}) | List nodes in the swarm                                      |
| [`docker node promote`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodepromote" >}}) | Promote one or more nodes to manager in the swarm            |
| [`docker node ps`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeps" >}}) | List tasks running on one or more nodes, defaults to current node |
| [`docker node rm`]({{< ref "/reference/CLIreference/docker/dockernode/dockernoderm" >}}) | Remove one or more nodes from the swarm                      |
| [`docker node update`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeupdate" >}}) | Update a node                                                |
