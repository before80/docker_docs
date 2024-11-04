+++
title = "docker config"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/config/](https://docs.docker.com/reference/cli/docker/config/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker config

| Description | Manage Swarm configs |
| :---------- | -------------------- |
| Usage       | `docker config`      |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令与 Swarm 调度器配合使用。

## Description

Manage configs.

​	管理配置。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker config create`]({{< ref "/reference/CLIreference/docker/dockerconfig/dockerconfigcreate" >}}) | 从文件或标准输入创建配置 Create a config from a file or STDIN |
| [`docker config inspect`]({{< ref "/reference/CLIreference/docker/dockerconfig/dockerconfiginspect" >}}) | 显示一个或多个配置的详细信息 Display detailed information on one or more configs |
| [`docker config ls`]({{< ref "/reference/CLIreference/docker/dockerconfig/dockerconfigls" >}}) | 列出配置 List configs                                        |
| [`docker config rm`]({{< ref "/reference/CLIreference/docker/dockerconfig/dockerconfigrm" >}}) | 删除一个或多个配置 Remove one or more configs                |
