+++
title = "docker secret create"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/secret/create/](https://docs.docker.com/reference/cli/docker/secret/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker secret create

| Description | Create a secret from a file or STDIN as content  |
| :---------- | ------------------------------------------------ |
| Usage       | `docker secret create [OPTIONS] SECRET [file|-]` |

Swarm This command works with the Swarm orchestrator.

## Description

Creates a secret using standard input or from a file for the secret content.

For detailed information about using secrets, refer to [manage sensitive data with Docker secrets]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}}).

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Options

| Option                                                       | Default | Description               |
| ------------------------------------------------------------ | ------- | ------------------------- |
| `-d, --driver`                                               |         | API 1.31+ Secret driver   |
| [`-l, --label`](https://docs.docker.com/reference/cli/docker/secret/create/#label) |         | Secret labels             |
| `--template-driver`                                          |         | API 1.37+ Template driver |

## Examples

### Create a secret



```console
$ printf "my super secret password" | docker secret create my_secret -

onakdyv307se2tl7nl20anokv

$ docker secret ls

ID                          NAME                CREATED             UPDATED
onakdyv307se2tl7nl20anokv   my_secret           6 seconds ago       6 seconds ago
```

### Create a secret with a file



```console
$ docker secret create my_secret ./secret.json

dg426haahpi5ezmkkj5kyl3sn

$ docker secret ls

ID                          NAME                CREATED             UPDATED
dg426haahpi5ezmkkj5kyl3sn   my_secret           7 seconds ago       7 seconds ago
```

### Create a secret with labels (--label)



```console
$ docker secret create \
  --label env=dev \
  --label rev=20170324 \
  my_secret ./secret.json

eo7jnzguqgtpdah3cm5srfb97
```



```console
$ docker secret inspect my_secret

[
    {
        "ID": "eo7jnzguqgtpdah3cm5srfb97",
        "Version": {
            "Index": 17
        },
        "CreatedAt": "2017-03-24T08:15:09.735271783Z",
        "UpdatedAt": "2017-03-24T08:15:09.735271783Z",
        "Spec": {
            "Name": "my_secret",
            "Labels": {
                "env": "dev",
                "rev": "20170324"
            }
        }
    }
]
```
