+++
title = "docker secret"
date = 2024-10-23T14:54:43+08:00
weight = 200
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/secret/](https://docs.docker.com/reference/cli/docker/secret/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker secret

| Description | Manage Swarm secrets |
| :---------- | -------------------- |
| Usage       | `docker secret`      |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 编排器。

## Description

Manage secrets.

​	管理机密。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker secret create`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretcreate" >}}) | 从文件或标准输入中创建机密 Create a secret from a file or STDIN as content |
| [`docker secret inspect`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretinspect" >}}) | 显示一个或多个机密的详细信息 Display detailed information on one or more secrets |
| [`docker secret ls`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretls" >}}) | 列出机密 List secrets                                        |
| [`docker secret rm`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretrm" >}}) | 移除一个或多个机密 Remove one or more secrets                |
