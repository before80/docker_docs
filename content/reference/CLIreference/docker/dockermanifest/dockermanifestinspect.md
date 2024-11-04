+++
title = "docker manifest inspect"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/manifest/inspect/](https://docs.docker.com/reference/cli/docker/manifest/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker manifest inspect

| Description | Display an image manifest, or manifest list                  |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker manifest inspect [OPTIONS] [MANIFEST_LIST] MANIFEST` |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

Display an image manifest, or manifest list

​	显示镜像清单或清单列表

## Options

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `--insecure`    |         | 允许与不安全的注册表通信 Allow communication with an insecure registry |
| `-v, --verbose` |         | 输出额外信息，包括层信息和平台 Output additional info including layers and platform |
