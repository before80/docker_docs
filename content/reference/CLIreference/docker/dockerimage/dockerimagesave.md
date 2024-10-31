+++
title = "docker image save"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/save/](https://docs.docker.com/reference/cli/docker/image/save/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image save

| Description | Save one or more images to a tar archive (streamed to STDOUT by default) |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker image save [OPTIONS] IMAGE [IMAGE...]`               |
| Aliases     | `docker save`                                                |

## Description

Produces a tarred repository to the standard output stream. Contains all parent layers, and all tags + versions, or specified `repo:tag`, for each argument provided.

​	生成一个 tar 格式的仓库并输出到标准输出流。包含所有父层以及所有标签和版本，或为每个提供的参数指定的 `repo:tag`。

## Options

| Option         | Default | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| `-o, --output` |         | 写入文件，而不是 STDOUT  - Write to a file, instead of STDOUT |

## Examples

### 创建一个可以用 `docker load` 加载的备份。 Create a backup that can then be used with `docker load`.



```console
$ docker save busybox > busybox.tar

$ ls -sh busybox.tar

2.7M busybox.tar

$ docker save --output busybox.tar busybox

$ ls -sh busybox.tar

2.7M busybox.tar

$ docker save -o fedora-all.tar fedora

$ docker save -o fedora-latest.tar fedora:latest
```

### 使用 gzip 将镜像保存为 tar.gz 文件 Save an image to a tar.gz file using gzip

You can use gzip to save the image file and make the backup smaller.

​	你可以使用 gzip 来保存镜像文件并使备份变得更小。



```console
$ docker save myimage:latest | gzip > myimage_latest.tar.gz
```

### 挑选特定标签 Cherry-pick particular tags

You can even cherry-pick particular tags of an image repository.

​	你甚至可以挑选镜像仓库的特定标签。

```console
$ docker save -o ubuntu.tar ubuntu:lucid ubuntu:saucy
```
