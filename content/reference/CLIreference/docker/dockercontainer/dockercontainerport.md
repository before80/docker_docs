+++
title = "docker container port"
date = 2024-10-23T14:54:43+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/port/](https://docs.docker.com/reference/cli/docker/container/port/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container port

| Description | List port mappings or a specific mapping for the container |
| :---------- | ---------------------------------------------------------- |
| Usage       | `docker container port CONTAINER [PRIVATE_PORT[/PROTO]]`   |
| Aliases     | `docker port`                                              |

## Description

List port mappings or a specific mapping for the container

## Examples

### Show all mapped ports

You can find out all the ports mapped by not specifying a `PRIVATE_PORT`, or just a specific mapping:



```console
$ docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                                            NAMES
b650456536c7        busybox:latest      top                 54 minutes ago      Up 54 minutes       0.0.0.0:1234->9876/tcp, 0.0.0.0:4321->7890/tcp   test

$ docker port test

7890/tcp -> 0.0.0.0:4321
9876/tcp -> 0.0.0.0:1234

$ docker port test 7890/tcp

0.0.0.0:4321

$ docker port test 7890/udp

2014/06/24 11:53:36 Error: No public port '7890/udp' published for test

$ docker port test 7890

0.0.0.0:4321
```
