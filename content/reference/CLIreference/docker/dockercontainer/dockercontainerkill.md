+++
title = "docker container kill"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/kill/](https://docs.docker.com/reference/cli/docker/container/kill/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container kill

| Description | Kill one or more running containers                        |
| :---------- | ---------------------------------------------------------- |
| Usage       | `docker container kill [OPTIONS] CONTAINER [CONTAINER...]` |
| Aliases     | `docker kill`                                              |

## Description

The `docker kill` subcommand kills one or more containers. The main process inside the container is sent `SIGKILL` signal (default), or the signal that is specified with the `--signal` option. You can reference a container by its ID, ID-prefix, or name.

​	`docker kill` 子命令用于终止一个或多个容器。容器内部的主进程会接收到 `SIGKILL` 信号（默认），或者使用 `--signal` 选项指定的信号。可以通过容器的 ID、ID 前缀或名称来引用容器。

The `--signal` flag sets the system call signal that is sent to the container. This signal can be a signal name in the format `SIG<NAME>`, for instance `SIGINT`, or an unsigned number that matches a position in the kernel's syscall table, for instance `2`.

​	`--signal` 标志设置发送到容器的系统调用信号。该信号可以是格式为 `SIG<NAME>` 的信号名称，例如 `SIGINT`，或者是一个无符号数字，该数字对应内核的系统调用表中的位置，例如 `2`。

While the default (`SIGKILL`) signal will terminate the container, the signal set through `--signal` may be non-terminal, depending on the container's main process. For example, the `SIGHUP` signal in most cases will be non-terminal, and the container will continue running after receiving the signal.

​	虽然默认的 `SIGKILL` 信号会终止容器，但通过 `--signal` 设置的信号可能是非终止的，这取决于容器的主进程。例如，在大多数情况下，`SIGHUP` 信号是非终止的，容器在接收到该信号后会继续运行。

> **Note**
>
> `ENTRYPOINT` and `CMD` in the *shell* form run as a child process of `/bin/sh -c`, which does not pass signals. This means that the executable is not the container’s PID 1 and does not receive Unix signals.
>
> ​	在 *shell* 形式的 `ENTRYPOINT` 和 `CMD` 中，作为 `/bin/sh -c` 的子进程运行，不会传递信号。这意味着可执行文件不是容器的 PID 1，并且不会接收到 Unix 信号。

## Options

| Option                                                       | Default | Description                                      |
| ------------------------------------------------------------ | ------- | ------------------------------------------------ |
| [`-s, --signal`](https://docs.docker.com/reference/cli/docker/container/kill/#signal) |         | 发送到容器的信号 Signal to send to the container |

## Examples

### 向容器发送 KILL 信号 Send a KILL signal to a container

The following example sends the default `SIGKILL` signal to the container named `my_container`:

​	以下示例向名为 `my_container` 的容器发送默认的 `SIGKILL` 信号：

```console
$ docker kill my_container
```

### 向容器发送自定义信号（`--signal`）Send a custom signal to a container (`--signal`)

The following example sends a `SIGHUP` signal to the container named `my_container`:

​	以下示例向名为 `my_container` 的容器发送 `SIGHUP` 信号：



```console
$ docker kill --signal=SIGHUP  my_container
```

You can specify a custom signal either by *name*, or *number*. The `SIG` prefix is optional, so the following examples are equivalent:

​	可以通过 *名称* 或 *数字* 指定自定义信号。`SIG` 前缀是可选的，因此以下示例是等效的：



```console
$ docker kill --signal=SIGHUP my_container
$ docker kill --signal=HUP my_container
$ docker kill --signal=1 my_container
```

Refer to the [`signal(7)`](https://man7.org/linux/man-pages/man7/signal.7.html) man-page for a list of standard Linux signals.

​	有关标准 Linux 信号的列表，请参考 [`signal(7)`](https://man7.org/linux/man-pages/man7/signal.7.html) 手册页。
