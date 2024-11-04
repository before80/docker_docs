+++
title = "docker container stop"
date = 2024-10-23T14:54:43+08:00
weight = 170
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/stop/](https://docs.docker.com/reference/cli/docker/container/stop/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container stop

| Description | Stop one or more running containers                        |
| :---------- | ---------------------------------------------------------- |
| Usage       | `docker container stop [OPTIONS] CONTAINER [CONTAINER...]` |
| Aliases     | `docker stop`                                              |

## Description

The main process inside the container will receive `SIGTERM`, and after a grace period, `SIGKILL`. The first signal can be changed with the `STOPSIGNAL` instruction in the container's Dockerfile, or the `--stop-signal` option to `docker run`.

​	容器内的主进程将接收到 `SIGTERM` 信号，在一个宽限期后，将接收到 `SIGKILL` 信号。第一个信号可以通过容器的 Dockerfile 中的 `STOPSIGNAL` 指令或 `docker run` 的 `--stop-signal` 选项进行更改。

## Options

| Option         | Default | Description                                  |
| -------------- | ------- | -------------------------------------------- |
| `-s, --signal` |         | Signal to send to the container              |
| `-t, --time`   |         | Seconds to wait before killing the container |

## Examples



```console
$ docker stop my_container
```
