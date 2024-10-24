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

## Examples

Start a container in the background.



```console
$ docker run -dit --name=my_container ubuntu bash
```

Run `docker wait`, which should block until the container exits.



```console
$ docker wait my_container
```

In another terminal, stop the first container. The `docker wait` command above returns the exit code.



```console
$ docker stop my_container
```

This is the same `docker wait` command from above, but it now exits, returning `0`.



```console
$ docker wait my_container

0
```
