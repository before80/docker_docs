+++
title = "docker buildx imagetools"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/buildx/imagetools/](https://docs.docker.com/reference/cli/docker/buildx/imagetools/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx imagetools

| Description | Commands to work on images in registry |
| :---------- | -------------------------------------- |
|             |                                        |

## [Description](https://docs.docker.com/reference/cli/docker/buildx/imagetools/#description)

The `imagetools` commands contains subcommands for working with manifest lists in container registries. These commands are useful for inspecting manifests to check multi-platform configuration and attestations.

## [Examples](https://docs.docker.com/reference/cli/docker/buildx/imagetools/#examples)

### [Override the configured builder instance (--builder)](https://docs.docker.com/reference/cli/docker/buildx/imagetools/#builder)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

## [Subcommands](https://docs.docker.com/reference/cli/docker/buildx/imagetools/#subcommands)

| Command                                                      | Description                               |
| :----------------------------------------------------------- | :---------------------------------------- |
| [`docker buildx imagetools create`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/) | Create a new image based on source images |
| [`docker buildx imagetools inspect`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/inspect/) | Show details of an image in the registry  |
