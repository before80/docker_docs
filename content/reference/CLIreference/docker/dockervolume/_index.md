+++
title = "docker volume"
date = 2024-10-23T14:54:43+08:00
weight = 270
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/volume/](https://docs.docker.com/reference/cli/docker/volume/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker volume

| Description | Manage volumes          |
| :---------- | ----------------------- |
| Usage       | `docker volume COMMAND` |

## Description

Manage volumes. You can use subcommands to create, inspect, list, remove, or prune volumes.

​	管理卷。您可以使用子命令来创建、检查、列出、删除或清理卷。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker volume create`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumecreate" >}}) | 创建一个卷 Create a volume                                   |
| [`docker volume inspect`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumeinspect" >}}) | 显示一个或多个卷的详细信息Display detailed information on one or more volumes |
| [`docker volume ls`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumels" >}}) | 列出卷List volumes                                           |
| [`docker volume prune`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumeprune" >}}) | 删除未使用的本地卷Remove unused local volumes                |
| [`docker volume rm`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumerm" >}}) | 删除一个或多个卷Remove one or more volumes                   |
| [`docker volume update`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumeupdate" >}}) | 更新卷（仅适用于集群卷）Update a volume (cluster volumes only) |
