+++
title = "docker buildx inspect"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/inspect/](https://docs.docker.com/reference/cli/docker/buildx/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx inspect

| Description | Inspect current builder instance |
| :---------- | -------------------------------- |
| Usage       | `docker buildx inspect [NAME]`   |

## Description

Shows information about the current or specified builder.

​	显示当前或指定构建器的信息。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--bootstrap`](https://docs.docker.com/reference/cli/docker/buildx/inspect/#bootstrap) |         | 确保在检查之前构建器已启动 Ensure builder has booted before inspecting |

## Examples

### 在检查之前确保构建器正在运行（`--bootstrap`） Ensure that the builder is running before inspecting (`--bootstrap`)

Use the `--bootstrap` option to ensure that the builder is running before inspecting it. If the driver is `docker-container`, then `--bootstrap` starts the BuildKit container and waits until it's operational. Bootstrapping is automatically done during build, and therefore not necessary. The same BuildKit container is used during the lifetime of the associated builder node (as displayed in `buildx ls`).

​	使用 `--bootstrap` 选项确保在检查构建器之前，它正在运行。如果驱动程序是 `docker-container`，则 `--bootstrap` 启动 BuildKit 容器并等待其运行。引导过程在构建期间自动进行，因此并不是必要的。在关联的构建器节点的生命周期内，使用的是相同的 BuildKit 容器（如在 `buildx ls` 中所示）。

### 覆盖配置的构建器实例（`--builder`） Override the configured builder instance (`--builder`)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

​	与 [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) 相同。

### 获取构建器实例的信息 Get information about a builder instance

By default, `inspect` shows information about the current builder. Specify the name of the builder to inspect to get information about that builder. The following example shows information about a builder instance named `elated_tesla`:

​	默认情况下，`inspect` 显示当前构建器的信息。指定要检查的构建器名称以获取该构建器的信息。以下示例显示名为 `elated_tesla` 的构建器实例的信息：

> **Note**
>
> The asterisk (`*`) next to node build platform(s) indicate they have been manually set during `buildx create`. Otherwise the platforms were automatically detected.
>
> ​	节点构建平台旁边的星号（`*`）表示它们是在 `buildx create` 期间手动设置的。否则，这些平台是自动检测的。



```console
$ docker buildx inspect elated_tesla
Name:          elated_tesla
Driver:        docker-container
Last Activity: 2022-11-30 12:42:47 +0100 CET

Nodes:
Name:           elated_tesla0
Endpoint:       unix:///var/run/docker.sock
Driver Options: env.BUILDKIT_STEP_LOG_MAX_SPEED="10485760" env.JAEGER_TRACE="localhost:6831" image="moby/buildkit:latest" network="host" env.BUILDKIT_STEP_LOG_MAX_SIZE="10485760"
Status:         running
Flags:          --debug --allow-insecure-entitlement security.insecure --allow-insecure-entitlement network.host
BuildKit:       v0.10.6
Platforms:      linux/arm64*, linux/arm/v7, linux/arm/v6
Labels:
 org.mobyproject.buildkit.worker.executor:         oci
 org.mobyproject.buildkit.worker.hostname:         docker-desktop
 org.mobyproject.buildkit.worker.network:          host
 org.mobyproject.buildkit.worker.oci.process-mode: sandbox
 org.mobyproject.buildkit.worker.selinux.enabled:  false
 org.mobyproject.buildkit.worker.snapshotter:      overlayfs
GC Policy rule#0:
 All:           false
 Filters:       type==source.local,type==exec.cachemount,type==source.git.checkout
 Keep Duration: 48h0m0s
 Keep Bytes:    488.3MiB
GC Policy rule#1:
 All:           false
 Keep Duration: 1440h0m0s
 Keep Bytes:    24.21GiB
GC Policy rule#2:
 All:        false
 Keep Bytes: 24.21GiB
GC Policy rule#3:
 All:        true
 Keep Bytes: 24.21GiB
```

`debug` flag can also be used to get more information about the builder:

​	`debug` 标志也可以用于获取有关构建器的更多信息：



```console
$ docker --debug buildx inspect elated_tesla
```
