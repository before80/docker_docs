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

If the Compose file specifies an [image](https://github.com/compose-spec/compose-spec/blob/master/spec.md#image) name, the image is tagged with that name, substituting any variables beforehand. See [variable interpolation](https://github.com/compose-spec/compose-spec/blob/master/spec.md#interpolation).

If you change a service's `Dockerfile` or the contents of its build directory, run `docker compose build` to rebuild it.

## Options

| Option                | Default | Description                                                  |
| --------------------- | ------- | ------------------------------------------------------------ |
| `--build-arg`         |         | Set build-time variables for services                        |
| `--builder`           |         | Set builder to use                                           |
| `-m, --memory`        |         | Set memory limit for the build container. Not supported by BuildKit. |
| `--no-cache`          |         | Do not use cache when building the image                     |
| `--pull`              |         | Always attempt to pull a newer version of the image          |
| `--push`              |         | Push service images                                          |
| `-q, --quiet`         |         | Don't print anything to STDOUT                               |
| `--ssh`               |         | Set SSH authentications used when building service images. (use 'default' for using your default SSH Agent) |
| `--with-dependencies` |         | Also build dependencies (transitively)                       |
