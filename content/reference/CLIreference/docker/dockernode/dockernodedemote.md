+++
title = "docker node demote"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/node/demote/](https://docs.docker.com/reference/cli/docker/node/demote/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker node demote

| Description | Demote one or more nodes from manager in the swarm |
| :---------- | -------------------------------------------------- |
| Usage       | `docker node demote NODE [NODE...]`                |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 协调器。

## Description

Demotes an existing manager so that it is no longer a manager.

​	将现有的管理节点降级，使其不再是管理器。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。有关管理节点和工作节点的更多信息，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Examples



```console
$ docker node demote <node name>
```
