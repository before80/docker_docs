+++
title = "docker checkpoint create"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/checkpoint/create/](https://docs.docker.com/reference/cli/docker/checkpoint/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker checkpoint create

| Description | Create a checkpoint from a running container              |
| :---------- | --------------------------------------------------------- |
| Usage       | `docker checkpoint create [OPTIONS] CONTAINER CHECKPOINT` |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

Create a checkpoint from a running container

## Options

| Option             | Default | Description                                  |
| ------------------ | ------- | -------------------------------------------- |
| `--checkpoint-dir` |         | Use a custom checkpoint storage directory    |
| `--leave-running`  |         | Leave the container running after checkpoint |
