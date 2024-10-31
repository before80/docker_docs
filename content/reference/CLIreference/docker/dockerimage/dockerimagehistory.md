+++
title = "docker image history"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/history/](https://docs.docker.com/reference/cli/docker/image/history/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image history

| Description | Show the history of an image           |
| :---------- | -------------------------------------- |
| Usage       | `docker image history [OPTIONS] IMAGE` |
| Aliases     | `docker history`                       |

## Description

Show the history of an image

​	显示镜像的历史记录。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--format`](https://docs.docker.com/reference/cli/docker/image/history/#format) |         | 使用自定义模板格式化输出：<br/>\- 'table'：以表格格式输出并包含列标题（默认）<br/>\- 'table TEMPLATE'：使用指定的 Go 模板以表格格式输出<br/>\- 'json'：以 JSON 格式输出<br/>\- 'TEMPLATE'：使用指定的 Go 模板输出。更多关于模板的格式化信息，请参阅 https://docs.docker.com/go/formatting/<br />Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-H, --human`                                                | `true`  | 以人类可读的格式显示大小和日期 Print sizes and dates in human readable format |
| `--no-trunc`                                                 |         | 不截断输出 Don't truncate output                             |
| `-q, --quiet`                                                |         | 仅显示镜像 ID - Only show image IDs                          |

## Examples

To see how the `docker:latest` image was built:

​	查看 `docker:latest` 镜像的构建过程：

```console
$ docker history docker

IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
3e23a5875458        8 days ago          /bin/sh -c #(nop) ENV LC_ALL=C.UTF-8            0 B
8578938dd170        8 days ago          /bin/sh -c dpkg-reconfigure locales &&    loc   1.245 MB
be51b77efb42        8 days ago          /bin/sh -c apt-get update && apt-get install    338.3 MB
4b137612be55        6 weeks ago         /bin/sh -c #(nop) ADD jessie.tar.xz in /        121 MB
750d58736b4b        6 weeks ago         /bin/sh -c #(nop) MAINTAINER Tianon Gravi <ad   0 B
511136ea3c5a        9 months ago                                                        0 B                 Imported from -
```

To see how the `docker:apache` image was added to a container's base image:

​	查看如何将 `docker:apache` 镜像添加到容器的基础镜像：

```console
$ docker history docker:scm
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
2ac9d1098bf1        3 months ago        /bin/bash                                       241.4 MB            Added Apache to Fedora base image
88b42ffd1f7c        5 months ago        /bin/sh -c #(nop) ADD file:1fd8d7f9f6557cafc7   373.7 MB
c69cab00d6ef        5 months ago        /bin/sh -c #(nop) MAINTAINER Lokesh Mandvekar   0 B
511136ea3c5a        19 months ago                                                       0 B                 Imported from -
```

### Format the output (--format)

The formatting option (`--format`) will pretty-prints history output using a Go template.

​	格式化选项（`--format`）可以使用 Go 模板格式化历史输出。

Valid placeholders for the Go template are listed below:

​	Go 模板的有效占位符如下：

| Placeholder     | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `.ID`           | 镜像 ID - Image ID                                           |
| `.CreatedSince` | 若 `--human=true` 显示镜像创建的时间差，否则显示时间戳 Elapsed time since the image was created if `--human=true`, otherwise timestamp of when image was created |
| `.CreatedAt`    | 显示镜像创建的时间戳 Timestamp of when image was created     |
| `.CreatedBy`    | 创建镜像的命令 Command that was used to create the image     |
| `.Size`         | 镜像磁盘大小 Image disk size                                 |
| `.Comment`      | 镜像注释 Comment for image                                   |

When using the `--format` option, the `history` command either outputs the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`history` 命令将根据模板的声明精确输出数据；如果使用 `table` 指令，还会包含列标题。

The following example uses a template without headers and outputs the `ID` and `CreatedSince` entries separated by a colon (`:`) for the `busybox` image:

​	以下示例不包含标题，输出 `busybox` 镜像的 `ID` 和 `CreatedSince` 项，并以冒号 (`:`) 分隔：

```console
$ docker history --format "{{.ID}}: {{.CreatedSince}}" busybox

f6e427c148a7: 4 weeks ago
<missing>: 4 weeks ago
```
