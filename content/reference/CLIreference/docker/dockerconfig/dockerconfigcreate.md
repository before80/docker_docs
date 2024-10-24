+++
title = "docker config create"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/config/create/](https://docs.docker.com/reference/cli/docker/config/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker config create

| Description | Create a config from a file or STDIN           |
| :---------- | ---------------------------------------------- |
| Usage       | `docker config create [OPTIONS] CONFIG file|-` |

Swarm This command works with the Swarm orchestrator.

## [Description](https://docs.docker.com/reference/cli/docker/config/create/#description)

Creates a config using standard input or from a file for the config content.

For detailed information about using configs, refer to [store configuration data using Docker Configs](https://docs.docker.com/engine/swarm/configs/).

> **Note**
>
> This is a cluster management command, and must be executed on a Swarm manager node. To learn about managers and workers, refer to the [Swarm mode section](https://docs.docker.com/engine/swarm/) in the documentation.

## [Options](https://docs.docker.com/reference/cli/docker/config/create/#options)

| Option                                                       | Default | Description               |
| ------------------------------------------------------------ | ------- | ------------------------- |
| [`-l, --label`](https://docs.docker.com/reference/cli/docker/config/create/#label) |         | Config labels             |
| `--template-driver`                                          |         | API 1.37+ Template driver |

## [Examples](https://docs.docker.com/reference/cli/docker/config/create/#examples)

### [Create a config](https://docs.docker.com/reference/cli/docker/config/create/#create-a-config)



```console
$ printf <config> | docker config create my_config -

onakdyv307se2tl7nl20anokv

$ docker config ls

ID                          NAME                CREATED             UPDATED
onakdyv307se2tl7nl20anokv   my_config           6 seconds ago       6 seconds ago
```

### [Create a config with a file](https://docs.docker.com/reference/cli/docker/config/create/#create-a-config-with-a-file)



```console
$ docker config create my_config ./config.json

dg426haahpi5ezmkkj5kyl3sn

$ docker config ls

ID                          NAME                CREATED             UPDATED
dg426haahpi5ezmkkj5kyl3sn   my_config           7 seconds ago       7 seconds ago
```

### [Create a config with labels (-l, --label)](https://docs.docker.com/reference/cli/docker/config/create/#label)



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
