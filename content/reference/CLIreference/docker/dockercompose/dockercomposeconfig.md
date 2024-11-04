+++
title = "docker compose config"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/config/](https://docs.docker.com/reference/cli/docker/compose/config/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose config

| Description | Parse, resolve and render compose file in canonical format |
| :---------- | ---------------------------------------------------------- |
| Usage       | `docker compose config [OPTIONS] [SERVICE...]`             |
| Aliases     | `docker compose convert`                                   |

## Description

`docker compose config` renders the actual data model to be applied on the Docker Engine. It merges the Compose files set by `-f` flags, resolves variables in the Compose file, and expands short-notation into the canonical format.

​	`docker compose config` 渲染要应用于 Docker 引擎的实际数据模型。它合并由 `-f` 标志设置的 Compose 文件，解析 Compose 文件中的变量，并将简短表示法展开为规范格式。

## Options

| Option                    | Default | Description                                                  |
| ------------------------- | ------- | ------------------------------------------------------------ |
| `--environment`           |         | 打印用于插值的环境 Print environment used for interpolation. |
| `--format`                | `yaml`  | 格式化输出。值： [yaml  Format the output. Values: [yaml \| json] |
| `--hash`                  |         | 打印服务配置哈希值，每行一个 Print the service config hash, one per line. |
| `--images`                |         | 打印镜像名称，每行一个 Print the image names, one per line.  |
| `--no-consistency`        |         | 不检查模型一致性 - 警告：可能产生无效的 Compose 输出 Don't check model consistency - warning: may produce invalid Compose output |
| `--no-interpolate`        |         | 不插入环境变量 Don't interpolate environment variables       |
| `--no-normalize`          |         | 不规范化 Compose 模型 Don't normalize compose model          |
| `--no-path-resolution`    |         | 不解析文件路径 Don't resolve file paths                      |
| `-o, --output`            |         | 保存到文件（默认输出到 stdout） Save to file (default to stdout) |
| `--profiles`              |         | 打印配置文件名称，每行一个 Print the profile names, one per line. |
| `-q, --quiet`             |         | 仅验证配置，不打印任何内容 Only validate the configuration, don't print anything |
| `--resolve-image-digests` |         | 将镜像标签固定到摘要 Pin image tags to digests               |
| `--services`              |         | 打印服务名称，每行一个 Print the service names, one per line. |
| `--variables`             |         | 打印模型变量及默认值 Print model variables and default values. |
| `--volumes`               |         | 打印卷名称，每行一个 Print the volume names, one per line.   |
