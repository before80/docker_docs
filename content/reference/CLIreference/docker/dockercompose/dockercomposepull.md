+++
title = "docker compose pull"
date = 2024-10-23T14:54:43+08:00
weight = 150
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/pull/](https://docs.docker.com/reference/cli/docker/compose/pull/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose pull

| Description | Pull service images                          |
| :---------- | -------------------------------------------- |
| Usage       | `docker compose pull [OPTIONS] [SERVICE...]` |

## Description

Pulls an image associated with a service defined in a `compose.yaml` file, but does not start containers based on those images

​	拉取与 `compose.yaml` 文件中定义的服务相关联的镜像，但不启动基于这些镜像的容器。

## Options

| Option                   | Default | Description                                                  |
| ------------------------ | ------- | ------------------------------------------------------------ |
| `--ignore-buildable`     |         | 忽略可以构建的镜像 Ignore images that can be built           |
| `--ignore-pull-failures` |         | 尝试拉取可用的镜像，忽略拉取失败的镜像 Pull what it can and ignores images with pull failures |
| `--include-deps`         |         | 还拉取声明为依赖的服务的镜像 Also pull services declared as dependencies |
| `--policy`               |         | 应用拉取策略 Apply pull policy ("missing"\|"always")         |
| `-q, --quiet`            |         | 无需打印进度信息进行拉取 Pull without printing progress information |

## Examples

Consider the following `compose.yaml`:

​	考虑以下 `compose.yaml` 文件：

```yaml
services:
  db:
    image: postgres
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
```

If you run `docker compose pull ServiceName` in the same directory as the `compose.yaml` file that defines the service, Docker pulls the associated image. For example, to call the postgres image configured as the db service in our example, you would run `docker compose pull db`.

​	如果你在定义了服务的 `compose.yaml` 文件的同一目录下运行 `docker compose pull ServiceName`，Docker 将拉取相关的镜像。例如，要拉取配置为 db 服务的 postgres 镜像，可以运行 `docker compose pull db`。



```console
$ docker compose pull db
[+] Running 1/15
 ⠸ db Pulling                                                             12.4s
   ⠿ 45b42c59be33 Already exists                                           0.0s
   ⠹ 40adec129f1a Downloading  3.374MB/4.178MB                             9.3s
   ⠹ b4c431d00c78 Download complete                                        9.3s
   ⠹ 2696974e2815 Download complete                                        9.3s
   ⠹ 564b77596399 Downloading  5.622MB/7.965MB                             9.3s
   ⠹ 5044045cf6f2 Downloading  216.7kB/391.1kB                             9.3s
   ⠹ d736e67e6ac3 Waiting                                                  9.3s
   ⠹ 390c1c9a5ae4 Waiting                                                  9.3s
   ⠹ c0e62f172284 Waiting                                                  9.3s
   ⠹ ebcdc659c5bf Waiting                                                  9.3s
   ⠹ 29be22cb3acc Waiting                                                  9.3s
   ⠹ f63c47038e66 Waiting                                                  9.3s
   ⠹ 77a0c198cde5 Waiting                                                  9.3s
   ⠹ c8752d5b785c Waiting                                                  9.3s
```

`docker compose pull` tries to pull image for services with a build section. If pull fails, it lets you know this service image must be built. You can skip this by setting `--ignore-buildable` flag.

​	`docker compose pull` 尝试为具有构建部分的服务拉取镜像。如果拉取失败，它会告知你该服务的镜像必须构建。你可以通过设置 `--ignore-buildable` 标志跳过此步骤。
