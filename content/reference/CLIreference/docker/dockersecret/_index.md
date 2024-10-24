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

## Description

Manage secrets.

## Subcommands

| Command                                                      | Description                                         |
| :----------------------------------------------------------- | :-------------------------------------------------- |
| [`docker secret create`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretcreate" >}}) | Create a secret from a file or STDIN as content     |
| [`docker secret inspect`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretinspect" >}}) | Display detailed information on one or more secrets |
| [`docker secret ls`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretls" >}}) | List secrets                                        |
| [`docker secret rm`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretrm" >}}) | Remove one or more secrets                          |
