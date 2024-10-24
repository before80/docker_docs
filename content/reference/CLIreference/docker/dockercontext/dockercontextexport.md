+++
title = "docker context export"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/context/export/](https://docs.docker.com/reference/cli/docker/context/export/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker context export

| Description | Export a context to a tar archive FILE or a tar stream on STDOUT. |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker context export [OPTIONS] CONTEXT [FILE|-]`           |

## Description

Exports a context to a file that can then be used with `docker context import`.

The default output filename is `<CONTEXT>.dockercontext`. To export to `STDOUT`, use `-` as filename, for example:



```console
$ docker context export my-context -
```
