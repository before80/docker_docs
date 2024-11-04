+++
title = "docker container diff"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/diff/](https://docs.docker.com/reference/cli/docker/container/diff/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container diff

| Description | Inspect changes to files or directories on a container's filesystem |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container diff CONTAINER`                            |
| Aliases     | `docker diff`                                                |

## Description

List the changed files and directories in a container᾿s filesystem since the container was created. Three different types of change are tracked:

​	列出自容器创建以来在容器文件系统中更改的文件和目录。跟踪三种不同类型的更改：

| Symbol | Description                                      |
| ------ | ------------------------------------------------ |
| `A`    | 添加了文件或目录 A file or directory was added   |
| `D`    | 删除了文件或目录 A file or directory was deleted |
| `C`    | 修改了文件或目录 A file or directory was changed |

You can use the full or shortened container ID or the container name set using `docker run --name` option.

​	您可以使用完整的或缩短的容器 ID，或者使用 `docker run --name` 选项设置的容器名称。

## Examples

Inspect the changes to an `nginx` container:

​	检查 `nginx` 容器的更改：



```console
$ docker diff 1fdfd1f54c1b

C /dev
C /dev/console
C /dev/core
C /dev/stdout
C /dev/fd
C /dev/ptmx
C /dev/stderr
C /dev/stdin
C /run
A /run/nginx.pid
C /var/lib/nginx/tmp
A /var/lib/nginx/tmp/client_body
A /var/lib/nginx/tmp/fastcgi
A /var/lib/nginx/tmp/proxy
A /var/lib/nginx/tmp/scgi
A /var/lib/nginx/tmp/uwsgi
C /var/log/nginx
A /var/log/nginx/access.log
A /var/log/nginx/error.log
```
