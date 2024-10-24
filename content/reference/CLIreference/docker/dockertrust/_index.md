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

## Subcommands

| Command                                                      | Description                                            |
| :----------------------------------------------------------- | :----------------------------------------------------- |
| [`docker trust inspect`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustinspect" >}}) | Return low-level information about keys and signatures |
| [`docker trust key`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustkey" >}}) | Manage keys for signing Docker images                  |
| [`docker trust revoke`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustrevoke" >}}) | Remove trust for an image                              |
| [`docker trust sign`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustsign" >}}) | Sign an image                                          |
| [`docker trust signer`]({{< ref "/reference/CLIreference/docker/dockertrust/dockertrustsigner" >}}) | Manage entities who can sign Docker images             |
