+++
title = "docker network inspect"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/network/inspect/](https://docs.docker.com/reference/cli/docker/network/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker network inspect

| Description | Display detailed information on one or more networks    |
| :---------- | ------------------------------------------------------- |
| Usage       | `docker network inspect [OPTIONS] NETWORK [NETWORK...]` |

## Description

Returns information about one or more networks. By default, this command renders all results in a JSON object.

​	返回一个或多个网络的信息。默认情况下，此命令将所有结果呈现为 JSON 对象。

## Options

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `-f, --format`  |         | 使用自定义模板格式化输出: 'json': 以 JSON 格式打印输出 'TEMPLATE': 使用给定的 Go 模板格式化输出。详细信息请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-v, --verbose` |         | 详细诊断输出 Verbose output for diagnostics                  |
