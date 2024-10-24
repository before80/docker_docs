+++
title = "docker scout stream"
date = 2024-10-23T14:54:43+08:00
weight = 140
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/scout/stream/](https://docs.docker.com/reference/cli/docker/scout/stream/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker scout stream

| Description | Manage streams (experimental)          |
| :---------- | -------------------------------------- |
| Usage       | `docker scout stream [STREAM] [IMAGE]` |

> **Warning**
>
> This command is deprecated
>
> It may be removed in a future Docker version. For more information, see the [Docker roadmap](https://github.com/docker/roadmap/issues/209)

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

The `docker scout stream` command lists the deployment streams and records an image to it.

Once recorded, streams can be referred to by their name, eg. in the `docker scout compare` command using `--to-stream`.

## Options

| Option         | Default | Description                          |
| -------------- | ------- | ------------------------------------ |
| `--org`        |         | Namespace of the Docker organization |
| `-o, --output` |         | Write the report to a file           |
| `--platform`   |         | Platform of image to record          |

## Examples

### List existing streams



```console
$ %[1]s %[2]s
prod-cluster-123
stage-cluster-234
```

### List images of a stream



```console
$ %[1]s %[2]s prod-cluster-123
namespace/repo:tag@sha256:9a4df4fadc9bbd44c345e473e0688c2066a6583d4741679494ba9228cfd93e1b
namespace/other-repo:tag@sha256:0001d6ce124855b0a158569c584162097fe0ca8d72519067c2c8e3ce407c580f
```

### Record an image to a stream, for a specific platform



```console
$ %[1]s %[2]s stage-cluster-234 namespace/repo:stage-latest --platform linux/amd64
✓ Pulled
✓ Successfully recorded namespace/repo:stage-latest in stream stage-cluster-234
```
