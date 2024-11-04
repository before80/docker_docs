+++
title = "docker container inspect"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/inspect/](https://docs.docker.com/reference/cli/docker/container/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container inspect

| Description | Display detailed information on one or more containers       |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container inspect [OPTIONS] CONTAINER [CONTAINER...]` |

## Description

Display detailed information on one or more containers

​	显示一个或多个容器的详细信息

## Options

| Option         | Default | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| `-f, --format` |         | 使用自定义模板格式化输出： 'json': 以 JSON 格式打印 'TEMPLATE': 使用给定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/    Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-s, --size`   |         | 显示文件总大小 Display total file sizes                      |
