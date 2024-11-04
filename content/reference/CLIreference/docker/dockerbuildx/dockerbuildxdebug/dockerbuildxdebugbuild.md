+++
title = "docker buildx debug build"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/debug/build/](https://docs.docker.com/reference/cli/docker/buildx/debug/build/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx debug build

| Description | Start a build                                                |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker buildx debug build [OPTIONS] PATH | URL | -`         |
| Aliases     | `docker build``docker builder build``docker image build``docker buildx b` |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

Start a build

​	启动构建

## Options

| Option              | Default | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| `--add-host`        |         | 添加自定义的主机到 IP 映射（格式：`host:ip`）Add a custom host-to-IP mapping (format: `host:ip`) |
| `--allow`           |         | 允许额外的特权权利（例如：`network.host`, `security.insecure`）Allow extra privileged entitlement (e.g., `network.host`, `security.insecure`) |
| `--annotation`      |         | 为镜像添加注解 Add annotation to the image                   |
| `--attest`          |         | 证明参数（格式：`type=sbom,generator=image`） Attestation parameters (format: `type=sbom,generator=image`) |
| `--build-arg`       |         | 设置构建时变量 Set build-time variables                      |
| `--build-context`   |         | 附加的构建上下文（例如：name=path） Additional build contexts (e.g., name=path) |
| `--cache-from`      |         | 外部缓存源（例如：`user/app:cache`, `type=local,src=path/to/dir`） External cache sources (e.g., `user/app:cache`, `type=local,src=path/to/dir`) |
| `--cache-to`        |         | 缓存导出目标（例如：`user/app:cache`, `type=local,dest=path/to/dir`） Cache export destinations (e.g., `user/app:cache`, `type=local,dest=path/to/dir`) |
| `--call`            | `build` | 设置评估构建的方法（`check`, `outline`, `targets`） Set method for evaluating build (`check`, `outline`, `targets`) |
| `--cgroup-parent`   |         | 设置构建期间 `RUN` 指令的父 cgroup Set the parent cgroup for the `RUN` instructions during build |
| `--check`           |         | `--call=check` 的简写 Shorthand for `--call=check`           |
| `--detach`          |         | 实验性（CLI）分离 buildx 服务器（仅支持 Linux） experimental (CLI) Detach buildx server (supported only on linux) |
| `-f, --file`        |         | Dockerfile 的名称（默认：`PATH/Dockerfile`） Name of the Dockerfile (default: `PATH/Dockerfile`) |
| `--iidfile`         |         | 将镜像 ID 写入文件 Write the image ID to a file              |
| `--label`           |         | 为镜像设置元数据 Set metadata for an image                   |
| `--load`            |         | `--output=type=docker` 的简写 Shorthand for `--output=type=docker` |
| `--metadata-file`   |         | 将构建结果元数据写入文件 Write build result metadata to a file |
| `--network`         |         | 设置构建期间 `RUN` 指令的网络模式 Set the networking mode for the `RUN` instructions during build |
| `--no-cache`        |         | 构建镜像时不使用缓存 Do not use cache when building the image |
| `--no-cache-filter` |         | 不缓存指定的阶段 Do not cache specified stages               |
| `-o, --output`      |         | 输出目标（格式：`type=local,dest=path`） Output destination (format: `type=local,dest=path`) |
| `--platform`        |         | 设置构建的目标平台 Set target platform for build             |
| `--progress`        | `auto`  | 设置进度输出的类型（`auto`, `plain`, `tty`, `rawjson`）。使用 plain 显示容器输出 Set type of progress output (`auto`, `plain`, `tty`, `rawjson`). Use plain to show container output |
| `--provenance`      |         | `--attest=type=provenance` 的简写 Shorthand for `--attest=type=provenance` |
| `--pull`            |         | 始终尝试拉取所有引用的镜像 Always attempt to pull all referenced images |
| `--push`            |         | `--output=type=registry` 的简写 Shorthand for `--output=type=registry` |
| `-q, --quiet`       |         | 抑制构建输出并在成功时打印镜像 ID Suppress the build output and print image ID on success |
| `--root`            |         | 实验性（CLI）指定要连接的服务器根目录 experimental (CLI) Specify root directory of server to connect |
| `--sbom`            |         | `--attest=type=sbom` 的简写 Shorthand for `--attest=type=sbom` |
| `--secret`          |         | 在构建中暴露的秘密（格式：`id=mysecret[,src=/local/secret]`） Secret to expose to the build (format: `id=mysecret[,src=/local/secret]`) |
| `--server-config`   |         | 实验性（CLI）指定 buildx 服务器配置文件（仅在启动新服务器时使用） experimental (CLI) Specify buildx server config file (used only when launching new server) |
| `--shm-size`        |         | 构建容器的共享内存大小 Shared memory size for build containers |
| `--ssh`             |         | 要暴露给构建的 SSH 代理套接字或密钥（格式：`default SSH agent socket or keys to expose to the build (format: `default|<id>[=<socket>|<key>[,<key>]]`) |
| `-t, --tag`         |         | 名称和可选标签（格式：`name:tag`） Name and optionally a tag (format: `name:tag`) |
| `--target`          |         | 设置要构建的目标构建阶段 Set the target build stage to build |
| `--ulimit`          |         | ulimit 选项 Ulimit options                                   |
