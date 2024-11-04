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

​	实验性 - 从您的 Compose 文件生成 Graphviz 图

## Options

| Option               | Default | Description                                                  |
| -------------------- | ------- | ------------------------------------------------------------ |
| `--image`            |         | 在输出图中包含服务的镜像名称 Include service's image name in output graph |
| `--indentation-size` | `1`     | 用于缩进的制表符或空格的数量 Number of tabs or spaces to use for indentation |
| `--networks`         |         | 在输出图中包含服务的附加网络 Include service's attached networks in output graph |
| `--ports`            |         | 在输出图中包含服务的暴露端口 Include service's exposed ports in output graph |
| `--spaces`           |         | 如果给定，将使用空格字符 ' ' 来缩进，否则将使用制表符字符 '\t' If given, space character ' ' will be used to indent, otherwise tab character '\t' will be used |
