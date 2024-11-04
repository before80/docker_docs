+++
title = "docker context import"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/context/import/](https://docs.docker.com/reference/cli/docker/context/import/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker context import

| Description | Import a context from a tar or zip file |
| :---------- | --------------------------------------- |
| Usage       | `docker context import CONTEXT FILE|-`  |

## Description

Imports a context previously exported with `docker context export`. To import from stdin, use a hyphen (`-`) as filename.

​	从之前使用 `docker context export` 导出的上下文导入。如果要从标准输入导入，请使用连字符 (`-`) 作为文件名。
