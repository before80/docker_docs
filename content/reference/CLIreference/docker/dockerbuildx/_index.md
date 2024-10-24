+++
title = "docker buildx"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/buildx/](https://docs.docker.com/reference/cli/docker/buildx/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx

| Description | Docker Buildx |
| :---------- | ------------- |
|             |               |

## [Description](https://docs.docker.com/reference/cli/docker/buildx/#description)

Extended build capabilities with BuildKit

## [Options](https://docs.docker.com/reference/cli/docker/buildx/#options)

| Option                                                       | Default | Description                              |
| ------------------------------------------------------------ | ------- | ---------------------------------------- |
| [`--builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) |         | Override the configured builder instance |
| `-D, --debug`                                                |         | Enable debug logging                     |

## [Examples](https://docs.docker.com/reference/cli/docker/buildx/#examples)

### [Override the configured builder instance (--builder)](https://docs.docker.com/reference/cli/docker/buildx/#builder)

You can also use the `BUILDX_BUILDER` environment variable.

## [Subcommands](https://docs.docker.com/reference/cli/docker/buildx/#subcommands)

| Command                                                      | Description                            |
| :----------------------------------------------------------- | :------------------------------------- |
| [`docker buildx bake`](https://docs.docker.com/reference/cli/docker/buildx/bake/) | Build from a file                      |
| [`docker buildx build`](https://docs.docker.com/reference/cli/docker/buildx/build/) | Start a build                          |
| [`docker buildx create`](https://docs.docker.com/reference/cli/docker/buildx/create/) | Create a new builder instance          |
| [`docker buildx debug`](https://docs.docker.com/reference/cli/docker/buildx/debug/) | Start debugger                         |
| [`docker buildx du`](https://docs.docker.com/reference/cli/docker/buildx/du/) | Disk usage                             |
| [`docker buildx imagetools`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/) | Commands to work on images in registry |
| [`docker buildx inspect`](https://docs.docker.com/reference/cli/docker/buildx/inspect/) | Inspect current builder instance       |
| [`docker buildx ls`](https://docs.docker.com/reference/cli/docker/buildx/ls/) | List builder instances                 |
| [`docker buildx prune`](https://docs.docker.com/reference/cli/docker/buildx/prune/) | Remove build cache                     |
| [`docker buildx rm`](https://docs.docker.com/reference/cli/docker/buildx/rm/) | Remove one or more builder instances   |
| [`docker buildx stop`](https://docs.docker.com/reference/cli/docker/buildx/stop/) | Stop builder instance                  |
| [`docker buildx use`](https://docs.docker.com/reference/cli/docker/buildx/use/) | Set the current builder instance       |
| [`docker buildx version`](https://docs.docker.com/reference/cli/docker/buildx/version/) | Show buildx version information        |
