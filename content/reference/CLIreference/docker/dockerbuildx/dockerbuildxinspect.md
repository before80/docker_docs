+++
title = "docker buildx inspect"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/buildx/inspect/](https://docs.docker.com/reference/cli/docker/buildx/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx inspect

| Description | Inspect current builder instance |
| :---------- | -------------------------------- |
| Usage       | `docker buildx inspect [NAME]`   |

## [Description](https://docs.docker.com/reference/cli/docker/buildx/inspect/#description)

Shows information about the current or specified builder.

## [Options](https://docs.docker.com/reference/cli/docker/buildx/inspect/#options)

| Option                                                       | Default | Description                                 |
| ------------------------------------------------------------ | ------- | ------------------------------------------- |
| [`--bootstrap`](https://docs.docker.com/reference/cli/docker/buildx/inspect/#bootstrap) |         | Ensure builder has booted before inspecting |

## [Examples](https://docs.docker.com/reference/cli/docker/buildx/inspect/#examples)

### [Ensure that the builder is running before inspecting (--bootstrap)](https://docs.docker.com/reference/cli/docker/buildx/inspect/#bootstrap)

Use the `--bootstrap` option to ensure that the builder is running before inspecting it. If the driver is `docker-container`, then `--bootstrap` starts the BuildKit container and waits until it's operational. Bootstrapping is automatically done during build, and therefore not necessary. The same BuildKit container is used during the lifetime of the associated builder node (as displayed in `buildx ls`).

### [Override the configured builder instance (--builder)](https://docs.docker.com/reference/cli/docker/buildx/inspect/#builder)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

### [Get information about a builder instance](https://docs.docker.com/reference/cli/docker/buildx/inspect/#get-information-about-a-builder-instance)

By default, `inspect` shows information about the current builder. Specify the name of the builder to inspect to get information about that builder. The following example shows information about a builder instance named `elated_tesla`:

> **Note**
>
> The asterisk (`*`) next to node build platform(s) indicate they have been manually set during `buildx create`. Otherwise the platforms were automatically detected.



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



```console
$ docker --debug buildx inspect elated_tesla
```
