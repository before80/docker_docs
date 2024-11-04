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

​	重新启动所有已停止和正在运行的服务，或仅重新启动指定的服务。

If you make changes to your `compose.yml` configuration, these changes are not reflected after running this command. For example, changes to environment variables (which are added after a container is built, but before the container's command is executed) are not updated after restarting.

​	如果你对 `compose.yml` 配置进行了更改，这些更改在运行此命令后不会反映。例如，对环境变量的更改（在容器构建后但在执行容器命令之前添加）在重启后不会更新。

If you are looking to configure a service's restart policy, refer to [restart](https://github.com/compose-spec/compose-spec/blob/master/spec.md#restart) or [restart_policy](https://github.com/compose-spec/compose-spec/blob/master/deploy.md#restart_policy).

​	如果你希望配置服务的重启策略，请参考 [restart](https://github.com/compose-spec/compose-spec/blob/master/spec.md#restart) 或 [restart_policy](https://github.com/compose-spec/compose-spec/blob/master/deploy.md#restart_policy)。

## Options

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `--no-deps`     |         | 不重新启动依赖的服务 Don't restart dependent services        |
| `-t, --timeout` |         | 指定关闭超时时间（秒） Specify a shutdown timeout in seconds |
