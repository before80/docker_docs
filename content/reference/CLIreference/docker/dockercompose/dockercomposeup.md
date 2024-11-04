+++
title = "docker compose up"
date = 2024-10-23T14:54:43+08:00
weight = 240
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/up/](https://docs.docker.com/reference/cli/docker/compose/up/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose up

| Description | Create and start containers                |
| :---------- | ------------------------------------------ |
| Usage       | `docker compose up [OPTIONS] [SERVICE...]` |

## Description

Builds, (re)creates, starts, and attaches to containers for a service.

​	构建、（重新）创建、启动并附加到服务的容器。

Unless they are already running, this command also starts any linked services.

​	除非它们已经在运行，否则此命令还会启动任何链接的服务。

The `docker compose up` command aggregates the output of each container (like `docker compose logs --follow` does). One can optionally select a subset of services to attach to using `--attach` flag, or exclude some services using `--no-attach` to prevent output to be flooded by some verbose services.

​	`docker compose up` 命令聚合每个容器的输出（类似于 `docker compose logs --follow`）。你可以选择使用 `--attach` 标志附加到某些服务的输出，或使用 `--no-attach` 排除某些服务，以防输出被某些冗长的服务淹没。

When the command exits, all containers are stopped. Running `docker compose up --detach` starts the containers in the background and leaves them running.

​	当命令退出时，所有容器都会停止。运行 `docker compose up --detach` 会在后台启动容器并使其保持运行。

If there are existing containers for a service, and the service’s configuration or image was changed after the container’s creation, `docker compose up` picks up the changes by stopping and recreating the containers (preserving mounted volumes). To prevent Compose from picking up changes, use the `--no-recreate` flag.

​	如果服务有现有容器，并且服务的配置或镜像在容器创建后发生了更改，`docker compose up` 会通过停止和重新创建容器（保留挂载的卷）来应用这些更改。要防止 Compose 应用更改，请使用 `--no-recreate` 标志。

If you want to force Compose to stop and recreate all containers, use the `--force-recreate` flag.

​	如果你希望强制 Compose 停止并重新创建所有容器，请使用 `--force-recreate` 标志。

If the process encounters an error, the exit code for this command is `1`. If the process is interrupted using `SIGINT` (ctrl + C) or `SIGTERM`, the containers are stopped, and the exit code is `0`.

​	如果过程遇到错误，该命令的退出代码为 `1`。如果使用 `SIGINT`（ctrl + C）或 `SIGTERM` 中断过程，则容器会停止，退出代码为 `0`。

## Options

| Option                         | Default  | Description                                                  |
| ------------------------------ | -------- | ------------------------------------------------------------ |
| `--abort-on-container-exit`    |          | 如果任何容器停止，则停止所有容器。与 -d 不兼容 Stops all containers if any container was stopped. Incompatible with -d |
| `--abort-on-container-failure` |          | 如果任何容器以失败状态退出，则停止所有容器。与 -d 不兼容 Stops all containers if any container exited with failure. Incompatible with -d |
| `--always-recreate-deps`       |          | 重新创建依赖容器。与 --no-recreate 不兼容 Recreate dependent containers. Incompatible with --no-recreate. |
| `--attach`                     |          | 限制附加到指定服务的输出。与 --attach-dependencies 不兼容 Restrict attaching to the specified services. Incompatible with --attach-dependencies. |
| `--attach-dependencies`        |          | 自动附加到依赖服务的日志输出 Automatically attach to log output of dependent services |
| `--build`                      |          | 启动容器之前构建镜像 Build images before starting containers |
| `-d, --detach`                 |          | 分离模式：在后台运行容器 Detached mode: Run containers in the background |
| `--exit-code-from`             |          | 返回所选服务容器的退出代码。隐含 `--abort-on-container-exit`   - Return the exit code of the selected service container. Implies `--abort-on-container-exit` |
| `--force-recreate`             |          | 即使它们的配置和镜像没有更改也重新创建容器 Recreate containers even if their configuration and image haven't changed |
| `--menu`                       |          | 在运行附加时启用交互式快捷方式。与 --detach 不兼容。可以通过设置 COMPOSE_MENU 环境变量来启用/禁用 Enable interactive shortcuts when running attached. Incompatible with --detach. Can also be enable/disable by setting COMPOSE_MENU environment var. |
| `--no-attach`                  |          | 不附加（流式日志）到指定服务 Do not attach (stream logs) to the specified services |
| `--no-build`                   |          | 即使是政策也不构建镜像 Don't build an image, even if it's policy |
| `--no-color`                   |          | 生成单色输出 Produce monochrome output                       |
| `--no-deps`                    |          | 不启动链接的服务 Don't start linked services                 |
| `--no-log-prefix`              |          | 不在日志中打印前缀 Don't print prefix in logs                |
| `--no-recreate`                |          | 如果容器已经存在，则不重新创建它们。与 `--force-recreate` 不兼容 If containers already exist, don't recreate them. Incompatible with --force-recreate. |
| `--no-start`                   |          | 创建后不启动服务 Don't start the services after creating them |
| `--pull`                       | `policy` | 运行前拉取镜像("always"\|"missing"\|"never")  Pull image before running ("always"\|"missing"\|"never") |
| `--quiet-pull`                 |          | 无需打印进度信息进行拉取 Pull without printing progress information |
| `--remove-orphans`             |          | 删除在 Compose 文件中未定义的服务的容器 Remove containers for services not defined in the Compose file |
| `-V, --renew-anon-volumes`     |          | 重新创建匿名卷，而不是从以前的容器中检索数据 Recreate anonymous volumes instead of retrieving data from the previous containers |
| `--scale`                      |          | 将服务缩放到指定实例数。如果在 Compose 文件中存在，则覆盖 `scale` 设置。 Scale SERVICE to NUM instances. Overrides the `scale` setting in the Compose file if present. |
| `-t, --timeout`                |          | 当附加或容器已经在运行时，使用此超时时间（秒） Use this timeout in seconds for container shutdown when attached or when containers are already running |
| `--timestamps`                 |          | 显示时间戳 Show timestamps                                   |
| `--wait`                       |          | 等待服务运行或健康。隐含分离模式。 Wait for services to be running\|healthy. Implies detached mode. |
| `--wait-timeout`               |          | 等待项目运行或健康的最长持续时间 Maximum duration to wait for the project to be running\|healthy |
| `-w, --watch`                  |          | 监视源代码，并在文件更新时重新构建/刷新容器 Watch source code and rebuild/refresh containers when files are updated. |
