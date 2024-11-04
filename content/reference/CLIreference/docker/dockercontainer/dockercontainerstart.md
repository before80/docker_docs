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

​	启动一个或多个已停止的容器

## Options

| Option              | Default | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| `-a, --attach`      |         | 附加标准输出/标准错误并转发信号 Attach STDOUT/STDERR and forward signals |
| `--checkpoint`      |         | 实验性（守护进程） 从此检查点恢复 experimental (daemon) Restore from this checkpoint |
| `--checkpoint-dir`  |         | 实验性（守护进程） 使用自定义检查点存储目录 experimental (daemon) Use a custom checkpoint storage directory |
| `--detach-keys`     |         | 覆盖分离容器的键序列 Override the key sequence for detaching a container |
| `-i, --interactive` |         | 附加容器的标准输入 Attach container's STDIN                  |

## Examples



```console
$ docker start my_container
```
