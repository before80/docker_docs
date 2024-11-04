+++
title = "docker checkpoint ls"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/checkpoint/ls/](https://docs.docker.com/reference/cli/docker/checkpoint/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker checkpoint ls

| Description | List checkpoints for a container           |
| :---------- | ------------------------------------------ |
| Usage       | `docker checkpoint ls [OPTIONS] CONTAINER` |
| Aliases     | `docker checkpoint list`                   |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

List checkpoints for a container

​	列出容器的检查点

## Options

| Option             | Default | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| `--checkpoint-dir` |         | 使用自定义检查点存储目录 Use a custom checkpoint storage directory |
