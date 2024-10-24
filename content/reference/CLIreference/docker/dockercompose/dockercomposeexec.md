+++
title = "docker compose exec"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/exec/](https://docs.docker.com/reference/cli/docker/compose/exec/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose exec

| Description | Execute a command in a running container                  |
| :---------- | --------------------------------------------------------- |
| Usage       | `docker compose exec [OPTIONS] SERVICE COMMAND [ARGS...]` |

## [Description](https://docs.docker.com/reference/cli/docker/compose/exec/#description)

This is the equivalent of `docker exec` targeting a Compose service.

With this subcommand, you can run arbitrary commands in your services. Commands allocate a TTY by default, so you can use a command such as `docker compose exec web sh` to get an interactive prompt.

## [Options](https://docs.docker.com/reference/cli/docker/compose/exec/#options)

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `-d, --detach`  |         | Detached mode: Run command in the background                 |
| `-e, --env`     |         | Set environment variables                                    |
| `--index`       |         | Index of the container if service has multiple replicas      |
| `-T, --no-TTY`  | `true`  | Disable pseudo-TTY allocation. By default `docker compose exec` allocates a TTY. |
| `--privileged`  |         | Give extended privileges to the process                      |
| `-u, --user`    |         | Run the command as this user                                 |
| `-w, --workdir` |         | Path to workdir directory for this command                   |
