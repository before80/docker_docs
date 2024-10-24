+++
title = "docker compose push"
date = 2024-10-23T14:54:43+08:00
weight = 160
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/push/](https://docs.docker.com/reference/cli/docker/compose/push/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose push

| Description | Push service images                          |
| :---------- | -------------------------------------------- |
| Usage       | `docker compose push [OPTIONS] [SERVICE...]` |

## Description

Pushes images for services to their respective registry/repository.

The following assumptions are made:

- You are pushing an image you have built locally
- You have access to the build key

Examples



```yaml
services:
  service1:
    build: .
    image: localhost:5000/yourimage  ## goes to local registry

  service2:
    build: .
    image: your-dockerid/yourimage  ## goes to your repository on Docker Hub
```

## Options

| Option                   | Default | Description                                            |
| ------------------------ | ------- | ------------------------------------------------------ |
| `--ignore-push-failures` |         | Push what it can and ignores images with push failures |
| `--include-deps`         |         | Also push images of services declared as dependencies  |
| `-q, --quiet`            |         | Push without printing progress information             |
