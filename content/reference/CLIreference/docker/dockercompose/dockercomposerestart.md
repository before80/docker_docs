+++
title = "docker compose restart"
date = 2024-10-23T14:54:43+08:00
weight = 170
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/restart/](https://docs.docker.com/reference/cli/docker/compose/restart/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose restart

| Description | Restart service containers                      |
| :---------- | ----------------------------------------------- |
| Usage       | `docker compose restart [OPTIONS] [SERVICE...]` |

## Description

Restarts all stopped and running services, or the specified services only.

If you make changes to your `compose.yml` configuration, these changes are not reflected after running this command. For example, changes to environment variables (which are added after a container is built, but before the container's command is executed) are not updated after restarting.

If you are looking to configure a service's restart policy, refer to [restart](https://github.com/compose-spec/compose-spec/blob/master/spec.md#restart) or [restart_policy](https://github.com/compose-spec/compose-spec/blob/master/deploy.md#restart_policy).

## Options

| Option          | Default | Description                           |
| --------------- | ------- | ------------------------------------- |
| `--no-deps`     |         | Don't restart dependent services      |
| `-t, --timeout` |         | Specify a shutdown timeout in seconds |
