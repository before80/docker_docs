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

## Description

Manage Swarm services.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Subcommands

| Command                                                      | Description                                          |
| :----------------------------------------------------------- | :--------------------------------------------------- |
| [`docker service create`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}) | Create a new service                                 |
| [`docker service inspect`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceinspect" >}}) | Display detailed information on one or more services |
| [`docker service logs`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicelogs" >}}) | Fetch the logs of a service or task                  |
| [`docker service ls`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicels" >}}) | List services                                        |
| [`docker service ps`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceps" >}}) | List the tasks of one or more services               |
| [`docker service rm`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicerm" >}}) | Remove one or more services                          |
| [`docker service rollback`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicerollback" >}}) | Revert changes to a service's configuration          |
| [`docker service scale`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicescale" >}}) | Scale one or multiple replicated services            |
| [`docker service update`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}}) | Update a service                                     |
