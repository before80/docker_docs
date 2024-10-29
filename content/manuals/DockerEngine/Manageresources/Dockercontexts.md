+++
title = "Docker 上下文"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/manage-resources/contexts/](https://docs.docker.com/engine/manage-resources/contexts/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker contexts - Docker 上下文

## 简介 Introduction

This guide shows how you can use contexts to manage Docker daemons from a single client.

​	本指南展示如何使用上下文来从单个客户端管理 Docker 守护进程。

Each context contains all information required to manage resources on the daemon. The `docker context` command makes it easy to configure these contexts and switch between them.

​	每个上下文包含管理守护进程资源所需的所有信息。`docker context` 命令可以轻松配置这些上下文并在其间切换。

As an example, a single Docker client might be configured with two contexts:

​	例如，一个 Docker 客户端可以配置两个上下文：

- A default context running locally
  - 一个默认的本地运行的上下文

- A remote, shared context
  - 一个远程的共享上下文


Once these contexts are configured, you can use the `docker context use <context-name>` command to switch between them.

​	配置这些上下文后，可以使用 `docker context use <context-name>` 命令在其间切换。

## 前置条件 Prerequisites

To follow the examples in this guide, you'll need:

​	要跟随本指南的示例，您需要：

- A Docker client that supports the top-level `context` command
  - 支持顶级 `context` 命令的 Docker 客户端

Run `docker context` to verify that your Docker client supports contexts.

​	运行 `docker context` 以验证您的 Docker 客户端是否支持上下文。

## 上下文的结构 The anatomy of a context

A context is a combination of several properties. These include:

​	一个上下文是多个属性的组合，包括：

- Name and description
  - 名称和描述

- Endpoint configuration
  - 端点配置

- TLS info
  - TLS 信息


To list available contexts, use the `docker context ls` command.

​	使用 `docker context ls` 命令列出可用的上下文。

```console
$ docker context ls
NAME        DESCRIPTION                               DOCKER ENDPOINT               ERROR
default *                                             unix:///var/run/docker.sock
```

This shows a single context called "default". It's configured to talk to a daemon through the local `/var/run/docker.sock` Unix socket.

​	此处显示了一个名为 "default" 的单一上下文，配置为通过本地 Unix 套接字 `/var/run/docker.sock` 与守护进程通信。

The asterisk in the `NAME` column indicates that this is the active context. This means all `docker` commands run against this context, unless overridden with environment variables such as `DOCKER_HOST` and `DOCKER_CONTEXT`, or on the command-line with the `--context` and `--host` flags.

​	`NAME` 列中的星号表示这是活动上下文，这意味着所有的 `docker` 命令都会针对该上下文运行，除非通过设置 `DOCKER_HOST` 和 `DOCKER_CONTEXT` 等环境变量或使用 `--context` 和 `--host` 标志在命令行中覆盖。

Dig a bit deeper with `docker context inspect`. The following example shows how to inspect the context called `default`.

​	可以通过 `docker context inspect` 查看更详细的信息。以下示例展示如何检查名为 `default` 的上下文。

```console
$ docker context inspect default
[
    {
        "Name": "default",
        "Metadata": {},
        "Endpoints": {
            "docker": {
                "Host": "unix:///var/run/docker.sock",
                "SkipTLSVerify": false
            }
        },
        "TLSMaterial": {},
        "Storage": {
            "MetadataPath": "\u003cIN MEMORY\u003e",
            "TLSPath": "\u003cIN MEMORY\u003e"
        }
    }
]
```

### 创建新上下文 Create a new context

You can create new contexts with the `docker context create` command.

​	可以使用 `docker context create` 命令创建新上下文。

The following example creates a new context called `docker-test` and specifies the host endpoint of the context to TCP socket `tcp://docker:2375`.

​	以下示例创建了一个名为 `docker-test` 的新上下文，并将该上下文的主机端点指定为 TCP 套接字 `tcp://docker:2375`。

```console
$ docker context create docker-test --docker host=tcp://docker:2375
docker-test
Successfully created context "docker-test"
```

The new context is stored in a `meta.json` file below `~/.docker/contexts/`. Each new context you create gets its own `meta.json` stored in a dedicated sub-directory of `~/.docker/contexts/`.

​	新上下文存储在 `~/.docker/contexts/` 下的 `meta.json` 文件中。每个新创建的上下文都有其自己的 `meta.json`，存储在 `~/.docker/contexts/` 的专用子目录中。

You can view the new context with `docker context ls` and `docker context inspect <context-name>`.

​	可以使用 `docker context ls` 和 `docker context inspect <context-name>` 查看新上下文。

```console
$ docker context ls
NAME          DESCRIPTION                             DOCKER ENDPOINT               ERROR
default *                                             unix:///var/run/docker.sock
docker-test                                           tcp://docker:2375
```

The current context is indicated with an asterisk ("*").

​	当前上下文用星号 ("*") 表示。

## 使用不同的上下文 Use a different context

You can use `docker context use` to switch between contexts.

​	可以使用 `docker context use` 在上下文之间切换。

The following command will switch the `docker` CLI to use the `docker-test` context.

​	以下命令将 `docker` CLI 切换为使用 `docker-test` 上下文。

```console
$ docker context use docker-test
docker-test
Current context is now "docker-test"
```

Verify the operation by listing all contexts and ensuring the asterisk ("*") is against the `docker-test` context.

​	通过列出所有上下文并确保 `docker-test` 上下文上有星号 ("*") 来验证操作。

```console
$ docker context ls
NAME            DESCRIPTION                           DOCKER ENDPOINT               ERROR
default                                               unix:///var/run/docker.sock
docker-test *                                         tcp://docker:2375
```

`docker` commands will now target endpoints defined in the `docker-test` context.

​	现在，`docker` 命令将针对 `docker-test` 上下文中定义的端点。

You can also set the current context using the `DOCKER_CONTEXT` environment variable. The environment variable overrides the context set with `docker context use`.

​	也可以使用 `DOCKER_CONTEXT` 环境变量设置当前上下文。该环境变量将覆盖 `docker context use` 设置的上下文。

Use the appropriate command below to set the context to `docker-test` using an environment variable.

​	使用以下适当的命令通过环境变量将上下文设置为 `docker-test`。

{{< tabpane text=true persist=disabled >}}

{{% tab header="PowerShell" %}}

```ps
> $env:DOCKER_CONTEXT='docker-test'
```

{{% /tab  %}}

{{% tab header="Bash" %}}

```bash
 export DOCKER_CONTEXT=docker-test
```

{{% /tab  %}}

{{< /tabpane >}}

 

------

Run `docker context ls` to verify that the `docker-test` context is now the active context.

​	运行 `docker context ls` 以验证 `docker-test` 上下文现在是否为活动上下文。

You can also use the global `--context` flag to override the context. The following command uses a context called `production`.

​	还可以使用全局 `--context` 标志覆盖上下文。以下命令使用名为 `production` 的上下文。

```console
$ docker --context production container ls
```

## 导出和导入 Docker 上下文 Exporting and importing Docker contexts

You can use the `docker context export` and `docker context import` commands to export and import contexts on different hosts.

​	可以使用 `docker context export` 和 `docker context import` 命令在不同主机上导出和导入上下文。

The `docker context export` command exports an existing context to a file. The file can be imported on any host that has the `docker` client installed.

​	`docker context export` 命令将现有上下文导出到文件。可以在安装了 `docker` 客户端的任意主机上导入该文件。

### 导出和导入上下文 Exporting and importing a context

The following example exports an existing context called `docker-test`. It will be written to a file called `docker-test.dockercontext`.

​	以下示例将一个名为 `docker-test` 的现有上下文导出。它将写入一个名为 `docker-test.dockercontext` 的文件。



```console
$ docker context export docker-test
Written file "docker-test.dockercontext"
```

Check the contents of the export file.

​	检查导出文件的内容。



```console
$ cat docker-test.dockercontext
```

Import this file on another host using `docker context import` to create context with the same configuration.

​	在另一台主机上使用 `docker context import` 命令导入此文件，以创建具有相同配置的上下文。



```console
$ docker context import docker-test docker-test.dockercontext
docker-test
Successfully imported context "docker-test"
```

You can verify that the context was imported with `docker context ls`.

​	可以使用 `docker context ls` 验证上下文是否已导入。

The format of the import command is `docker context import <context-name> <context-file>`.

​	导入命令的格式为 `docker context import <context-name> <context-file>`。

## 更新上下文 Updating a context

You can use `docker context update` to update fields in an existing context.

​	可以使用 `docker context update` 更新现有上下文中的字段。

The following example updates the description field in the existing `docker-test` context.

​	以下示例更新了现有 `docker-test` 上下文中的描述字段。



```console
$ docker context update docker-test --description "Test context"
docker-test
Successfully updated context "docker-test"
```
