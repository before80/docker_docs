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

## Description

Removes the specified services from the swarm.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Examples

Remove the `redis` service:



```console
$ docker service rm redis

redis

$ docker service ls

ID  NAME  MODE  REPLICAS  IMAGE
```

> **Warning**
>
> Unlike `docker rm`, this command does not ask for confirmation before removing a running service.
