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

​	将服务的镜像推送到相应的注册表/仓库。

The following assumptions are made:

​	以下假设成立：

- You are pushing an image you have built locally
  - 你正在推送一个在本地构建的镜像

- You have access to the build key
  - 你有权访问构建密钥

Examples



```yaml
services:
  service1:
    build: .
    image: localhost:5000/yourimage  ## goes to local registry 推送到本地注册表

  service2:
    build: .
    image: your-dockerid/yourimage  ## goes to your repository on Docker Hub 推送到你在 Docker Hub 的仓库
```

## Options

| Option                   | Default | Description                                                  |
| ------------------------ | ------- | ------------------------------------------------------------ |
| `--ignore-push-failures` |         | 尝试推送可用的镜像，忽略推送失败的镜像 Push what it can and ignores images with push failures |
| `--include-deps`         |         | 还推送声明为依赖的服务的镜像 Also push images of services declared as dependencies |
| `-q, --quiet`            |         | 无需打印进度信息进行推送 Push without printing progress information |
