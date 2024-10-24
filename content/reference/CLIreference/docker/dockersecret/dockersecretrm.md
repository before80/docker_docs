+++
title = "docker secret rm"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/secret/rm/](https://docs.docker.com/reference/cli/docker/secret/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker secret rm

| Description | Remove one or more secrets            |
| :---------- | ------------------------------------- |
| Usage       | `docker secret rm SECRET [SECRET...]` |
| Aliases     | `docker secret remove`                |

Swarm This command works with the Swarm orchestrator.

## Description

Removes the specified secrets from the swarm.

For detailed information about using secrets, refer to [manage sensitive data with Docker secrets]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}}).

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Examples

This example removes a secret:



```console
$ docker secret rm secret.json
sapth4csdo5b6wz2p5uimh5xg
```

> **Warning**
>
> Unlike `docker rm`, this command does not ask for confirmation before removing a secret.
