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

​	Swarm 该命令可在 Swarm 编排器中使用。

## Description

Removes the specified secrets from the swarm.

​	从 Swarm 中删除指定的机密。

For detailed information about using secrets, refer to [manage sensitive data with Docker secrets]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}}).

​	有关使用机密的详细信息，请参阅 [使用 Docker 机密管理敏感数据]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}})。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。关于管理节点和工作节点的更多信息，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Examples

This example removes a secret:

​	以下示例删除一个机密：

```console
$ docker secret rm secret.json
sapth4csdo5b6wz2p5uimh5xg
```

> **Warning**
>
> Unlike `docker rm`, this command does not ask for confirmation before removing a secret.
>
> ​	与 `docker rm` 不同，此命令在删除机密之前不会请求确认。
