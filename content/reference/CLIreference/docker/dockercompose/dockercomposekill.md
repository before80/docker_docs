+++
title = "docker compose kill"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/kill/](https://docs.docker.com/reference/cli/docker/compose/kill/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose kill

| Description | Force stop service containers                |
| :---------- | -------------------------------------------- |
| Usage       | `docker compose kill [OPTIONS] [SERVICE...]` |

## Description

Forces running containers to stop by sending a `SIGKILL` signal. Optionally the signal can be passed, for example:

​	通过发送 `SIGKILL` 信号强制停止正在运行的容器。可以选择性地传递信号，例如：

```console
$ docker compose kill -s SIGINT
```

## Options

| Option             | Default   | Description                                                  |
| ------------------ | --------- | ------------------------------------------------------------ |
| `--remove-orphans` |           | 删除在 Compose 文件中未定义的服务的容器 Remove containers for services not defined in the Compose file |
| `-s, --signal`     | `SIGKILL` | 发送到容器的信号 SIGNAL to send to the container             |
