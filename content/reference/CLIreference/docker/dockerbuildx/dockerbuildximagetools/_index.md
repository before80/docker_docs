+++
title = "docker buildx imagetools"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/imagetools/](https://docs.docker.com/reference/cli/docker/buildx/imagetools/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx imagetools

| Description | Commands to work on images in registry |
| :---------- | -------------------------------------- |
|             |                                        |

## Description

The `imagetools` commands contains subcommands for working with manifest lists in container registries. These commands are useful for inspecting manifests to check multi-platform configuration and attestations.

​	`imagetools` 命令包含用于处理容器注册表中清单列表的子命令。这些命令对于检查多平台配置和认证信息非常有用。

## Examples

### 重写配置的构建器实例 (`--builder`) Override the configured builder instance (--builder)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

​	与 [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) 相同。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker buildx imagetools create`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildximagetools/dockerbuildximagetoolscreate" >}}) | 基于源镜像创建新镜像 Create a new image based on source images |
| [`docker buildx imagetools inspect`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildximagetools/dockerbuildximagetoolsinspect" >}}) | 显示注册表中镜像的详细信息 Show details of an image in the registry |
