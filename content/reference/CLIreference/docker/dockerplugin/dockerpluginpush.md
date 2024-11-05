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

​	在使用 `docker plugin create` 创建插件并准备好分发后，使用 `docker plugin push` 将镜像共享到 Docker Hub 或自托管注册表。

Registry credentials are managed by [docker login]({{< ref "/reference/CLIreference/docker/dockerlogin" >}}).

​	注册表凭证由 [docker login]({{< ref "/reference/CLIreference/docker/dockerlogin" >}}) 管理。

## Options

| Option                    | Default | Description                     |
| ------------------------- | ------- | ------------------------------- |
| `--disable-content-trust` | `true`  | 跳过镜像签名 Skip image signing |

## Examples

The following example shows how to push a sample `user/plugin`.

​	以下示例展示如何推送一个示例 `user/plugin`。



```console
$ docker plugin ls

ID             NAME                    DESCRIPTION                  ENABLED
69553ca1d456   user/plugin:latest      A sample plugin for Docker   false

$ docker plugin push user/plugin
```
