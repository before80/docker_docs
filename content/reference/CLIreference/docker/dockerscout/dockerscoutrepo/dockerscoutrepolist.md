+++
title = "docker scout repo list"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/scout/repo/list/](https://docs.docker.com/reference/cli/docker/scout/repo/list/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker scout repo list

| Description | List Docker Scout repositories |
| :---------- | ------------------------------ |
| Usage       | `docker scout repo list`       |

## Description

The docker scout repo list command shows all repositories in an organization.

If ORG is not provided the default configured organization will be used.

## Options

| Option            | Default | Description                                                  |
| ----------------- | ------- | ------------------------------------------------------------ |
| `--filter`        |         | Regular expression to filter repositories by name            |
| `--only-disabled` |         | Filter to disabled repositories only                         |
| `--only-enabled`  |         | Filter to enabled repositories only                          |
| `--only-registry` |         | Filter to a specific registry only: - hub.docker.com - ecr (AWS ECR) |
| `--org`           |         | Namespace of the Docker organization                         |
