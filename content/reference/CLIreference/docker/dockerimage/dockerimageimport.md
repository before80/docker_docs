+++
title = "docker image import"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/import/](https://docs.docker.com/reference/cli/docker/image/import/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image import

| Description | Import the contents from a tarball to create a filesystem image |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker image import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]` |
| Aliases     | `docker import`                                              |

## Description

You can specify a `URL` or `-` (dash) to take data directly from `STDIN`. The `URL` can point to an archive (.tar, .tar.gz, .tgz, .bzip, .tar.xz, or .txz) containing a filesystem or to an individual file on the Docker host. If you specify an archive, Docker untars it in the container relative to the `/` (root). If you specify an individual file, you must specify the full path within the host. To import from a remote location, specify a `URI` that begins with the `http://` or `https://` protocol.

​	你可以指定一个 `URL` 或 `-`（表示 STDIN），从中直接读取数据。`URL` 可以指向包含文件系统的归档文件（.tar、.tar.gz、.tgz、.bzip、.tar.xz 或 .txz），也可以是 Docker 主机上的单个文件。如果指定了归档文件，Docker 会在容器中将其解压到 `/`（根目录）。如果指定了单个文件，则必须指定主机上的完整路径。要从远程位置导入，需指定以 `http://` 或 `https://` 协议开头的 URI。

The `--change` option applies `Dockerfile` instructions to the image that is created. Supported `Dockerfile` instructions: 

​	`-change` 选项可以将 `Dockerfile` 指令应用于创建的镜像。支持的 `Dockerfile` 指令包括：

`CMD`|`ENTRYPOINT`|`ENV`|`EXPOSE`|`ONBUILD`|`USER`|`VOLUME`|`WORKDIR`

​	

## Options

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `-c, --change`  |         | 将 Dockerfile 指令应用到创建的镜像 Apply Dockerfile instruction to the created image |
| `-m, --message` |         | 为导入的镜像设置提交消息 Set commit message for imported image |
| `--platform`    |         | API 1.32+ 设置平台（如果服务器支持多平台） API 1.32+ Set platform if server is multi-platform capable |

## Examples

### 从远程位置导入 Import from a remote location

This creates a new untagged image.

​	创建一个新的未打标签的镜像。

```console
$ docker import https://example.com/exampleimage.tgz
```

### 从本地文件导入 Import from a local file

Import to docker via pipe and `STDIN`.

​	通过管道和 `STDIN` 导入至 docker。

```console
$ cat exampleimage.tgz | docker import - exampleimagelocal:new
```

Import with a commit message.

​	带有提交消息的导入。

```console
$ cat exampleimage.tgz | docker import --message "New image imported from tarball" - exampleimagelocal:new
```

Import to docker from a local archive.

​	从本地归档导入至 docker。

```console
$ docker import /path/to/exampleimage.tgz
```

### 从本地目录导入 Import from a local directory

​	

```console
$ sudo tar -c . | docker import - exampleimagedir
```

### 使用新配置从本地目录导入 Import from a local directory with new configurations



```console
$ sudo tar -c . | docker import --change "ENV DEBUG=true" - exampleimagedir
```

Note the `sudo` in this example – you must preserve the ownership of the files (especially root ownership) during the archiving with tar. If you are not root (or the sudo command) when you tar, then the ownerships might not get preserved.

​	注意此示例中的 `sudo`——你在使用 tar 进行归档时必须保持文件的所有权（特别是 root 的所有权）。如果归档时没有以 root 或使用 sudo 命令执行，所有权可能不会得到保留。
