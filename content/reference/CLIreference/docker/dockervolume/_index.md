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

## Subcommands

| Command                                                      | Description                                         |
| :----------------------------------------------------------- | :-------------------------------------------------- |
| [`docker volume create`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumecreate" >}}) | Create a volume                                     |
| [`docker volume inspect`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumeinspect" >}}) | Display detailed information on one or more volumes |
| [`docker volume ls`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumels" >}}) | List volumes                                        |
| [`docker volume prune`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumeprune" >}}) | Remove unused local volumes                         |
| [`docker volume rm`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumerm" >}}) | Remove one or more volumes                          |
| [`docker volume update`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumeupdate" >}}) | Update a volume (cluster volumes only)              |
