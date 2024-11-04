+++
title = "docker compose build"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/build/](https://docs.docker.com/reference/cli/docker/compose/build/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose build

| Description | Build or rebuild services                     |
| :---------- | --------------------------------------------- |
| Usage       | `docker compose build [OPTIONS] [SERVICE...]` |

## Description

Services are built once and then tagged, by default as `project-service`.

​	服务构建一次后会被标记，默认标记为 `project-service`。

If the Compose file specifies an [image](https://github.com/compose-spec/compose-spec/blob/master/spec.md#image) name, the image is tagged with that name, substituting any variables beforehand. See [variable interpolation](https://github.com/compose-spec/compose-spec/blob/master/spec.md#interpolation).

​	如果 Compose 文件指定了一个 [image](https://github.com/compose-spec/compose-spec/blob/master/spec.md#image) 名称，镜像将以该名称进行标记，并事先替换任何变量。请参阅 [变量插值](https://github.com/compose-spec/compose-spec/blob/master/spec.md#interpolation)。

If you change a service's `Dockerfile` or the contents of its build directory, run `docker compose build` to rebuild it.

​	如果您更改了服务的 `Dockerfile` 或其构建目录的内容，请运行 `docker compose build` 进行重建。

## Options

| Option                | Default | Description                                                  |
| --------------------- | ------- | ------------------------------------------------------------ |
| `--build-arg`         |         | 为服务设置构建时变量Set build-time variables for services    |
| `--builder`           |         | 设置要使用的构建器 Set builder to use                        |
| `-m, --memory`        |         | 为构建容器设置内存限制。不支持 BuildKit。Set memory limit for the build container. Not supported by BuildKit. |
| `--no-cache`          |         | 构建镜像时不使用缓存 Do not use cache when building the image |
| `--pull`              |         | 始终尝试拉取新版本的镜像 Always attempt to pull a newer version of the image |
| `--push`              |         | 推送服务镜像 Push service images                             |
| `-q, --quiet`         |         | 不打印任何内容到 STDOUT Don't print anything to STDOUT       |
| `--ssh`               |         | 设置构建服务镜像时使用的 SSH 认证（使用 'default' 来使用默认 SSH Agent） Set SSH authentications used when building service images. (use 'default' for using your default SSH Agent) |
| `--with-dependencies` |         | 还构建依赖项（传递式构建） Also build dependencies (transitively) |
