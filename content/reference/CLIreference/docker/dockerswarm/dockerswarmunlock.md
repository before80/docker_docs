+++
title = "docker swarm unlock"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/swarm/unlock/](https://docs.docker.com/reference/cli/docker/swarm/unlock/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker swarm unlock

| Description | Unlock swarm          |
| :---------- | --------------------- |
| Usage       | `docker swarm unlock` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 协调器。

## Description

Unlocks a locked manager using a user-supplied unlock key. This command must be used to reactivate a manager after its Docker daemon restarts if the autolock setting is turned on. The unlock key is printed at the time when autolock is enabled, and is also available from the `docker swarm unlock-key` command.

​	使用用户提供的解锁密钥解锁被锁定的管理节点。如果启用了自动锁定设置，在 Docker 守护进程重新启动后必须使用此命令重新激活管理节点。启用自动锁定时会打印解锁密钥，密钥也可通过 `docker swarm unlock-key` 命令获取。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理器和工作节点，请参阅 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Examples



```console
$ docker swarm unlock
Enter unlock key:
```
