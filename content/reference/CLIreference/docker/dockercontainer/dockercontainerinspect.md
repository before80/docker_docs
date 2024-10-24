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

## Options

| Option         | Default | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| `-f, --format` |         | Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-s, --size`   |         | Display total file sizes                                     |
