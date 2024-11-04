+++
title = "docker buildx debug"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/debug/](https://docs.docker.com/reference/cli/docker/buildx/debug/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx debug

| Description | Start debugger        |
| :---------- | --------------------- |
| Usage       | `docker buildx debug` |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

Start debugger

​	启动调试器

## Options

| Option            | Default | Description                                                  |
| ----------------- | ------- | ------------------------------------------------------------ |
| `--detach`        | `true`  | 实验性（CLI）分离 buildx 服务器以进行监控（仅支持 Linux）experimental (CLI) Detach buildx server for the monitor (supported only on linux) |
| `--invoke`        |         | 实验性（CLI）启动一个监控器并执行指定的命令experimental (CLI) Launch a monitor with executing specified command |
| `--on`            | `error` | 实验性（CLI）何时启动监控器（[always, error]） experimental (CLI) When to launch the monitor ([always, error]) |
| `--progress`      | `auto`  | 设置监控器的进度输出类型（`auto`, `plain`, `tty`, `rawjson`）。使用 plain 显示容器输出 Set type of progress output (`auto`, `plain`, `tty`, `rawjson`) for the monitor. Use plain to show container output |
| `--root`          |         | 实验性（CLI）指定要连接的服务器根目录 experimental (CLI) Specify root directory of server to connect for the monitor |
| `--server-config` |         | 实验性（CLI）指定监控器的 buildx 服务器配置文件（仅在启动新服务器时使用） experimental (CLI) Specify buildx server config file for the monitor (used only when launching new server) |

## Subcommands

| Command                                                      | Description            |
| :----------------------------------------------------------- | :--------------------- |
| [`docker buildx debug build`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxdebug/dockerbuildxdebugbuild" >}}) | 启动构建 Start a build |
