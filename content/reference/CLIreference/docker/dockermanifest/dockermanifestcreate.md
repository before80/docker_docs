+++
title = "docker manifest create"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/manifest/create/](https://docs.docker.com/reference/cli/docker/manifest/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker manifest create

| Description | Create a local manifest list for annotating and pushing to a registry |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker manifest create MANIFEST_LIST MANIFEST [MANIFEST...]` |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

Create a local manifest list for annotating and pushing to a registry

​	创建用于标注并推送到注册表的本地清单列表

## Options

| Option        | Default | Description                                                  |
| ------------- | ------- | ------------------------------------------------------------ |
| `-a, --amend` |         | 修改现有的清单列表 Amend an existing manifest list           |
| `--insecure`  |         | 允许与不安全的注册表通信 Allow communication with an insecure registry |
