+++
title = "docker compose alpha publish"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/alpha/publish/](https://docs.docker.com/reference/cli/docker/compose/alpha/publish/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose alpha publish

| Description | Publish compose application                           |
| :---------- | ----------------------------------------------------- |
| Usage       | `docker compose alpha publish [OPTIONS] [REPOSITORY]` |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

Publish compose application

​	发布 Compose 应用

## Options

| Option                    | Default | Description                                                  |
| ------------------------- | ------- | ------------------------------------------------------------ |
| `--oci-version`           |         | OCI 镜像/工件规范版本（默认自动确定） OCI Image/Artifact specification version (automatically determined by default) |
| `--resolve-image-digests` |         | 将镜像标签固定为摘要 Pin image tags to digests               |
