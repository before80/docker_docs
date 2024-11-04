+++
title = "docker buildx rm"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/rm/](https://docs.docker.com/reference/cli/docker/buildx/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx rm

| Description | Remove one or more builder instances          |
| :---------- | --------------------------------------------- |
| Usage       | `docker buildx rm [OPTIONS] [NAME] [NAME...]` |

## Description

Removes the specified or current builder. It is a no-op attempting to remove the default builder.

​	删除指定的或当前的构建器。尝试删除默认构建器将不会有任何操作。

## Options

| Option                                                       | Default | Description                                                |
| ------------------------------------------------------------ | ------- | ---------------------------------------------------------- |
| [`--all-inactive`](https://docs.docker.com/reference/cli/docker/buildx/rm/#all-inactive) |         | 删除所有不活跃的构建器 Remove all inactive builders        |
| [`-f, --force`](https://docs.docker.com/reference/cli/docker/buildx/rm/#force) |         | 删除时不提示确认 Do not prompt for confirmation            |
| [`--keep-daemon`](https://docs.docker.com/reference/cli/docker/buildx/rm/#keep-daemon) |         | 保持 BuildKit 守护进程运行Keep the BuildKit daemon running |
| [`--keep-state`](https://docs.docker.com/reference/cli/docker/buildx/rm/#keep-state) |         | 保持 BuildKit 状态 Keep BuildKit state                     |

## Examples

### 删除所有不活跃的构建器（`--all-inactive`） Remove all inactive builders (`--all-inactive`)

Remove builders that are not in running state.

​	删除所有不在运行状态的构建器。



```console
$ docker buildx rm --all-inactive
WARNING! This will remove all builders that are not in running state. Are you sure you want to continue? [y/N] y
```

### 覆盖配置的构建器实例（`--builder`） Override the configured builder instance (`--builder`)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

​	与 [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) 相同。

### 删除时不提示确认（`--force`） Do not prompt for confirmation (`--force`)

Do not prompt for confirmation before removing inactive builders.

​	在删除不活跃的构建器时不提示确认。

```console
$ docker buildx rm --all-inactive --force
```

### 保持 BuildKit 守护进程运行（`--keep-daemon`） Keep the BuildKit daemon running (`--keep-daemon`)

Keep the BuildKit daemon running after the buildx context is removed. This is useful when you manage BuildKit daemons and buildx contexts independently. Only supported by the [`docker-container`](https://docs.docker.com/build/drivers/docker-container/) and [`kubernetes`](https://docs.docker.com/build/drivers/kubernetes/) drivers.

​	在删除 buildx 上下文后保持 BuildKit 守护进程运行。当您独立管理 BuildKit 守护进程和 buildx 上下文时，这很有用。仅支持 [`docker-container`](https://docs.docker.com/build/drivers/docker-container/) 和 [`kubernetes`](https://docs.docker.com/build/drivers/kubernetes/) 驱动程序。

### 保持 BuildKit 状态（`--keep-state`） Keep BuildKit state (`--keep-state`)

Keep BuildKit state, so it can be reused by a new builder with the same name. Currently, only supported by the [`docker-container` driver](https://docs.docker.com/build/drivers/docker-container/).

​	保持 BuildKit 状态，以便可以被具有相同名称的新构建器重用。目前仅支持 [`docker-container` 驱动程序](https://docs.docker.com/build/drivers/docker-container/)。
