+++
title = "docker trust"
date = 2024-10-23T14:54:43+08:00
weight = 250
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/trust/](https://docs.docker.com/reference/cli/docker/trust/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker trust

| Description | Manage trust on Docker images |
| :---------- | ----------------------------- |
| Usage       | `docker trust`                |

## Description

Manage trust on Docker images

​	管理 Docker 镜像的信任

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker trust inspect`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustinspect" >}}) | 返回关于密钥和签名的低级信息 Return low-level information about keys and signatures |
| [`docker trust key`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustkey" >}}) | 管理 Docker 镜像的签名密钥 Manage keys for signing Docker images |
| [`docker trust revoke`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustrevoke" >}}) | 移除镜像的信任 Remove trust for an image                     |
| [`docker trust sign`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustsign" >}}) | 对镜像进行签名 Sign an image                                 |
| [`docker trust signer`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustsigner" >}}) | 管理可以签署 Docker 镜像的实体 Manage entities who can sign Docker images |
