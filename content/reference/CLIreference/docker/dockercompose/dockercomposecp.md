+++
title = "docker compose cp"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/cp/](https://docs.docker.com/reference/cli/docker/compose/cp/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose cp

| Description | Copy files/folders between a service container and the local filesystem |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker compose cp [OPTIONS] SERVICE:SRC_PATH DEST_PATH|- docker compose cp [OPTIONS] SRC_PATH|- SERVICE:DEST_PATH` |

## Description

Copy files/folders between a service container and the local filesystem

​	在服务容器与本地文件系统之间复制文件/文件夹

## Options

| Option              | Default | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| `-a, --archive`     |         | 归档模式（复制所有 uid/gid 信息） Archive mode (copy all uid/gid information) |
| `-L, --follow-link` |         | 始终跟随 SRC_PATH 中的符号链接 Always follow symbol link in SRC_PATH |
| `--index`           |         | 如果服务有多个副本，则为容器的索引 Index of the container if service has multiple replicas |
