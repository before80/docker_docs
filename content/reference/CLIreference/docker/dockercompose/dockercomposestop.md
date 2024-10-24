+++
title = "docker compose stop"
date = 2024-10-23T14:54:43+08:00
weight = 210
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/stop/](https://docs.docker.com/reference/cli/docker/compose/stop/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose top

| Description | Display the running processes      |
| :---------- | ---------------------------------- |
| Usage       | `docker compose top [SERVICES...]` |

## [Description](https://docs.docker.com/reference/cli/docker/compose/top/#description)

Displays the running processes

## [Examples](https://docs.docker.com/reference/cli/docker/compose/top/#examples)



```console
$ docker compose top
example_foo_1
UID    PID      PPID     C    STIME   TTY   TIME       CMD
root   142353   142331   2    15:33   ?     00:00:00   ping localhost -c 5
```
