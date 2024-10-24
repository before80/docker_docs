+++
title = "docker node promote"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/node/promote/](https://docs.docker.com/reference/cli/docker/node/promote/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker node promote

| Description | Promote one or more nodes to manager in the swarm |
| :---------- | ------------------------------------------------- |
| Usage       | `docker node promote NODE [NODE...]`              |

Swarm This command works with the Swarm orchestrator.

## Description

Promotes a node to manager. This command can only be executed on a manager node.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Examples



```console
$ docker node promote <node name>
```
