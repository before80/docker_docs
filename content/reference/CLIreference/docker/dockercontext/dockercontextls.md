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

## Options

| Option        | Default | Description                                                  |
| ------------- | ------- | ------------------------------------------------------------ |
| `--format`    |         | Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-q, --quiet` |         | Only show context names                                      |

## Examples

Use `docker context ls` to print all contexts. The currently active context is indicated with an `*`:



```console
$ docker context ls

NAME                DESCRIPTION                               DOCKER ENDPOINT                      ORCHESTRATOR
default *           Current DOCKER_HOST based configuration   unix:///var/run/docker.sock          swarm
production                                                    tcp:///prod.corp.example.com:2376
staging                                                       tcp:///stage.corp.example.com:2376
```

