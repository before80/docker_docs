+++
title = "docker service rm"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/service/rm/](https://docs.docker.com/reference/cli/docker/service/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker service rm

| Description | Remove one or more services              |
| :---------- | ---------------------------------------- |
| Usage       | `docker service rm SERVICE [SERVICE...]` |
| Aliases     | `docker service remove`                  |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 编排器。

## Description

Removes the specified services from the swarm.

​	从 Swarm 中移除指定的服务。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Examples

Remove the `redis` service:

​	移除 `redis` 服务：



```console
$ docker service rm redis

redis

$ docker service ls

ID  NAME  MODE  REPLICAS  IMAGE
```

> **Warning**
>
> Unlike `docker rm`, this command does not ask for confirmation before removing a running service.
>
> ​	不同于 `docker rm`，此命令在移除正在运行的服务时不会请求确认。
