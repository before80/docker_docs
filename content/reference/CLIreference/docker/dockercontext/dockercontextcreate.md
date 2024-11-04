+++
title = "docker context create"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/context/create/](https://docs.docker.com/reference/cli/docker/context/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker context create

| Description | Create a context                          |
| :---------- | ----------------------------------------- |
| Usage       | `docker context create [OPTIONS] CONTEXT` |

## Description

Creates a new `context`. This lets you switch the daemon your `docker` CLI connects to.

​	创建一个新的 `context`，允许你切换 `docker` CLI 连接的守护进程。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `--description`                                              |         | 上下文的描述 Description of the context                      |
| [`--docker`](https://docs.docker.com/reference/cli/docker/context/create/#docker) |         | 设置 docker 端点 set the docker endpoint                     |
| [`--from`](https://docs.docker.com/reference/cli/docker/context/create/#from) |         | 从指定的上下文创建上下文 create context from a named context |

## Examples

### 使用 Docker 端点创建上下文 (`--docker`) Create a context with a Docker endpoint (`--docker`)

Use the `--docker` flag to create a context with a custom endpoint. The following example creates a context named `my-context` with a docker endpoint of `/var/run/docker.sock`:

​	使用 `--docker` 标志创建具有自定义端点的上下文。以下示例创建一个名为 `my-context` 的上下文，并将 docker 端点设置为 `/var/run/docker.sock`：

```console
$ docker context create \
    --docker host=unix:///var/run/docker.sock \
    my-context
```

### 基于现有上下文创建上下文 (`--from`) Create a context based on an existing context (`--from`)

Use the `--from=<context-name>` option to create a new context from an existing context. The example below creates a new context named `my-context` from the existing context `existing-context`:

​	使用 `--from=<context-name>` 选项从现有上下文创建一个新上下文。下面的示例从现有的 `existing-context` 上下文创建一个名为 `my-context` 的新上下文：

```console
$ docker context create --from existing-context my-context
```

If the `--from` option isn't set, the `context` is created from the current context:

​	如果未设置 `--from` 选项，则 `context` 是从当前上下文创建的：

```console
$ docker context create my-context
```

This can be used to create a context out of an existing `DOCKER_HOST` based script:

​	这可以用于从现有的 `DOCKER_HOST` 脚本创建一个上下文：

```console
$ source my-setup-script.sh
$ docker context create my-context
```

To source the `docker` endpoint configuration from an existing context use the `--docker from=<context-name>` option. The example below creates a new context named `my-context` using the docker endpoint configuration from the existing context `existing-context`:

​	要从现有上下文导入 `docker` 端点配置，请使用 `--docker from=<context-name>` 选项。下面的示例使用现有的 `existing-context` 中的 docker 端点配置创建一个名为 `my-context` 的新上下文：

```console
$ docker context create \
    --docker from=existing-context \
    my-context
```

Docker endpoints configurations, as well as the description can be modified with `docker context update`.

​	可以使用 `docker context update` 修改 Docker 端点配置以及描述。

Refer to the [`docker context update` reference]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextupdate" >}}) for details.

​	有关详细信息，请参阅 `docker context update` 参考。
