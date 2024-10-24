+++
title = "docker volume update"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/volume/update/](https://docs.docker.com/reference/cli/docker/volume/update/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker volume update

| Description | Update a volume (cluster volumes only)    |
| :---------- | ----------------------------------------- |
| Usage       | `docker volume update [OPTIONS] [VOLUME]` |

Swarm This command works with the Swarm orchestrator.

## Description

Update a volume (cluster volumes only)

## Options

| Option           | Default  | Description                                                  |
| ---------------- | -------- | ------------------------------------------------------------ |
| `--availability` | `active` | API 1.42+ Swarm Cluster Volume availability (`active`, `pause`, `drain`) |
