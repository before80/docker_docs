+++
title = "docker container start"
date = 2024-10-23T14:54:43+08:00
weight = 150
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/start/](https://docs.docker.com/reference/cli/docker/container/start/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container start

| Description | Start one or more stopped containers                        |
| :---------- | ----------------------------------------------------------- |
| Usage       | `docker container start [OPTIONS] CONTAINER [CONTAINER...]` |
| Aliases     | `docker start`                                              |

## Description

Start one or more stopped containers

## Options

| Option              | Default | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| `-a, --attach`      |         | Attach STDOUT/STDERR and forward signals                     |
| `--checkpoint`      |         | experimental (daemon) Restore from this checkpoint           |
| `--checkpoint-dir`  |         | experimental (daemon) Use a custom checkpoint storage directory |
| `--detach-keys`     |         | Override the key sequence for detaching a container          |
| `-i, --interactive` |         | Attach container's STDIN                                     |

## Examples



```console
$ docker start my_container
```
