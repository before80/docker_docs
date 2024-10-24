+++
title = "docker plugin push"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/plugin/push/](https://docs.docker.com/reference/cli/docker/plugin/push/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker plugin push

| Description | Push a plugin to a registry                 |
| :---------- | ------------------------------------------- |
| Usage       | `docker plugin push [OPTIONS] PLUGIN[:TAG]` |

## Description

After you have created a plugin using `docker plugin create` and the plugin is ready for distribution, use `docker plugin push` to share your images to Docker Hub or a self-hosted registry.

Registry credentials are managed by [docker login]({{< ref "/reference/CLIreference/docker/dockerlogin" >}}).

## Options

| Option                    | Default | Description        |
| ------------------------- | ------- | ------------------ |
| `--disable-content-trust` | `true`  | Skip image signing |

## Examples

The following example shows how to push a sample `user/plugin`.



```console
$ docker plugin ls

ID             NAME                    DESCRIPTION                  ENABLED
69553ca1d456   user/plugin:latest      A sample plugin for Docker   false

$ docker plugin push user/plugin
```
