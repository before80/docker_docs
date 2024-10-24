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

| Symbol | Description                     |
| ------ | ------------------------------- |
| `A`    | A file or directory was added   |
| `D`    | A file or directory was deleted |
| `C`    | A file or directory was changed |

You can use the full or shortened container ID or the container name set using `docker run --name` option.

## Examples

Inspect the changes to an `nginx` container:



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
