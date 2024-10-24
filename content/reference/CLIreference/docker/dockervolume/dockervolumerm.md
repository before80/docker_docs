+++
title = "docker volume rm"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/volume/rm/](https://docs.docker.com/reference/cli/docker/volume/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker volume rm

| Description | Remove one or more volumes                      |
| :---------- | ----------------------------------------------- |
| Usage       | `docker volume rm [OPTIONS] VOLUME [VOLUME...]` |
| Aliases     | `docker volume remove`                          |

## Description

Remove one or more volumes. You can't remove a volume that's in use by a container.

## Options

| Option        | Default | Description                                        |
| ------------- | ------- | -------------------------------------------------- |
| `-f, --force` |         | API 1.25+ Force the removal of one or more volumes |

## Examples



```console
$ docker volume rm hello

hello
```
