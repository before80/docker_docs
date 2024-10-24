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

## Options

| Option            | Default | Description                                                  |
| ----------------- | ------- | ------------------------------------------------------------ |
| `--detach`        | `true`  | experimental (CLI) Detach buildx server for the monitor (supported only on linux) |
| `--invoke`        |         | experimental (CLI) Launch a monitor with executing specified command |
| `--on`            | `error` | experimental (CLI) When to launch the monitor ([always, error]) |
| `--progress`      | `auto`  | Set type of progress output (`auto`, `plain`, `tty`, `rawjson`) for the monitor. Use plain to show container output |
| `--root`          |         | experimental (CLI) Specify root directory of server to connect for the monitor |
| `--server-config` |         | experimental (CLI) Specify buildx server config file for the monitor (used only when launching new server) |

## Subcommands

| Command                                                      | Description   |
| :----------------------------------------------------------- | :------------ |
| [`docker buildx debug build`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxdebug/dockerbuildxdebugbuild" >}}) | Start a build |
