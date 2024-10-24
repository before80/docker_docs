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

## Description

Removes the specified configs from the Swarm.

For detailed information about using configs, refer to [store configuration data using Docker Configs]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}}).

> **Note**
>
> This is a cluster management command, and must be executed on a Swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Examples

This example removes a config:



```console
$ docker config rm my_config
sapth4csdo5b6wz2p5uimh5xg
```

> **Warning**
>
> This command doesn't ask for confirmation before removing a config.
