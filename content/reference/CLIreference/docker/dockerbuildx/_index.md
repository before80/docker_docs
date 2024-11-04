+++
title = "docker buildx"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/](https://docs.docker.com/reference/cli/docker/buildx/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx

| Description | Docker Buildx   |
| :---------- | --------------- |
| **Usage**   | `docker buildx` |

## Description

Extended build capabilities with BuildKit

​	扩展的 BuildKit 构建功能

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) |         | 覆盖已配置的构建实例 Override the configured builder instance |
| `-D, --debug`                                                |         | 启用调试日志 Enable debug logging                            |

## Examples

### 覆盖已配置的构建实例 (`--builder`) Override the configured builder instance (`--builder`)

You can also use the `BUILDX_BUILDER` environment variable.

​	您也可以使用 `BUILDX_BUILDER` 环境变量。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker buildx bake`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbake" >}}) | 从文件中构建 Build from a file                               |
| [`docker buildx build`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbuild" >}}) | 开始构建 Start a build                                       |
| [`docker buildx create`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxcreate" >}}) | 创建新的构建实例 Create a new builder instance               |
| [`docker buildx debug`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxdebug" >}}) | 启动调试器 Start debugger                                    |
| [`docker buildx du`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxdu" >}}) | 磁盘使用情况 Disk usage                                      |
| [`docker buildx imagetools`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildximagetools" >}}) | 操作镜像仓库中的镜像命令 Commands to work on images in registry |
| [`docker buildx inspect`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxinspect" >}}) | 检查当前的构建实例 Inspect current builder instance          |
| [`docker buildx ls`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxls" >}}) | 列出构建实例 List builder instances                          |
| [`docker buildx prune`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxprune" >}}) | 删除构建缓存 Remove build cache                              |
| [`docker buildx rm`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxrm" >}}) | 删除一个或多个构建实例 Remove one or more builder instances  |
| [`docker buildx stop`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxstop" >}}) | 停止构建实例 Stop builder instance                           |
| [`docker buildx use`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxuse" >}}) | 设置当前的构建实例 Set the current builder instance          |
| [`docker buildx version`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxversion" >}}) | 显示 buildx 版本信息 Show buildx version information         |
