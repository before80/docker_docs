+++
title = "docker context ls"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/context/ls/](https://docs.docker.com/reference/cli/docker/context/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker context ls

| Description | List contexts                 |
| :---------- | ----------------------------- |
| Usage       | `docker context ls [OPTIONS]` |
| Aliases     | `docker context list`         |

## Description

List contexts

​	列出上下文

## Options

| Option        | Default | Description                                                  |
| ------------- | ------- | ------------------------------------------------------------ |
| `--format`    |         | 使用自定义模板格式化输出：'table'：以带有列标题的表格格式打印输出（默认）；'table TEMPLATE'：使用指定的 Go 模板以表格格式打印输出；'json'：以 JSON 格式打印输出；'TEMPLATE'：使用指定的 Go 模板格式化输出。有关使用模板格式化输出的详细信息，请参阅 https://docs.docker.com/go/formatting/。  Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-q, --quiet` |         | 仅显示上下文名称 Only show context names                     |

## Examples

Use `docker context ls` to print all contexts. The currently active context is indicated with an `*`:

​	使用 `docker context ls` 打印所有上下文。当前活动的上下文用 `*` 表示。



```console
$ docker context ls

NAME                DESCRIPTION                               DOCKER ENDPOINT                      ORCHESTRATOR
default *           Current DOCKER_HOST based configuration   unix:///var/run/docker.sock          swarm
production                                                    tcp:///prod.corp.example.com:2376
staging                                                       tcp:///stage.corp.example.com:2376
```

