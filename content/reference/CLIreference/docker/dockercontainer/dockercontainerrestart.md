+++
title = "docker container restart"
date = 2024-10-23T14:54:43+08:00
weight = 130
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/restart/](https://docs.docker.com/reference/cli/docker/container/restart/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container restart

| Description | Restart one or more containers                               |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container restart [OPTIONS] CONTAINER [CONTAINER...]` |
| Aliases     | `docker restart`                                             |

## Description

Restart one or more containers

## Options

| Option         | Default | Description                                  |
| -------------- | ------- | -------------------------------------------- |
| `-s, --signal` |         | Signal to send to the container              |
| `-t, --time`   |         | Seconds to wait before killing the container |

## Examples



```console
$ docker restart my_container
```
