+++
title = "docker image load"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/load/](https://docs.docker.com/reference/cli/docker/image/load/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image load

| Description | Load an image from a tar archive or STDIN |
| :---------- | ----------------------------------------- |
| Usage       | `docker image load [OPTIONS]`             |
| Aliases     | `docker load`                             |

## Description

Load an image or repository from a tar archive (even if compressed with gzip, bzip2, xz or zstd) from a file or STDIN. It restores both images and tags.

​	从文件或标准输入中加载一个 tar 归档（即使经过 gzip、bzip2、xz 或 zstd 压缩）。它可以恢复镜像和标签。

## Options

| Option                                                       | Default | Description                                  |
| ------------------------------------------------------------ | ------- | -------------------------------------------- |
| [`-i, --input`](https://docs.docker.com/reference/cli/docker/image/load/#input) |         | Read from tar archive file, instead of STDIN |
| `-q, --quiet`                                                |         | Suppress the load output                     |

## Examples



```console
$ docker image ls

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

### Load images from STDIN



```console
$ docker load < busybox.tar.gz

Loaded image: busybox:latest
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
busybox             latest              769b9341d937        7 weeks ago         2.489 MB
```

### Load images from a file (--input)



```console
$ docker load --input fedora.tar

Loaded image: fedora:rawhide
Loaded image: fedora:20

$ docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
busybox             latest              769b9341d937        7 weeks ago         2.489 MB
fedora              rawhide             0d20aec6529d        7 weeks ago         387 MB
fedora              20                  58394af37342        7 weeks ago         385.5 MB
fedora              heisenbug           58394af37342        7 weeks ago         385.5 MB
fedora              latest              58394af37342        7 weeks ago         385.5 MB
```
