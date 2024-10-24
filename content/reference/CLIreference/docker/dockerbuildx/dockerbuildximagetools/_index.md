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

## Examples

### Override the configured builder instance (--builder)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

## Subcommands

| Command                                                      | Description                               |
| :----------------------------------------------------------- | :---------------------------------------- |
| [`docker buildx imagetools create`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildximagetools/dockerbuildximagetoolscreate" >}}) | Create a new image based on source images |
| [`docker buildx imagetools inspect`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildximagetools/dockerbuildximagetoolsinspect" >}}) | Show details of an image in the registry  |
