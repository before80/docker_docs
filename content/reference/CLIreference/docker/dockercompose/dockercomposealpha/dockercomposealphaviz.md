+++
title = "docker compose alpha viz"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/alpha/viz/](https://docs.docker.com/reference/cli/docker/compose/alpha/viz/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose alpha viz

| Description | EXPERIMENTAL - Generate a graphviz graph from your compose file |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker compose alpha viz [OPTIONS]`                         |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

EXPERIMENTAL - Generate a graphviz graph from your compose file

## Options

| Option               | Default | Description                                                  |
| -------------------- | ------- | ------------------------------------------------------------ |
| `--image`            |         | Include service's image name in output graph                 |
| `--indentation-size` | `1`     | Number of tabs or spaces to use for indentation              |
| `--networks`         |         | Include service's attached networks in output graph          |
| `--ports`            |         | Include service's exposed ports in output graph              |
| `--spaces`           |         | If given, space character ' ' will be used to indent, otherwise tab character '\t' will be used |
