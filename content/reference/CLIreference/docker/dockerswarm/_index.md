+++
title = "docker swarm"
date = 2024-10-23T14:54:43+08:00
weight = 230
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/swarm/](https://docs.docker.com/reference/cli/docker/swarm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker swarm

| Description | Manage Swarm   |
| :---------- | -------------- |
| Usage       | `docker swarm` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 协调器。

## Description

Manage the swarm.

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker swarm ca`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmca" >}}) | 显示和轮换根 CA 证书 Display and rotate the root CA          |
| [`docker swarm init`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarminit" >}}) | 初始化 Swarm -Initialize a swarm                             |
| [`docker swarm join`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin" >}}) | 作为节点或管理器加入 Swarm  Join a swarm as a node and/or manager |
| [`docker swarm join-token`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin-token" >}}) | 管理加入令牌  Manage join tokens                             |
| [`docker swarm leave`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmleave" >}}) | 离开 Swarm  - Leave the swarm                                |
| [`docker swarm unlock`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmunlock" >}}) | 解锁 Swarm  - Unlock swarm                                   |
| [`docker swarm unlock-key`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmunlock-key" >}}) | 管理解锁密钥 Manage the unlock key                           |
| [`docker swarm update`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmupdate" >}}) | 更新 Swarm  - Update the swarm                               |

