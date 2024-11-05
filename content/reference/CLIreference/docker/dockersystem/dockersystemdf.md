+++
title = "docker system df"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/system/df/](https://docs.docker.com/reference/cli/docker/system/df/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker system df

| Description | Show docker disk usage       |
| :---------- | ---------------------------- |
| Usage       | `docker system df [OPTIONS]` |

## Description

The `docker system df` command displays information regarding the amount of disk space used by the Docker daemon.

​	`docker system df` 命令显示有关 Docker 守护进程使用的磁盘空间的信息。

## Options

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `--format`      |         | 使用自定义模板格式化输出: 'table': 以表格格式打印输出并包含列标题（默认） 'table TEMPLATE': 使用指定的 Go 模板以表格格式打印输出 'json': 以 JSON 格式打印 'TEMPLATE': 使用指定的 Go 模板格式化输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-v, --verbose` |         | 显示空间使用的详细信息  Show detailed information on space usage |

## Examples

By default the command displays a summary of the data used:

​	默认情况下，命令显示所用数据的摘要：

```console
$ docker system df

TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              5                   2                   16.43 MB            11.63 MB (70%)
Containers          2                   0                   212 B               212 B (100%)
Local Volumes       2                   1                   36 B                0 B (0%)
```

Use the `-v, --verbose` flag to get more detailed information:

​	使用 `-v, --verbose` 标志获取更详细的信息：

```console
$ docker system df -v

Images space usage:

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE                SHARED SIZE         UNIQUE SIZE         CONTAINERS
my-curl             latest              b2789dd875bf        6 minutes ago       11 MB               11 MB               5 B                 0
my-jq               latest              ae67841be6d0        6 minutes ago       9.623 MB            8.991 MB            632.1 kB            0
<none>              <none>              a0971c4015c1        6 minutes ago       11 MB               11 MB               0 B                 0
alpine              latest              4e38e38c8ce0        9 weeks ago         4.799 MB            0 B                 4.799 MB            1
alpine              3.3                 47cf20d8c26c        9 weeks ago         4.797 MB            4.797 MB            0 B                 1

Containers space usage:

CONTAINER ID        IMAGE               COMMAND             LOCAL VOLUMES       SIZE                CREATED             STATUS                      NAMES
4a7f7eebae0f        alpine:latest       "sh"                1                   0 B                 16 minutes ago      Exited (0) 5 minutes ago    hopeful_yalow
f98f9c2aa1ea        alpine:3.3          "sh"                1                   212 B               16 minutes ago      Exited (0) 48 seconds ago   anon-vol

Local Volumes space usage:

NAME                                                               LINKS               SIZE
07c7bdf3e34ab76d921894c2b834f073721fccfbbcba792aa7648e3a7a664c2e   2                   36 B
my-named-vol                                                       0                   0 B
```

- `SHARED SIZE` is the amount of space that an image shares with another one (i.e. their common data)
  - `SHARED SIZE` 是镜像与其他镜像共享的空间量（即它们的公共数据）

- `UNIQUE SIZE` is the amount of space that's only used by a given image
  - `UNIQUE SIZE` 是仅被某个镜像使用的空间量

- `SIZE` is the virtual size of the image, it's the sum of `SHARED SIZE` and `UNIQUE SIZE`
  - `SIZE` 是镜像的虚拟大小，它是 `SHARED SIZE` 和 `UNIQUE SIZE` 的总和

> **Note**
>
> Network information isn't shown, because it doesn't consume disk space.
>
> ​	不显示网络信息，因为它不消耗磁盘空间。
