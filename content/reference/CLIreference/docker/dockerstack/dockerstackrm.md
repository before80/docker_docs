+++
title = "docker stack rm"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/stack/rm/](https://docs.docker.com/reference/cli/docker/stack/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker stack rm

| Description | Remove one or more stacks                    |
| :---------- | -------------------------------------------- |
| Usage       | `docker stack rm [OPTIONS] STACK [STACK...]` |
| Aliases     | `docker stack remove``docker stack down`     |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 编排器。

## Description

Remove the stack from the swarm.

​	从 Swarm 中移除栈。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理器和工作节点，请参阅 Swarm 模式部分。

## Options

| Option         | Default | Description                                |
| -------------- | ------- | ------------------------------------------ |
| `-d, --detach` | `true`  | 不等待栈移除 Do not wait for stack removal |

## Examples

### 移除栈 Remove a stack

This will remove the stack with the name `myapp`. Services, networks, and secrets associated with the stack will be removed.

​	这将移除名为 `myapp` 的栈。与栈关联的服务、网络和密钥将被移除。



```console
$ docker stack rm myapp

Removing service myapp_redis
Removing service myapp_web
Removing service myapp_lb
Removing network myapp_default
Removing network myapp_frontend
```

### 移除多个栈 Remove multiple stacks

This will remove all the specified stacks, `myapp` and `vossibility`. Services, networks, and secrets associated with all the specified stacks will be removed.

​	这将移除所有指定的栈，包括 `myapp` 和 `vossibility`。所有指定栈关联的服务、网络和密钥将被移除。

```console
$ docker stack rm myapp vossibility

Removing service myapp_redis
Removing service myapp_web
Removing service myapp_lb
Removing network myapp_default
Removing network myapp_frontend
Removing service vossibility_nsqd
Removing service vossibility_logstash
Removing service vossibility_elasticsearch
Removing service vossibility_kibana
Removing service vossibility_ghollector
Removing service vossibility_lookupd
Removing network vossibility_default
Removing network vossibility_vossibility
```
