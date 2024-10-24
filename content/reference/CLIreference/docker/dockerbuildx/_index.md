+++
title = "docker buildx"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/](https://docs.docker.com/reference/cli/docker/buildx/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx

| Description | Docker Buildx |
| :---------- | ------------- |
|             |               |

## Description

Extended build capabilities with BuildKit

## Options

| Option                                                       | Default | Description                              |
| ------------------------------------------------------------ | ------- | ---------------------------------------- |
| [`--builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) |         | Override the configured builder instance |
| `-D, --debug`                                                |         | Enable debug logging                     |

## Examples

### Override the configured builder instance (--builder)

You can also use the `BUILDX_BUILDER` environment variable.

## Subcommands

| Command                                                      | Description                            |
| :----------------------------------------------------------- | :------------------------------------- |
| [`docker buildx bake`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbake" >}}) | Build from a file                      |
| [`docker buildx build`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbuild" >}}) | Start a build                          |
| [`docker buildx create`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxcreate" >}}) | Create a new builder instance          |
| [`docker buildx debug`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxdebug" >}}) | Start debugger                         |
| [`docker buildx du`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxdu" >}}) | Disk usage                             |
| [`docker buildx imagetools`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildximagetools" >}}) | Commands to work on images in registry |
| [`docker buildx inspect`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxinspect" >}}) | Inspect current builder instance       |
| [`docker buildx ls`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxls" >}}) | List builder instances                 |
| [`docker buildx prune`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxprune" >}}) | Remove build cache                     |
| [`docker buildx rm`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxrm" >}}) | Remove one or more builder instances   |
| [`docker buildx stop`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxstop" >}}) | Stop builder instance                  |
| [`docker buildx use`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxuse" >}}) | Set the current builder instance       |
| [`docker buildx version`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxversion" >}}) | Show buildx version information        |
