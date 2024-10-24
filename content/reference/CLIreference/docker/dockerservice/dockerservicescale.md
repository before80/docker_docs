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

## Description

The scale command enables you to scale one or more replicated services either up or down to the desired number of replicas. This command cannot be applied on services which are global mode. The command will return immediately, but the actual scaling of the service may take some time. To stop all replicas of a service while keeping the service active in the swarm you can set the scale to 0.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Options

| Option         | Default | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| `-d, --detach` |         | API 1.29+ Exit immediately instead of waiting for the service to converge |

## Examples

### Scale a single service

The following command scales the "frontend" service to 50 tasks.



```console
$ docker service scale frontend=50

frontend scaled to 50
```

The following command tries to scale a global service to 10 tasks and returns an error.



```console
$ docker service create --mode global --name backend backend:latest

b4g08uwuairexjub6ome6usqh

$ docker service scale backend=10

backend: scale can only be used with replicated or replicated-job mode
```

Directly afterwards, run `docker service ls`, to see the actual number of replicas.



```console
$ docker service ls --filter name=frontend

ID            NAME      MODE        REPLICAS  IMAGE
3pr5mlvu3fh9  frontend  replicated  15/50     nginx:alpine
```

You can also scale a service using the [`docker service update`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}}) command. The following commands are equivalent:



```console
$ docker service scale frontend=50
$ docker service update --replicas=50 frontend
```

### Scale multiple services

The `docker service scale` command allows you to set the desired number of tasks for multiple services at once. The following example scales both the backend and frontend services:



```console
$ docker service scale backend=3 frontend=5

backend scaled to 3
frontend scaled to 5

$ docker service ls

ID            NAME      MODE        REPLICAS  IMAGE
3pr5mlvu3fh9  frontend  replicated  5/5       nginx:alpine
74nzcxxjv6fq  backend   replicated  3/3       redis:3.0.6
```
