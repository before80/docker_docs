+++
title = "docker container rm"
date = 2024-10-23T14:54:43+08:00
weight = 140
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/rm/](https://docs.docker.com/reference/cli/docker/container/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container rm

| Description | Remove one or more containers                            |
| :---------- | -------------------------------------------------------- |
| Usage       | `docker container rm [OPTIONS] CONTAINER [CONTAINER...]` |
| Aliases     | `docker container remove``docker rm`                     |

## Description

Remove one or more containers

​	移除一个或多个容器。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --force`](https://docs.docker.com/reference/cli/docker/container/rm/#force) |         | 强制移除正在运行的容器（使用 SIGKILL） Force the removal of a running container (uses SIGKILL) |
| [`-l, --link`](https://docs.docker.com/reference/cli/docker/container/rm/#link) |         | 移除指定链接 Remove the specified link                       |
| [`-v, --volumes`](https://docs.docker.com/reference/cli/docker/container/rm/#volumes) |         | 移除与容器相关的匿名卷 Remove anonymous volumes associated with the container |

## Examples

### 移除一个容器 Remove a container

This removes the container referenced under the link `/redis`.

​	该命令移除链接 `/redis` 下的容器。



```console
$ docker rm /redis

/redis
```

### 移除默认桥接网络上的 `--link` 指定的链接（`--link`） Remove a link specified with `--link` on the default bridge network (`--link`)

This removes the underlying link between `/webapp` and the `/redis` containers on the default bridge network, removing all network communication between the two containers. This does not apply when `--link` is used with user-specified networks.

​	该命令移除默认桥接网络上 `/webapp` 和 `/redis` 容器之间的底层链接，移除这两个容器之间的所有网络通信。当 `--link` 与用户指定的网络一起使用时，则不适用。



```console
$ docker rm --link /webapp/redis

/webapp/redis
```

### 强制移除正在运行的容器（`--force`） Force-remove a running container (`--force`)

This command force-removes a running container.

​	该命令强制移除正在运行的容器。



```console
$ docker rm --force redis

redis
```

The main process inside the container referenced under the link `redis` will receive `SIGKILL`, then the container will be removed.

​	链接为 `redis` 的容器内部的主进程将接收到 `SIGKILL`，然后容器将被移除。

### 移除所有已停止的容器 Remove all stopped containers

Use the [`docker container prune`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerprune" >}}) command to remove all stopped containers, or refer to the [`docker system prune`]({{< ref "/reference/CLIreference/docker/dockersystem/dockersystemprune" >}}) command to remove unused containers in addition to other Docker resources, such as (unused) images and networks.

​	使用 [`docker container prune`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerprune" >}}) 命令移除所有已停止的容器，或参考 [`docker system prune`]({{< ref "/reference/CLIreference/docker/dockersystem/dockersystemprune" >}}) 命令，移除未使用的容器以及其他 Docker 资源，如（未使用的）镜像和网络。

Alternatively, you can use the `docker ps` with the `-q` / `--quiet` option to generate a list of container IDs to remove, and use that list as argument for the `docker rm` command.

​	或者，您可以使用 `docker ps` 和 `-q` / `--quiet` 选项生成要移除的容器 ID 列表，并将该列表作为参数传递给 `docker rm` 命令。

Combining commands can be more flexible, but is less portable as it depends on features provided by the shell, and the exact syntax may differ depending on what shell is used. To use this approach on Windows, consider using PowerShell or Bash.

​	组合命令可以更灵活，但因依赖于 shell 提供的功能而降低可移植性，确切语法可能因所用 shell 的不同而有所差异。在 Windows 上使用此方法时，可以考虑使用 PowerShell 或 Bash。

The example below uses `docker ps -q` to print the IDs of all containers that have exited (`--filter status=exited`), and removes those containers with the `docker rm` command:

​	下面的示例使用 `docker ps -q` 打印所有已退出容器的 ID（`--filter status=exited`），并使用 `docker rm` 命令移除这些容器：



```console
$ docker rm $(docker ps --filter status=exited -q)
```

Or, using the `xargs` Linux utility:

​	或者，使用 `xargs` Linux 工具：



```console
$ docker ps --filter status=exited -q | xargs docker rm
```

### 移除一个容器及其卷（`-v`，`--volumes`） Remove a container and its volumes (`-v`, `--volumes`)



```console
$ docker rm --volumes redis
redis
```

This command removes the container and any volumes associated with it. Note that if a volume was specified with a name, it will not be removed.

​	该命令移除容器及其关联的所有卷。请注意，如果指定了名称的卷，则不会被移除。

### 移除一个容器并有选择性地移除卷 Remove a container and selectively remove volumes



```console
$ docker create -v awesome:/foo -v /bar --name hello redis
hello

$ docker rm -v hello
```

In this example, the volume for `/foo` remains intact, but the volume for `/bar` is removed. The same behavior holds for volumes inherited with `--volumes-from`.

​	在这个例子中，`/foo` 的卷保持不变，但 `/bar` 的卷被移除。对使用 `--volumes-from` 继承的卷也适用相同的行为。
