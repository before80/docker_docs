+++
title = "docker config create"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/config/create/](https://docs.docker.com/reference/cli/docker/config/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker config create

| Description | Create a config from a file or STDIN           |
| :---------- | ---------------------------------------------- |
| Usage       | `docker config create [OPTIONS] CONFIG file|-` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令与 Swarm 调度器配合使用。

## Description

Creates a config using standard input or from a file for the config content.

​	使用标准输入或文件创建配置内容。

For detailed information about using configs, refer to [store configuration data using Docker Configs]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}}).

​	有关使用配置的详细信息，请参考 [使用 Docker 配置存储配置数据]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}})。

> **Note**
>
> This is a cluster management command, and must be executed on a Swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参考文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                      |
| ------------------------------------------------------------ | ------- | ------------------------------------------------ |
| [`-l, --label`](https://docs.docker.com/reference/cli/docker/config/create/#label) |         | 配置标签 Config labels                           |
| `--template-driver`                                          |         | API 1.37+ 模板驱动程序 API 1.37+ Template driver |

## Examples

### 创建配置 Create a config



```console
$ printf <config> | docker config create my_config -

onakdyv307se2tl7nl20anokv

$ docker config ls

ID                          NAME                CREATED             UPDATED
onakdyv307se2tl7nl20anokv   my_config           6 seconds ago       6 seconds ago
```

### 从文件创建配置 Create a config with a file



```console
$ docker config create my_config ./config.json

dg426haahpi5ezmkkj5kyl3sn

$ docker config ls

ID                          NAME                CREATED             UPDATED
dg426haahpi5ezmkkj5kyl3sn   my_config           7 seconds ago       7 seconds ago
```

### 创建带标签的配置（`-l`, `--label`） Create a config with labels (`-l`, `--label`)



```console
$ docker config create \
    --label env=dev \
    --label rev=20170324 \
    my_config ./config.json

eo7jnzguqgtpdah3cm5srfb97
```



```console
$ docker config inspect my_config

[
    {
        "ID": "eo7jnzguqgtpdah3cm5srfb97",
        "Version": {
            "Index": 17
        },
        "CreatedAt": "2017-03-24T08:15:09.735271783Z",
        "UpdatedAt": "2017-03-24T08:15:09.735271783Z",
        "Spec": {
            "Name": "my_config",
            "Labels": {
                "env": "dev",
                "rev": "20170324"
            },
            "Data": "aGVsbG8K"
        }
    }
]
```
