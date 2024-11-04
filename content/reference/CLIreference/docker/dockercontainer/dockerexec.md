+++
title = "docker exec"
date = 2024-10-23T14:54:43+08:00
weight = 220
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/exec/](https://docs.docker.com/reference/cli/docker/container/exec/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container exec

| Description | Execute a command in a running container                     |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]` |
| Aliases     | `docker exec`                                                |

> **Introducing Docker Debug 介绍 Docker 调试**
>
> To easily get a debug shell into any container, use `docker debug`. Docker Debug is a replacement for debugging with `docker exec`. With it, you can get a shell into any container or image, even slim ones, without modifications. Plus, you can bring along your favorite debugging tools in its customizable toolbox.
>
> ​	要轻松进入任何容器的调试 shell，请使用 `docker debug`。Docker 调试是 `docker exec` 的替代方案。通过它，您可以进入任何容器或镜像，即使是精简版的，也无需进行修改。此外，您还可以在其可自定义的工具箱中带上您最喜欢的调试工具。
>
> Explore [Docker Debug]({{< ref "/reference/CLIreference/docker/dockerdebug" >}}) now.
>
> ​	立即探索 [Docker 调试]({{< ref "/reference/CLIreference/docker/dockerdebug" >}})。

## Description

The `docker exec` command runs a new command in a running container.

​	`docker exec` 命令在运行中的容器内运行新命令。

The command you specify with `docker exec` only runs while the container's primary process (`PID 1`) is running, and it isn't restarted if the container is restarted.

​	您使用 `docker exec` 指定的命令仅在容器的主进程 (`PID 1`) 运行时执行，如果容器重新启动，该命令不会被重新启动。

The command runs in the default working directory of the container.

​	该命令在容器的默认工作目录中运行。

The command must be an executable. A chained or a quoted command doesn't work.

​	命令必须是可执行的。链式命令或引用的命令不起作用。

- This works: `docker exec -it my_container sh -c "echo a && echo b"`
  - 这个有效：`docker exec -it my_container sh -c "echo a && echo b"`

- This doesn't work: `docker exec -it my_container "echo a && echo b"`

  - 这个无效：`docker exec -it my_container "echo a && echo b"`

  

## Options

| Option                                                       | Default | xxxxxxxxxx3 1$ docker wait my_container2​30console            |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `-d, --detach`                                               |         | 分离模式：在后台运行命令 Detached mode: run command in the background |
| `--detach-keys`                                              |         | 覆盖用于分离容器的键序列 Override the key sequence for detaching a container |
| [`-e, --env`](https://docs.docker.com/reference/cli/docker/container/exec/#env) |         | API 1.25+ 设置环境变量 API 1.25+ Set environment variables   |
| `--env-file`                                                 |         | API 1.25+ 从文件中读取环境变量 API 1.25+ Read in a file of environment variables |
| `-i, --interactive`                                          |         | 即使未附加也保持 STDIN 打开 Keep STDIN open even if not attached |
| [`--privileged`](https://docs.docker.com/reference/cli/docker/container/exec/#privileged) |         | 给予命令扩展权限 Give extended privileges to the command     |
| `-t, --tty`                                                  |         | 分配一个伪终端 Allocate a pseudo-TTY                         |
| `-u, --user`                                                 |         | 用户名或 UID - Username or UID (format: `<name|uid>[:<group|gid>]`) |
| [`-w, --workdir`](https://docs.docker.com/reference/cli/docker/container/exec/#workdir) |         | API 1.35+ 在容器内的工作目录 API 1.35+ Working directory inside the container |

## Examples

### 在运行中的容器上运行 `docker exec` - Run `docker exec` on a running container

First, start a container.

​	首先，启动一个容器。



```console
$ docker run --name mycontainer -d -i -t alpine /bin/sh
```

This creates and starts a container named `mycontainer` from an `alpine` image with an `sh` shell as its main process. The `-d` option (shorthand for `--detach`) sets the container to run in the background, in detached mode, with a pseudo-TTY attached (`-t`). The `-i` option is set to keep `STDIN` attached (`-i`), which prevents the `sh` process from exiting immediately.

​	这将创建并启动一个名为 `mycontainer` 的容器，使用 `alpine` 镜像，并将 `sh` shell 作为其主进程。`-d` 选项（`--detach` 的缩写）将容器设置为在后台运行，以分离模式运行，并附加一个伪终端（`-t`）。`-i` 选项用于保持 `STDIN` 附加（`-i`），这防止 `sh` 进程立即退出。

Next, execute a command on the container.

​	接下来，在容器上执行一个命令。



```console
$ docker exec -d mycontainer touch /tmp/execWorks
```

This creates a new file `/tmp/execWorks` inside the running container `mycontainer`, in the background.

​	这将在运行中的容器 `mycontainer` 内部创建一个新文件 `/tmp/execWorks`，在后台运行。

Next, execute an interactive `sh` shell on the container.

​	接下来，在容器上执行一个交互式 `sh` shell。



```console
$ docker exec -it mycontainer sh
```

This starts a new shell session in the container `mycontainer`.

​	这将在容器 `mycontainer` 中启动一个新的 shell 会话。

### 为 exec 进程设置环境变量 (`--env`, `-e`) - Set environment variables for the exec process (`--env`, `-e`)

Next, set environment variables in the current bash session.

​	接下来，在当前 bash 会话中设置环境变量。

The `docker exec` command inherits the environment variables that are set at the time the container is created. Use the `--env` (or the `-e` shorthand) to override global environment variables, or to set additional environment variables for the process started by `docker exec`.

​	`docker exec` 命令继承在创建容器时设置的环境变量。使用 `--env`（或 `-e` 缩写）来覆盖全局环境变量，或为由 `docker exec` 启动的进程设置附加环境变量。

The following example creates a new shell session in the container `mycontainer`, with environment variables `$VAR_A` set to `1`, and `$VAR_B` set to `2`. These environment variables are only valid for the `sh` process started by that `docker exec` command, and aren't available to other processes running inside the container.

​	以下示例在容器 `mycontainer` 中创建一个新的 shell 会话，环境变量 `$VAR_A` 设置为 `1`，`$VAR_B` 设置为 `2`。这些环境变量仅对由该 `docker exec` 命令启动的 `sh` 进程有效，其他在容器内运行的进程不可用。



```console
$ docker exec -e VAR_A=1 -e VAR_B=2 mycontainer env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=f64a4851eb71
VAR_A=1
VAR_B=2
HOME=/root
```

### 提升容器权限 (`--privileged`) Escalate container privileges (`--privileged`)

See [`docker run --privileged`](https://docs.docker.com/reference/cli/docker/container/run/#privileged).

​	参见 [`docker run --privileged`](https://docs.docker.com/reference/cli/docker/container/run/#privileged)。

### 为 exec 进程设置工作目录 (`--workdir`, `-w`) Set the working directory for the exec process (`--workdir`, `-w`)

By default `docker exec` command runs in the same working directory set whenhe container was created.

​	默认情况下，`docker exec` 命令在创建容器时设置的相同工作目录中运行。



```console
$ docker exec -it mycontainer pwd
/
```

You can specify an alternative working directory for the command to execute using the `--workdir` option (or the `-w` shorthand):

​	您可以使用 `--workdir` 选项（或 `-w` 缩写）为要执行的命令指定替代工作目录：



```console
$ docker exec -it -w /root mycontainer pwd
/root
```

### 尝试在暂停的容器上运行 `docker exec` - Try to run `docker exec` on a paused container

If the container is paused, then the `docker exec` command fails with an error:

​	如果容器处于暂停状态，则 `docker exec` 命令将失败并返回错误：



```console
$ docker pause mycontainer
mycontainer

$ docker ps

CONTAINER ID   IMAGE     COMMAND     CREATED          STATUS                   PORTS     NAMES
482efdf39fac   alpine    "/bin/sh"   17 seconds ago   Up 16 seconds (Paused)             mycontainer

$ docker exec mycontainer sh

Error response from daemon: Container mycontainer is paused, unpause the container before exec

$ echo $?
1
```
