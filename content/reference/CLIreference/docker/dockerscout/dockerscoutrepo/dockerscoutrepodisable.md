+++
title = "docker scout repo disable"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/scout/repo/disable/](https://docs.docker.com/reference/cli/docker/scout/repo/disable/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker scout repo disable

| Description | Disable Docker Scout                     |
| :---------- | ---------------------------------------- |
| Usage       | `docker scout repo disable [REPOSITORY]` |

## Description

The docker scout repo disable command disables Docker Scout on repositories.

## Options

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `--all`         |         | Disable all repositories of the organization. Can not be used with --filter. |
| `--filter`      |         | Regular expression to filter repositories by name            |
| `--integration` |         | Name of the integration to use for enabling an image         |
| `--org`         |         | Namespace of the Docker organization                         |
| `--registry`    |         | Container Registry                                           |

## Examples

### Disable a specific repository



```console
$ docker scout repo disable my/repository
```

### Disable all repositories of the organization



```console
$ docker scout repo disable --all
```

### Disable some repositories based on a filter



```console
$ docker scout repo disable --filter namespace/backend
```

### Disable a repository from a specific registry



```console
$ docker scout repo disable my/repository --registry 123456.dkr.ecr.us-east-1.amazonaws.com
```
