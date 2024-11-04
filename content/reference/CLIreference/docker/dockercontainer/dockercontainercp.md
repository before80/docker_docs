+++
title = "docker container cp"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/cp/](https://docs.docker.com/reference/cli/docker/container/cp/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container cp

| Description | Copy files/folders between a container and the local filesystem |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|- docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH` |
| Aliases     | `docker cp`                                                  |

## Description

The `docker cp` utility copies the contents of `SRC_PATH` to the `DEST_PATH`. You can copy from the container's file system to the local machine or the reverse, from the local filesystem to the container. If `-` is specified for either the `SRC_PATH` or `DEST_PATH`, you can also stream a tar archive from `STDIN` or to `STDOUT`. The `CONTAINER` can be a running or stopped container. The `SRC_PATH` or `DEST_PATH` can be a file or directory.

​	`docker cp` 工具用于将 `SRC_PATH` 的内容复制到 `DEST_PATH`。你可以从容器的文件系统复制文件到本地机器，或反向操作，从本地文件系统复制到容器。如果为 `SRC_PATH` 或 `DEST_PATH` 指定了 `-`，你还可以从 `STDIN` 流式传输 tar 归档或向 `STDOUT` 输出。`CONTAINER` 可以是正在运行或已停止的容器。`SRC_PATH` 或 `DEST_PATH` 可以是文件或目录。

The `docker cp` command assumes container paths are relative to the container's `/` (root) directory. This means supplying the initial forward slash is optional; The command sees `compassionate_darwin:/tmp/foo/myfile.txt` and `compassionate_darwin:tmp/foo/myfile.txt` as identical. Local machine paths can be an absolute or relative value. The command interprets a local machine's relative paths as relative to the current working directory where `docker cp` is run.

​	`docker cp` 命令假定容器路径是相对于容器的 `/`（根）目录的。这意味着提供初始的正斜杠是可选的；该命令将 `compassionate_darwin:/tmp/foo/myfile.txt` 和 `compassionate_darwin:tmp/foo/myfile.txt` 视为相同。主机路径可以是绝对值或相对值。该命令将主机的相对路径解释为相对于运行 `docker cp` 的当前工作目录。

The `cp` command behaves like the Unix `cp -a` command in that directories are copied recursively with permissions preserved if possible. Ownership is set to the user and primary group at the destination. For example, files copied to a container are created with `UID:GID` of the root user. Files copied to the local machine are created with the `UID:GID` of the user which invoked the `docker cp` command. However, if you specify the `-a` option, `docker cp` sets the ownership to the user and primary group at the source. If you specify the `-L` option, `docker cp` follows any symbolic link in the `SRC_PATH`. `docker cp` doesn't create parent directories for `DEST_PATH` if they don't exist.

​	`cp` 命令的行为类似于 Unix 的 `cp -a` 命令，因为目录会递归复制，并尽可能保留权限。所有权设置为目标的用户和主组。例如，复制到容器的文件会使用 root 用户的 `UID:GID` 创建。复制到本地机器的文件会使用调用 `docker cp` 命令的用户的 `UID:GID` 创建。但是，如果指定了 `-a` 选项，`docker cp` 会将所有权设置为源处的用户和主组。如果指定了 `-L` 选项，`docker cp` 会跟随 `SRC_PATH` 中的任何符号链接。如果 `DEST_PATH` 的父目录不存在，`docker cp` 不会创建。

Assuming a path separator of `/`, a first argument of `SRC_PATH` and second argument of `DEST_PATH`, the behavior is as follows:

​	假设路径分隔符为 `/`，第一个参数为 `SRC_PATH`，第二个参数为 `DEST_PATH`，行为如下：

- `SRC_PATH` specifies a file `SRC_PATH` 指定一个文件
  - `DEST_PATH` does not exist `DEST_PATH` 不存在
    - the file is saved to a file created at `DEST_PATH` 文件保存到在 `DEST_PATH` 创建的文件中

  - `DEST_PATH` does not exist and ends with `/` `DEST_PATH` 不存在且以 `/` 结尾
    - Error condition: the destination directory must exist. 错误条件：目标目录必须存在。

  - `DEST_PATH` exists and is a file `DEST_PATH` 存在且是文件
    - the destination is overwritten with the source file's contents 目标文件被源文件的内容覆盖

  - `DEST_PATH` exists and is a directory `DEST_PATH` 存在且是目录
    - the file is copied into this directory using the basename from `SRC_PATH ` 文件被复制到此目录中，使用 `SRC_PATH` 的基本名称


- SRC_PATH specifies a directory `SRC_PATH` 指定一个目录

  - DEST_PATH does not exist `DEST_PATH` 不存在
    - DEST_PATH is created as a directory and the contents of the source directory are copied into this directory `DEST_PATH` 创建为一个目录，并将源目录的内容复制到此目录中

  - DEST_PATH exists and is a file `DEST_PATH` 存在且是文件
    - Error condition: cannot copy a directory to a file 错误条件：无法将目录复制到文件

  - DEST_PATH exists and is a directory `DEST_PATH` 存在且是目录

    - SRC_PATH does not end with /. (that is: slash followed by dot) `SRC_PATH` 不以 /.（即：斜杠后跟点）结尾
      - the source directory is copied into this directory 源目录被复制到此目录中

    - SRC_PATH does end with /. (that is: slash followed by dot) `SRC_PATH` 以 /.（即：斜杠后跟点）结尾
      - the content of the source directory is copied into this directory 源目录的内容被复制到此目录中



The command requires `SRC_PATH` and `DEST_PATH` to exist according to the above rules. If `SRC_PATH` is local and is a symbolic link, the symbolic link, not the target, is copied by default. To copy the link target and not the link, specify the `-L` option.

​	该命令要求根据上述规则 `SRC_PATH` 和 `DEST_PATH` 必须存在。如果 `SRC_PATH` 是本地并且是符号链接，则默认情况下复制的是符号链接，而不是目标。要复制链接目标而不是链接，请指定 `-L` 选项。

A colon (`:`) is used as a delimiter between `CONTAINER` and its path. You can also use `:` when specifying paths to a `SRC_PATH` or `DEST_PATH` on a local machine, for example `file:name.txt`. If you use a `:` in a local machine path, you must be explicit with a relative or absolute path, for example:

​	使用冒号（`:`）作为 `CONTAINER` 及其路径之间的分隔符。在指定本地机器上的 `SRC_PATH` 或 `DEST_PATH` 的路径时，也可以使用 `:`，例如 `file:name.txt`。如果在本地机器路径中使用 `:`，则必须明确指定相对路径或绝对路径，例如：

```
`/path/to/file:name.txt` or `./file:name.txt`
```

## Options

| Option              | Default | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| `-a, --archive`     |         | 归档模式（复制所有 uid/gid 信息） Archive mode (copy all uid/gid information) |
| `-L, --follow-link` |         | 始终跟随 `SRC_PATH` 中的符号链接 Always follow symbol link in SRC_PATH |
| `-q, --quiet`       |         | 在复制过程中抑制进度输出。如果没有终端连接，则进度输出会自动抑制 Suppress progress output during copy. Progress output is automatically suppressed if no terminal is attached |

## Examples

Copy a local file into container

​	将本地文件复制到容器

```console
$ docker cp ./some_file CONTAINER:/work
```

Copy files from container to local path

​	从容器复制文件到本地路径

```console
$ docker cp CONTAINER:/var/logs/ /tmp/app_logs
```

Copy a file from container to stdout. Note `cp` command produces a tar stream

​	从容器复制文件到 stdout。注意 `cp` 命令会生成 tar 流

```console
$ docker cp CONTAINER:/var/logs/app.log - | tar x -O | grep "ERROR"
```

### 边界情况 Corner cases

It isn't possible to copy certain system files such as resources under `/proc`, `/sys`, `/dev`, [tmpfs](https://docs.docker.com/reference/cli/docker/container/run/#tmpfs), and mounts created by the user in the container. However, you can still copy such files by manually running `tar` in `docker exec`. Both of the following examples do the same thing in different ways (consider `SRC_PATH` and `DEST_PATH` are directories):

​	无法复制某些系统文件，例如 `/proc`、`/sys`、`/dev`、[tmpfs](https://docs.docker.com/reference/cli/docker/container/run/#tmpfs) 下的资源，以及用户在容器中创建的挂载点。不过，你仍然可以通过手动在 `docker exec` 中运行 `tar` 来复制这些文件。以下两个示例以不同的方式完成相同的操作（假设 `SRC_PATH` 和 `DEST_PATH` 是目录）：

```console
$ docker exec CONTAINER tar Ccf $(dirname SRC_PATH) - $(basename SRC_PATH) | tar Cxf DEST_PATH -
```



```console
$ tar Ccf $(dirname SRC_PATH) - $(basename SRC_PATH) | docker exec -i CONTAINER tar Cxf DEST_PATH -
```

Using `-` as the `SRC_PATH` streams the contents of `STDIN` as a tar archive. The command extracts the content of the tar to the `DEST_PATH` in container's filesystem. In this case, `DEST_PATH` must specify a directory. Using `-` as the `DEST_PATH` streams the contents of the resource as a tar archive to `STDOUT`.

​	使用 `-` 作为 `SRC_PATH` 会将 `STDIN` 的内容作为 tar 归档流式传输。该命令会将 tar 的内容提取到容器的 `DEST_PATH` 文件系统中。在这种情况下，`DEST_PATH` 必须指定为目录。使用 `-` 作为 `DEST_PATH` 会将资源的内容作为 tar 归档流式传输到 `STDOUT`。
