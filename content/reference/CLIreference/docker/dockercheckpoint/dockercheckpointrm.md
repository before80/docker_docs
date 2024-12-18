+++
title = "docker checkpoint rm"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/checkpoint/rm/](https://docs.docker.com/reference/cli/docker/checkpoint/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker checkpoint rm

| Description | Remove a checkpoint                                   |
| :---------- | ----------------------------------------------------- |
| Usage       | `docker checkpoint rm [OPTIONS] CONTAINER CHECKPOINT` |
| Aliases     | `docker checkpoint remove`                            |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

Remove a checkpoint

​	删除一个检查点

## Options

| Option             | Default | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| `--checkpoint-dir` |         | 使用自定义检查点存储目录 Use a custom checkpoint storage directory |
