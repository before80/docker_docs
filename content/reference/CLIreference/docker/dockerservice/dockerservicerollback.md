+++
title = "docker service rollback"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/service/rollback/](https://docs.docker.com/reference/cli/docker/service/rollback/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker service rollback

| Description | Revert changes to a service's configuration |
| :---------- | ------------------------------------------- |
| Usage       | `docker service rollback [OPTIONS] SERVICE` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 编排器。

## Description

Roll back a specified service to its previous version from the swarm.

​	将指定服务回滚到其之前的版本。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option         | Default | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| `-d, --detach` |         | API 1.29+ 立即退出，而不是等待服务趋于稳定  API 1.29+ Exit immediately instead of waiting for the service to converge |
| `-q, --quiet`  |         | 隐藏进度输出  Suppress progress output                       |

## Examples

### 回滚服务到之前的版本  Roll back to the previous version of a service

Use the `docker service rollback` command to roll back to the previous version of a service. After executing this command, the service is reverted to the configuration that was in place before the most recent `docker service update` command.

​	使用 `docker service rollback` 命令将服务回滚到之前的版本。执行此命令后，服务将恢复到上一次 `docker service update` 命令之前的配置。

The following example creates a service with a single replica, updates the service to use three replicas, and then rolls back the service to the previous version, having one replica.

​	以下示例创建一个具有单个副本的服务，更新该服务为三个副本，然后将其回滚到之前的版本，仅保留一个副本。

Create a service with a single replica:

​	创建一个单副本服务：

```console
$ docker service create --name my-service -p 8080:80 nginx:alpine
```

Confirm that the service is running with a single replica:

​	确认服务正在运行并具有单个副本：

```console
$ docker service ls

ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
xbw728mf6q0d        my-service          replicated          1/1                 nginx:alpine        *:8080->80/tcp
```

Update the service to use three replicas:

​	将服务更新为三个副本：

```console
$ docker service update --replicas=3 my-service

$ docker service ls

ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
xbw728mf6q0d        my-service          replicated          3/3                 nginx:alpine        *:8080->80/tcp
```

Now roll back the service to its previous version, and confirm it is running a single replica again:

​	现在回滚服务到之前的版本，并确认它再次运行单个副本。

```console
$ docker service rollback my-service

$ docker service ls

ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
xbw728mf6q0d        my-service          replicated          1/1                 nginx:alpine        *:8080->80/tcp
```

