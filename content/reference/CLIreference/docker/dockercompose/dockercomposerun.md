+++
title = "docker compose run"
date = 2024-10-23T14:54:43+08:00
weight = 190
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/run/](https://docs.docker.com/reference/cli/docker/compose/run/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose run

| Description | Run a one-off command on a service                         |
| :---------- | ---------------------------------------------------------- |
| Usage       | `docker compose run [OPTIONS] SERVICE [COMMAND] [ARGS...]` |

## Description

Runs a one-time command against a service.

​	对服务运行一次性命令。

The following command starts the `web` service and runs `bash` as its command:

​	以下命令启动 `web` 服务并运行 `bash` 作为其命令：



```console
$ docker compose run web bash
```

Commands you use with run start in new containers with configuration defined by that of the service, including volumes, links, and other details. However, there are two important differences:

​	与 `run` 一起使用的命令会在新容器中启动，配置由服务定义，包括卷、链接和其他细节。然而，有两个重要的区别：

First, the command passed by `run` overrides the command defined in the service configuration. For example, if the `web` service configuration is started with `bash`, then `docker compose run web python app.py` overrides it with `python app.py`.

​	首先，`run` 传递的命令会覆盖服务配置中定义的命令。例如，如果 `web` 服务配置以 `bash` 启动，则 `docker compose run web python app.py` 会用 `python app.py` 覆盖它。

The second difference is that the `docker compose run` command does not create any of the ports specified in the service configuration. This prevents port collisions with already-open ports. If you do want the service’s ports to be created and mapped to the host, specify the `--service-ports`

​	第二个区别是 `docker compose run` 命令不会创建服务配置中指定的任何端口。这可以防止与已经开放的端口发生冲突。如果你希望服务的端口被创建并映射到主机，请指定 `--service-ports`：



```console
$ docker compose run --service-ports web python manage.py shell
```

Alternatively, manual port mapping can be specified with the `--publish` or `-p` options, just as when using docker run:

​	或者，可以使用 `--publish` 或 `-p` 选项手动指定端口映射，和使用 `docker run` 时一样：



```console
$ docker compose run --publish 8080:80 -p 2022:22 -p 127.0.0.1:2021:21 web python manage.py shell
```

If you start a service configured with links, the run command first checks to see if the linked service is running and starts the service if it is stopped. Once all the linked services are running, the run executes the command you passed it. For example, you could run:

​	如果你启动一个配置了链接的服务，`run` 命令会首先检查被链接的服务是否在运行，如果停止则启动该服务。一旦所有链接的服务都在运行，`run` 会执行你传递给它的命令。例如，你可以运行：



```console
$ docker compose run db psql -h db -U docker
```

This opens an interactive PostgreSQL shell for the linked `db` container.

​	这会为链接的 `db` 容器打开一个交互式 PostgreSQL shell。

If you do not want the run command to start linked containers, use the `--no-deps` flag:

​	如果你不希望 `run` 命令启动链接的容器，可以使用 `--no-deps` 标志：



```console
$ docker compose run --no-deps web python manage.py shell
```

If you want to remove the container after running while overriding the container’s restart policy, use the `--rm` flag:

​	如果你希望在运行后移除容器，同时覆盖容器的重启策略，可以使用 `--rm` 标志：



```console
$ docker compose run --rm web python manage.py db upgrade
```

This runs a database upgrade script, and removes the container when finished running, even if a restart policy is specified in the service configuration.

​	这会运行一个数据库升级脚本，并在运行完成后移除容器，即使服务配置中指定了重启策略。

## Options

| Option                | Default | Description                                                  |
| --------------------- | ------- | ------------------------------------------------------------ |
| `--build`             |         | 在启动容器之前构建镜像 Build image before starting container |
| `--cap-add`           |         | 添加 Linux 功能 Add Linux capabilities                       |
| `--cap-drop`          |         | 删除 Linux 功能 Drop Linux capabilities                      |
| `-d, --detach`        |         | 在后台运行容器并打印容器 ID Run container in background and print container ID |
| `--entrypoint`        |         | 覆盖镜像的入口点 Override the entrypoint of the image        |
| `-e, --env`           |         | 设置环境变量 Set environment variables                       |
| `-i, --interactive`   | `true`  | 即使未连接也保持 STDIN 打开 Keep STDIN open even if not attached |
| `-l, --label`         |         | 添加或覆盖标签 Add or override a label                       |
| `--name`              |         | 为容器分配一个名称 Assign a name to the container            |
| `-T, --no-TTY`        | `true`  | 禁用伪 TTY 分配（默认：自动检测） Disable pseudo-TTY allocation (default: auto-detected) |
| `--no-deps`           |         | 不启动链接的服务 Don't start linked services                 |
| `-p, --publish`       |         | 将容器的端口发布到主机 Publish a container's port(s) to the host |
| `--quiet-pull`        |         | 拉取时不打印进度信息 Pull without printing progress information |
| `--remove-orphans`    |         | 移除 Compose 文件中未定义服务的容器 Remove containers for services not defined in the Compose file |
| `--rm`                |         | 容器退出时自动移除 Automatically remove the container when it exits |
| `-P, --service-ports` |         | 以启用并映射所有服务端口的方式运行命令 Run command with all service's ports enabled and mapped to the host |
| `--use-aliases`       |         | 在容器连接的网络中使用服务的网络别名 Use the service's network useAliases in the network(s) the container connects to |
| `-u, --user`          |         | 以指定的用户名或 UID 运行 Run as specified username or uid   |
| `-v, --volume`        |         | 绑定挂载一个卷 Bind mount a volume                           |
| `-w, --workdir`       |         | 容器内的工作目录 Working directory inside the container      |
