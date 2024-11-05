+++
title = "docker service"
date = 2024-10-23T14:54:43+08:00
weight = 210
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/service/](https://docs.docker.com/reference/cli/docker/service/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker service

| Description | Manage Swarm services |
| :---------- | --------------------- |
| Usage       | `docker service`      |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 协调器。

## Description

Manage Swarm services.

​	管理 Swarm 服务。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参考文档中的 Swarm 模式部分。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker service create`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}) | 创建一个新服务 Create a new service                          |
| [`docker service inspect`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceinspect" >}}) | 显示一个或多个服务的详细信息 Display detailed information on one or more services |
| [`docker service logs`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicelogs" >}}) | 获取服务或任务的日志 Fetch the logs of a service or task     |
| [`docker service ls`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicels" >}}) | 列出服务 List services                                       |
| [`docker service ps`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceps" >}}) | 列出一个或多个服务的任务 List the tasks of one or more services |
| [`docker service rm`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicerm" >}}) | 删除一个或多个服务 Remove one or more services               |
| [`docker service rollback`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicerollback" >}}) | 还原服务的配置更改 Revert changes to a service's configuration |
| [`docker service scale`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicescale" >}}) | 扩容一个或多个复制服务 Scale one or multiple replicated services |
| [`docker service update`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}}) | 更新服务 Update a service                                    |
