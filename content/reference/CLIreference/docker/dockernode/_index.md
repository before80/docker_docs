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

​	Swarm 该命令适用于 Swarm 协调器。

## Description

Manage nodes.

​	管理节点。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker node demote`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodedemote" >}}) | 将一个或多个节点从 Swarm 管理器降级 Demote one or more nodes from manager in the swarm |
| [`docker node inspect`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeinspect" >}}) | 显示一个或多个节点的详细信息 Display detailed information on one or more nodes |
| [`docker node ls`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodels" >}}) | 列出 Swarm 中的节点 List nodes in the swarm                  |
| [`docker node promote`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodepromote" >}}) | 将一个或多个节点提升为 Swarm 管理器 Promote one or more nodes to manager in the swarm |
| [`docker node ps`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeps" >}}) | 列出在一个或多个节点上运行的任务，默认显示当前节点 List tasks running on one or more nodes, defaults to current node |
| [`docker node rm`]({{< ref "/reference/CLIreference/docker/dockernode/dockernoderm" >}}) | 从 Swarm 中删除一个或多个节点 Remove one or more nodes from the swarm |
| [`docker node update`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeupdate" >}}) | 更新一个节点 Update a node                                   |
