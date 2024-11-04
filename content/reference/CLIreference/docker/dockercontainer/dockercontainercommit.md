+++
title = "docker container commit"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/commit/](https://docs.docker.com/reference/cli/docker/container/commit/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container commit

| Description | Create a new image from a container's changes                |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]` |
| Aliases     | `docker commit`                                              |

## Description

It can be useful to commit a container's file changes or settings into a new image. This lets you debug a container by running an interactive shell, or export a working dataset to another server.

​	将容器的文件更改或设置提交到新镜像中是很有用的。这使你能够通过运行交互式 shell 来调试容器，或将有效的数据集导出到另一台服务器。

Commits do not include any data contained in mounted volumes.

​	提交不包括挂载卷中的任何数据。

By default, the container being committed and its processes will be paused while the image is committed. This reduces the likelihood of encountering data corruption during the process of creating the commit. If this behavior is undesired, set the `--pause` option to false.

​	默认情况下，在提交镜像时，被提交的容器及其进程将暂停。这减少了在创建提交过程中的数据损坏的可能性。如果不希望这种行为，请将 `--pause` 选项设置为 false。

The `--change` option will apply `Dockerfile` instructions to the image that's created. Supported `Dockerfile` instructions: 

​	`--change` 选项将把 `Dockerfile` 指令应用于创建的镜像。支持的 `Dockerfile` 指令有：

`CMD`|`ENTRYPOINT`|`ENV`|`EXPOSE`|`LABEL`|`ONBUILD`|`USER`|`VOLUME`|`WORKDIR`

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `-a, --author`                                               |         | 作者（例如，`John Hannibal Smith <hannibal@a-team.com>`） Author (e.g., `John Hannibal Smith <hannibal@a-team.com>`) |
| [`-c, --change`](https://docs.docker.com/reference/cli/docker/container/commit/#change) |         | 将 Dockerfile 指令应用于创建的镜像 Apply Dockerfile instruction to the created image |
| `-m, --message`                                              |         | 提交信息 Commit message                                      |
| `-p, --pause`                                                | `true`  | 在提交期间暂停容器 Pause container during commit             |

## Examples

### 提交一个容器 Commit a container



```console
$ docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS              NAMES
c3f279d17e0a        ubuntu:24.04        /bin/bash           7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:24.04        /bin/bash           7 days ago          Up 25 hours                            focused_hamilton

$ docker commit c3f279d17e0a  svendowideit/testimage:version3

f5283438590d

$ docker images

REPOSITORY                        TAG                 ID                  CREATED             SIZE
svendowideit/testimage            version3            f5283438590d        16 seconds ago      335.7 MB
```

### 提交带有新配置的容器（`--change`） Commit a container with new configurations (`--change`)



```console
$ docker ps

CONTAINER ID       IMAGE               COMMAND             CREATED             STATUS              PORTS              NAMES
c3f279d17e0a       ubuntu:24.04        /bin/bash           7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436       ubuntu:24.04        /bin/bash           7 days ago          Up 25 hours                            focused_hamilton

$ docker inspect -f "{{ .Config.Env }}" c3f279d17e0a

[HOME=/ PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin]

$ docker commit --change "ENV DEBUG=true" c3f279d17e0a  svendowideit/testimage:version3

f5283438590d

$ docker inspect -f "{{ .Config.Env }}" f5283438590d

[HOME=/ PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin DEBUG=true]
```

### 提交带有新 `CMD` 和 `EXPOSE` 指令的容器 Commit a container with new `CMD` and `EXPOSE` instructions



```console
$ docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS              NAMES
c3f279d17e0a        ubuntu:24.04        /bin/bash           7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:24.04        /bin/bash           7 days ago          Up 25 hours                            focused_hamilton

$ docker commit --change='CMD ["apachectl", "-DFOREGROUND"]' -c "EXPOSE 80" c3f279d17e0a  svendowideit/testimage:version4

f5283438590d

$ docker run -d svendowideit/testimage:version4

89373736e2e7f00bc149bd783073ac43d0507da250e999f3f1036e0db60817c0

$ docker ps

CONTAINER ID        IMAGE               COMMAND                 CREATED             STATUS              PORTS              NAMES
89373736e2e7        testimage:version4  "apachectl -DFOREGROU"  3 seconds ago       Up 2 seconds        80/tcp             distracted_fermat
c3f279d17e0a        ubuntu:24.04        /bin/bash               7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:24.04        /bin/bash               7 days ago          Up 25 hours                            focused_hamilton
```

