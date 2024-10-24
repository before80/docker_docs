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

The `--signal` flag sets the system call signal that is sent to the container. This signal can be a signal name in the format `SIG<NAME>`, for instance `SIGINT`, or an unsigned number that matches a position in the kernel's syscall table, for instance `2`.

While the default (`SIGKILL`) signal will terminate the container, the signal set through `--signal` may be non-terminal, depending on the container's main process. For example, the `SIGHUP` signal in most cases will be non-terminal, and the container will continue running after receiving the signal.

> **Note**
>
> `ENTRYPOINT` and `CMD` in the *shell* form run as a child process of `/bin/sh -c`, which does not pass signals. This means that the executable is not the container’s PID 1 and does not receive Unix signals.

## Options

| Option                                                       | Default | Description                     |
| ------------------------------------------------------------ | ------- | ------------------------------- |
| [`-s, --signal`](https://docs.docker.com/reference/cli/docker/container/kill/#signal) |         | Signal to send to the container |

## Examples

### Send a KILL signal to a container

The following example sends the default `SIGKILL` signal to the container named `my_container`:



```console
$ docker kill my_container
```

### Send a custom signal to a container (--signal)

The following example sends a `SIGHUP` signal to the container named `my_container`:



```console
$ docker kill --signal=SIGHUP  my_container
```

You can specify a custom signal either by *name*, or *number*. The `SIG` prefix is optional, so the following examples are equivalent:



```console
$ docker kill --signal=SIGHUP my_container
$ docker kill --signal=HUP my_container
$ docker kill --signal=1 my_container
```

Refer to the [`signal(7)`](https://man7.org/linux/man-pages/man7/signal.7.html) man-page for a list of standard Linux signals.
