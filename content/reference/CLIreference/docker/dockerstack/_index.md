+++
title = "docker stack"
date = 2024-10-23T14:54:43+08:00
weight = 220
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/stack/](https://docs.docker.com/reference/cli/docker/stack/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker stack

| Description | Manage Swarm stacks      |
| :---------- | ------------------------ |
| Usage       | `docker stack [OPTIONS]` |

Swarm This command works with the Swarm orchestrator.

## Description

Manage stacks.

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker stack config`]({{< ref "/reference/CLIreference/docker/dockerstack/dockerstackconfig" >}}) | Outputs the final config file, after doing merges and interpolations |
| [`docker stack deploy`]({{< ref "/reference/CLIreference/docker/dockerstack/dockerstackdeploy" >}}) | Deploy a new stack or update an existing stack               |
| [`docker stack ls`]({{< ref "/reference/CLIreference/docker/dockerstack/dockerstackls" >}}) | List stacks                                                  |
| [`docker stack ps`]({{< ref "/reference/CLIreference/docker/dockerstack/dockerstackps" >}}) | List the tasks in the stack                                  |
| [`docker stack rm`]({{< ref "/reference/CLIreference/docker/dockerstack/dockerstackrm" >}}) | Remove one or more stacks                                    |
| [`docker stack services`]({{< ref "/reference/CLIreference/docker/dockerstack/dockerstackservices" >}}) | List the services in the stack                               |
