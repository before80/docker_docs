+++
title = "docker config rm"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/config/rm/](https://docs.docker.com/reference/cli/docker/config/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker config rm

| Description | Remove one or more configs            |
| :---------- | ------------------------------------- |
| Usage       | `docker config rm CONFIG [CONFIG...]` |
| Aliases     | `docker config remove`                |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令与 Swarm 调度器配合使用。

## Description

Removes the specified configs from the Swarm.

​	从 Swarm 中删除指定的配置。

For detailed information about using configs, refer to [store configuration data using Docker Configs]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}}).

​	有关使用配置的详细信息，请参考 [使用 Docker 配置存储配置数据]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}})。

> **Note**
>
> This is a cluster management command, and must be executed on a Swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参考文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Examples

This example removes a config:

​	以下示例删除一个配置：



```console
$ docker config rm my_config
sapth4csdo5b6wz2p5uimh5xg
```

> **Warning**
>
> This command doesn't ask for confirmation before removing a config.
