+++
title = "docker image"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/](https://docs.docker.com/reference/cli/docker/image/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image

| Description | Manage images  |
| :---------- | -------------- |
| Usage       | `docker image` |

## Description

Manage images.

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker image history`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagehistory" >}}) | 显示镜像的历史记录 Show the history of an image              |
| [`docker image import`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimageimport" >}}) | 从 tar 包中导入内容以创建文件系统镜像 Import the contents from a tarball to create a filesystem image |
| [`docker image inspect`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimageinspect" >}}) | 显示一个或多个镜像的详细信息 Display detailed information on one or more images |
| [`docker image load`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimageload" >}}) | 从 tar 归档或标准输入中加载镜像 Load an image from a tar archive or STDIN |
| [`docker image prune`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimageprune" >}}) | 移除未使用的镜像 Remove unused images                        |
| [`docker image rm`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagerm" >}}) | 删除一个或多个镜像 Remove one or more images                 |
| [`docker image save`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagesave" >}}) | 将一个或多个镜像保存为 tar 归档（默认为流式输出至标准输出） Save one or more images to a tar archive (streamed to STDOUT by default) |
| [`docker image tag`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagetag" >}}) | 创建一个引用 SOURCE_IMAGE 的 TARGET_IMAGE 标签 Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE |
| [`docker image ls`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimages" >}}) | 列出镜像 List images                                         |
| [`docker image pull`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpull" >}}) | 从注册表下载镜像 Download an image from a registry           |
| [`docker image push`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpush" >}}) | 将镜像上传至注册表 Upload an image to a registry             |
