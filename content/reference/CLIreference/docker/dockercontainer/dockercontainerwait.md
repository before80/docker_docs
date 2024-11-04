+++
title = "docker container wait"
date = 2024-10-23T14:54:43+08:00
weight = 210
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/wait/](https://docs.docker.com/reference/cli/docker/container/wait/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container wait

| Description | Block until one or more containers stop, then print their exit codes |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container wait CONTAINER [CONTAINER...]`             |
| Aliases     | `docker wait`                                                |

## Description

Block until one or more containers stop, then print their exit codes

​	在一个或多个容器停止时阻塞，然后打印它们的退出代码

## Examples

Start a container in the background.

​	在后台启动一个容器。



```console
$ docker run -dit --name=my_container ubuntu bash
```

Run `docker wait`, which should block until the container exits.

​	运行 `docker wait`，这应该会阻塞直到容器退出。



```console
$ docker wait my_container
```

In another terminal, stop the first container. The `docker wait` command above returns the exit code.

​	在另一个终端中，停止第一个容器。上面的 `docker wait` 命令将返回退出代码。



```console
$ docker stop my_container
```

This is the same `docker wait` command from above, but it now ets, returning `0`.

​	这是上面相同的 `docker wait` 命令，但现在退出，返回 `0`。



```console
$ docker wait my_container

0
```
