+++
title = "docker compose port"
date = 2024-10-23T14:54:43+08:00
weight = 130
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/port/](https://docs.docker.com/reference/cli/docker/compose/port/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose port

| Description | Print the public port for a port binding             |
| :---------- | ---------------------------------------------------- |
| Usage       | `docker compose port [OPTIONS] SERVICE PRIVATE_PORT` |

## Description

Prints the public port for a port binding

​	打印端口绑定的公共端口

## Options

| Option       | Default | Description                                                  |
| ------------ | ------- | ------------------------------------------------------------ |
| `--index`    |         | 如果服务有多个副本，则为容器的索引 Index of the container if service has multiple replicas |
| `--protocol` | `tcp`   | tcp 或 udp - tcp or udp                                      |
