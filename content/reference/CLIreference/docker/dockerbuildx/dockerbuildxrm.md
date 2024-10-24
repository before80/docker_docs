+++
title = "docker buildx rm"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/buildx/rm/](https://docs.docker.com/reference/cli/docker/buildx/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx rm

| Description | Remove one or more builder instances          |
| :---------- | --------------------------------------------- |
| Usage       | `docker buildx rm [OPTIONS] [NAME] [NAME...]` |

## [Description](https://docs.docker.com/reference/cli/docker/buildx/rm/#description)

Removes the specified or current builder. It is a no-op attempting to remove the default builder.

## [Options](https://docs.docker.com/reference/cli/docker/buildx/rm/#options)

| Option                                                       | Default | Description                      |
| ------------------------------------------------------------ | ------- | -------------------------------- |
| [`--all-inactive`](https://docs.docker.com/reference/cli/docker/buildx/rm/#all-inactive) |         | Remove all inactive builders     |
| [`-f, --force`](https://docs.docker.com/reference/cli/docker/buildx/rm/#force) |         | Do not prompt for confirmation   |
| [`--keep-daemon`](https://docs.docker.com/reference/cli/docker/buildx/rm/#keep-daemon) |         | Keep the BuildKit daemon running |
| [`--keep-state`](https://docs.docker.com/reference/cli/docker/buildx/rm/#keep-state) |         | Keep BuildKit state              |

## [Examples](https://docs.docker.com/reference/cli/docker/buildx/rm/#examples)

### [Remove all inactive builders (--all-inactive)](https://docs.docker.com/reference/cli/docker/buildx/rm/#all-inactive)

Remove builders that are not in running state.



```console
$ docker buildx rm --all-inactive
WARNING! This will remove all builders that are not in running state. Are you sure you want to continue? [y/N] y
```

### [Override the configured builder instance (--builder)](https://docs.docker.com/reference/cli/docker/buildx/rm/#builder)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

### [Do not prompt for confirmation (--force)](https://docs.docker.com/reference/cli/docker/buildx/rm/#force)

Do not prompt for confirmation before removing inactive builders.



```console
$ docker buildx rm --all-inactive --force
```

### [Keep the BuildKit daemon running (--keep-daemon)](https://docs.docker.com/reference/cli/docker/buildx/rm/#keep-daemon)

Keep the BuildKit daemon running after the buildx context is removed. This is useful when you manage BuildKit daemons and buildx contexts independently. Only supported by the [`docker-container`](https://docs.docker.com/build/drivers/docker-container/) and [`kubernetes`](https://docs.docker.com/build/drivers/kubernetes/) drivers.

### [Keep BuildKit state (--keep-state)](https://docs.docker.com/reference/cli/docker/buildx/rm/#keep-state)

Keep BuildKit state, so it can be reused by a new builder with the same name. Currently, only supported by the [`docker-container` driver](https://docs.docker.com/build/drivers/docker-container/).
