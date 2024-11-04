+++
title = "docker compose exec"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/exec/](https://docs.docker.com/reference/cli/docker/compose/exec/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose exec

| Description | Execute a command in a running container                  |
| :---------- | --------------------------------------------------------- |
| Usage       | `docker compose exec [OPTIONS] SERVICE COMMAND [ARGS...]` |

## Description

This is the equivalent of `docker exec` targeting a Compose service.

​	这相当于针对 Compose 服务的 `docker exec`。

With this subcommand, you can run arbitrary commands in your services. Commands allocate a TTY by default, so you can use a command such as `docker compose exec web sh` to get an interactive prompt.

​	通过此子命令，您可以在服务中运行任意命令。命令默认分配 TTY，因此可以使用类似 `docker compose exec web sh` 的命令获取交互式提示符。

## Options

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `-d, --detach`  |         | 分离模式：在后台运行命令 Detached mode: Run command in the background |
| `-e, --env`     |         | 设置环境变量 Set environment variables                       |
| `--index`       |         | 如果服务有多个副本，则为容器的索引 Index of the container if service has multiple replicas |
| `-T, --no-TTY`  | `true`  | 禁用伪 TTY 分配。默认情况下，`docker compose exec` 会分配 TTY。 Disable pseudo-TTY allocation. By default `docker compose exec` allocates a TTY. |
| `--privileged`  |         | 授予进程扩展权限 Give extended privileges to the process     |
| `-u, --user`    |         | 以该用户运行命令 Run the command as this user                |
| `-w, --workdir` |         | 该命令的工作目录路径 Path to workdir directory for this command |
