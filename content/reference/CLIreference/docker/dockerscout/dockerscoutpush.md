+++
title = "docker scout push"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/scout/push/](https://docs.docker.com/reference/cli/docker/scout/push/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker scout push

| Description | Push an image or image index to Docker Scout |
| :---------- | -------------------------------------------- |
| Usage       | `docker scout push IMAGE`                    |

## Description

The `docker scout push` command lets you push an image or analysis result to Docker Scout.

## Options

| Option         | Default | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| `--author`     |         | Name of the author of the image                              |
| `--dry-run`    |         | Do not push the image but process it                         |
| `--org`        |         | Namespace of the Docker organization to which image will be pushed |
| `-o, --output` |         | Write the report to a file                                   |
| `--platform`   |         | Platform of image to be pushed                               |
| `--sbom`       |         | Create and upload SBOMs                                      |
| `--timestamp`  |         | Timestamp of image or tag creation                           |

## Examples

### Push an image to Docker Scout



```console
$ docker scout push --org my-org registry.example.com/repo:tag
```
