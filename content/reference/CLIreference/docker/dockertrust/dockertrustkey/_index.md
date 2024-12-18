+++
title = "docker trust key"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/trust/key/](https://docs.docker.com/reference/cli/docker/trust/key/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker trust key

| Description | Manage keys for signing Docker images |
| :---------- | ------------------------------------- |
| Usage       | `docker trust key`                    |

## Description

Manage keys for signing Docker images

​	管理签署 Docker 镜像的密钥

## Subcommands

| Command                                                      | Description                                                |
| :----------------------------------------------------------- | :--------------------------------------------------------- |
| [`docker trust key generate`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustkey/dockertrustkeygenerate" >}}) | 生成并加载签名密钥对Generate and load a signing key-pair   |
| [`docker trust key load`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustkey/dockertrustkeyload" >}}) | 加载用于签名的私钥文件 Load a private key file for signing |
