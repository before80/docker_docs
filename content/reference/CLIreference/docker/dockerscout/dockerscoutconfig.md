+++
title = "docker scout config"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/scout/config/](https://docs.docker.com/reference/cli/docker/scout/config/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker scout config

| Description | Manage Docker Scout configuration   |
| :---------- | ----------------------------------- |
| Usage       | `docker scout config [KEY] [VALUE]` |

## Description

`docker scout config` allows you to list, get and set Docker Scout configuration.

Available configuration key:

- `organization`: Namespace of the Docker organization to be used by default.

## Examples

### List existing configuration



```console
$ docker scout config
organization=my-org-namespace
```

### Print configuration value



```console
$ docker scout config organization
my-org-namespace
```

### Set configuration value



```console
$ docker scout config organization my-org-namespace
    ✓ Successfully set organization to my-org-namespace
```
