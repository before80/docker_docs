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

## Options

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `-f, --format`  |         | Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-v, --verbose` |         | Verbose output for diagnostics                               |
