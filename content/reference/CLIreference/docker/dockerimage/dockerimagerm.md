+++
title = "docker image rm"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/rm/](https://docs.docker.com/reference/cli/docker/image/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image rm

| Description | Remove one or more images                    |
| :---------- | -------------------------------------------- |
| Usage       | `docker image rm [OPTIONS] IMAGE [IMAGE...]` |
| Aliases     | `docker image remove``docker rmi`            |

## Description

Removes (and un-tags) one or more images from the host node. If an image has multiple tags, using this command with the tag as a parameter only removes the tag. If the tag is the only one for the image, both the image and the tag are removed.

​	删除（并去标签）主机节点上的一个或多个镜像。如果一个镜像有多个标签，使用此命令并将标签作为参数仅删除该标签。如果该标签是镜像的唯一标签，则会同时删除镜像和标签。

This does not remove images from a registry. You cannot remove an image of a running container unless you use the `-f` option. To see all images on a host use the [`docker image ls`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimages" >}}) command.

​	这不会从注册表中删除镜像。除非使用 `-f` 选项，否则无法删除正在运行的容器的镜像。要查看主机上的所有镜像，请使用 [`docker image ls`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimages" >}}) 命令。

## Options

| Option        | Default | Description                                         |
| ------------- | ------- | --------------------------------------------------- |
| `-f, --force` |         | 强制删除镜像 Force removal of the image             |
| `--no-prune`  |         | 不删除未标记的父镜像 Do not delete untagged parents |

## Examples

You can remove an image using its short or long ID, its tag, or its digest. If an image has one or more tags referencing it, you must remove all of them before the image is removed. Digest references are removed automatically when an image is removed by tag.

​	您可以使用镜像的短 ID、长 ID、标签或摘要来删除镜像。如果一个镜像有一个或多个标签引用它，必须先删除所有标签，然后才能删除该镜像。当通过标签删除镜像时，摘要引用会自动删除。



```console
$ docker images

REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
test1                     latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)
test                      latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)
test2                     latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)

$ docker rmi fd484f19954f

Error: Conflict, cannot delete image fd484f19954f because it is tagged in multiple repositories, use -f to force
2013/12/11 05:47:16 Error: failed to remove one or more images

$ docker rmi test1:latest

Untagged: test1:latest

$ docker rmi test2:latest

Untagged: test2:latest


$ docker images

REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
test                      latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)

$ docker rmi test:latest

Untagged: test:latest
Deleted: fd484f19954f4920da7ff372b5067f5b7ddb2fd3830cecd17b96ea9e286ba5b8
```

If you use the `-f` flag and specify the image's short or long ID, then this command untags and removes all images that match the specified I

​	如果使用 `-f` 标志并指定镜像的短 ID 或长 ID，则该命令会去标签并删除所有匹配指定 ID 的镜像。



```console
$ docker images

REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
test1                     latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)
test                      latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)
test2                     latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)

$ docker rmi -f fd484f19954f

Untagged: test1:latest
Untagged: test:latest
Untagged: test2:latest
Deleted: fd484f19954f4920da7ff372b5067f5b7ddb2fd3830cecd17b96ea9e286ba5b8
```

An image pulled by digest has no tag associated with it:

​	通过摘要拉取的镜像没有与之关联的标签：



```console
$ docker images --digests

REPOSITORY                     TAG       DIGEST                                                                    IMAGE ID        CREATED         SIZE
localhost:5000/test/busybox    <none>    sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf   4986bf8c1536    9 weeks ago     2.43 MB
```

To remove an image using its digest:

​	使用摘要删除镜像：



```console
$ docker rmi localhost:5000/test/busybox@sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf
Untagged: localhost:5000/test/busybox@sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf
Deleted: 4986bf8c15363d1c5d15512d5266f8777bfba4974ac56e3270e7760f6f0a8125
Deleted: ea13149945cb6b1e746bf28032f02e9b5a793523481a0a18645fc77ad53c4ea2
Deleted: df7546f9f060a2268024c8a230d8639878585defcc1bc6f79d2728a13957871b
```
