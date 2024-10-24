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

## Description

Unlocks a locked manager using a user-supplied unlock key. This command must be used to reactivate a manager after its Docker daemon restarts if the autolock setting is turned on. The unlock key is printed at the time when autolock is enabled, and is also available from the `docker swarm unlock-key` command.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Examples



```console
$ docker swarm unlock
Enter unlock key:
```
