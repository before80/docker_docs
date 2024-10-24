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

## Description

Remove the stack from the swarm.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Options

| Option         | Default | Description                   |
| -------------- | ------- | ----------------------------- |
| `-d, --detach` | `true`  | Do not wait for stack removal |

## Examples

### Remove a stack

This will remove the stack with the name `myapp`. Services, networks, and secrets associated with the stack will be removed.



```console
$ docker stack rm myapp

Removing service myapp_redis
Removing service myapp_web
Removing service myapp_lb
Removing network myapp_default
Removing network myapp_frontend
```

### Remove multiple stacks

This will remove all the specified stacks, `myapp` and `vossibility`. Services, networks, and secrets associated with all the specified stacks will be removed.



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
