+++
title = "docker service scale"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/service/scale/](https://docs.docker.com/reference/cli/docker/service/scale/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker service scale

| Description | Scale one or multiple replicated services                    |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker service scale SERVICE=REPLICAS [SERVICE=REPLICAS...]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 编排器。

## Description

The scale command enables you to scale one or more replicated services either up or down to the desired number of replicas. This command cannot be applied on services which are global mode. The command will return immediately, but the actual scaling of the service may take some time. To stop all replicas of a service while keeping the service active in the swarm you can set the scale to 0.

​	`scale` 命令允许您将一个或多个复制服务的副本数量按需进行扩容或缩容。此命令不能应用于全局模式的服务。命令会立即返回，但服务的实际扩容可能需要一些时间。若要在保持服务活跃的情况下停止所有副本，可以将扩容值设为 0。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option         | Default | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| `-d, --detach` |         | API 1.29+ 立即退出，而不是等待服务趋于稳定  API 1.29+ Exit immediately instead of waiting for the service to converge |

## Examples

### 扩容单个服务 Scale a single service

The following command scales the "frontend" service to 50 tasks.

​	以下命令将 "frontend" 服务扩容至 50 个任务。



```console
$ docker service scale frontend=50

frontend scaled to 50
```

The following command tries to scale a global service to 10 tasks and returns an error.

​	以下命令尝试将一个全局服务扩容至 10 个任务，并返回错误。



```console
$ docker service create --mode global --name backend backend:latest

b4g08uwuairexjub6ome6usqh

$ docker service scale backend=10

backend: scale can only be used with replicated or replicated-job mode
```

Directly afterwards, run `docker service ls`, to see the actual number of replicas.

​	随后，运行 `docker service ls`，查看实际的副本数量。



```console
$ docker service ls --filter name=frontend

ID            NAME      MODE        REPLICAS  IMAGE
3pr5mlvu3fh9  frontend  replicated  15/50     nginx:alpine
```

You can also scale a service using the [`docker service update`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}}) command. The following commands are equivalent:

​	您也可以使用 [`docker service update`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}}) 命令扩容服务。以下命令是等效的：

```console
$ docker service scale frontend=50
$ docker service update --replicas=50 frontend
```

### 扩容多个服务 Scale multiple services

The `docker service scale` command allows you to set the desired number of tasks for multiple services at once. The following example scales both the backend and frontend services:

​	`docker service scale` 命令允许您一次设置多个服务的目标任务数。以下示例同时扩容 backend 和 frontend 服务。



```console
$ docker service scale backend=3 frontend=5

backend scaled to 3
frontend scaled to 5

$ docker service ls

ID            NAME      MODE        REPLICAS  IMAGE
3pr5mlvu3fh9  frontend  replicated  5/5       nginx:alpine
74nzcxxjv6fq  backend   replicated  3/3       redis:3.0.6
```
