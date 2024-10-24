+++
title = "docker scout repo enable"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/scout/repo/enable/](https://docs.docker.com/reference/cli/docker/scout/repo/enable/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker scout repo enable

| Description | Enable Docker Scout                     |
| :---------- | --------------------------------------- |
| Usage       | `docker scout repo enable [REPOSITORY]` |

## Description

The docker scout repo enable command enables Docker Scout on repositories.

## Options

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `--all`         |         | Enable all repositories of the organization. Can not be used with --filter. |
| `--filter`      |         | Regular expression to filter repositories by name            |
| `--integration` |         | Name of the integration to use for enabling an image         |
| `--org`         |         | Namespace of the Docker organization                         |
| `--registry`    |         | Container Registry                                           |

## Examples

### Enable a specific repository



```console
$ docker scout repo enable my/repository
```

### Enable all repositories of the organization



```console
$ docker scout repo enable --all
```

### Enable some repositories based on a filter



```console
$ docker scout repo enable --filter namespace/backend
```

### Enable a repository from a specific registry



```console
$ docker scout repo enable my/repository --registry 123456.dkr.ecr.us-east-1.amazonaws.com
```
