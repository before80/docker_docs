+++
title = "docker volume create"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/volume/create/](https://docs.docker.com/reference/cli/docker/volume/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker volume create

| Description | Create a volume                           |
| :---------- | ----------------------------------------- |
| Usage       | `docker volume create [OPTIONS] [VOLUME]` |

## Description

Creates a new volume that containers can consume and store data in. If a name is not specified, Docker generates a random name.

​	创建一个新卷，供容器使用并存储数据。如果未指定名称，Docker 会生成一个随机名称。

## Options

| Option                                                       | Default  | Description                                                  |
| ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| `--availability`                                             | `active` | API 1.42+ Swarm 集群卷可用性（`active`、`pause`、`drain`） API 1.42+ Swarm Cluster Volume availability (`active`, `pause`, `drain`) |
| `-d, --driver`                                               | `local`  | 指定卷驱动程序名称 Specify volume driver name                |
| `--group`                                                    |          | API 1.42+ Swarm 集群卷组（集群卷） API 1.42+ Swarm Cluster Volume group (cluster volumes) |
| `--label`                                                    |          | 为卷设置元数据 Set metadata for a volume                     |
| `--limit-bytes`                                              |          | xxxxxxxxxx3 1$ docker volume inspect --format '{{ .Mountpoint }}' myvolume2​3/var/lib/docker/volumes/myvolume/_dataconsole |
| [`-o, --opt`](https://docs.docker.com/reference/cli/docker/volume/create/#opt) |          | 设置特定驱动程序的选项 Set driver specific options           |
| `--required-bytes`                                           |          | API 1.42+ Swarm 集群卷的最大大小（以字节为单位） API 1.42+ Swarm Maximum size of the Cluster Volume in bytes |
| `--scope`                                                    | `single` | API 1.42+ Swarm 集群卷访问范围（`single`、`multi`） API 1.42+ Swarm Cluster Volume access scope (`single`, `multi`) |
| `--secret`                                                   |          | API 1.42+ Swarm 集群卷的秘密 API 1.42+ Swarm Cluster Volume secrets |
| `--sharing`                                                  | `none`   | API 1.42+ Swarm 集群卷访问共享（`none`、`readonly`、`onewriter`、`all`） API 1.42+ Swarm Cluster Volume access sharing (`none`, `readonly`, `onewriter`, `all`) |
| `--topology-preferred`                                       |          | API 1.42+ Swarm 集群卷的首选拓扑API 1.42+ Swarm A topology that the Cluster Volume would be preferred in |
| `--topology-required`                                        |          | API 1.42+ Swarm 集群卷必须可访问的拓扑 API 1.42+ Swarm A topology that the Cluster Volume must be accessible from |
| `--type`                                                     | `block`  | API 1.42+ Swarm 集群卷访问类型（`mount`、`block`） API 1.42+ Swarm Cluster Volume access type (`mount`, `block`) |

## Examples

Create a volume and then configure the container to use it:

​	创建一个卷并配置容器使用它：

```console
$ docker volume create hello

hello

$ docker run -d -v hello:/world busybox ls /world
```

The mount is created inside the container's `/world` directory. Docker doesn't support relative paths for mount points inside the container.

​	挂载在容器的 `/world` 目录中。Docker 不支持容器内挂载点的相对路径。

Multiple containers can use the same volume. This is useful if two containers need access to shared data. For example, if one container writes and the other reads the data.

​	多个容器可以使用相同的卷。这对于两个需要访问共享数据的容器非常有用。例如，一个容器写入数据，而另一个容器读取数据。

Volume names must be unique among drivers. This means you can't use the same volume name with two different drivers. Attempting to create two volumes with the same name results in an error:

​	卷名称在驱动程序之间必须唯一。这意味着不能在两个不同的驱动程序中使用相同的卷名称。尝试创建两个同名卷将导致错误：



```console
A volume named  "hello"  already exists with the "some-other" driver. Choose a different volume name.
```

If you specify a volume name already in use on the current driver, Docker assumes you want to reuse the existing volume and doesn't return an error.

​	如果你指定的卷名称在当前驱动程序上已经使用，Docker 会假定你想重用现有卷，并不会返回错误。

### 驱动程序特定选项 (-o, `--opt`) Driver-specific options (-o, --opt)

Some volume drivers may take options to customize the volume creation. Use the `-o` or `--opt` flags to pass driver options:

​	某些卷驱动程序可能会接受选项以自定义卷创建。使用 `-o` 或 `--opt` 标志传递驱动程序选项：

```console
$ docker volume create --driver fake \
    --opt tardis=blue \
    --opt timey=wimey \
    foo
```

These options are passed directly to the volume driver. Options for different volume drivers may do different things (or nothing at all).

​	这些选项将直接传递给卷驱动程序。不同卷驱动程序的选项可能会有不同的功能（或根本没有作用）。

The built-in `local` driver accepts no options on Windows. On Linux and with Docker Desktop, the `local` driver accepts options similar to the Linux `mount` command. You can provide multiple options by passing the `--opt` flag multiple times. Some `mount` options (such as the `o` option) can take a comma-separated list of options. Complete list of available mount options can be found [here](https://man7.org/linux/man-pages/man8/mount.8.html).

​	内置的 `local` 驱动程序在 Windows 上不接受任何选项。在 Linux 和 Docker Desktop 上，`local` 驱动程序接受与 Linux `mount` 命令类似的选项。你可以通过多次传递 `--opt` 标志提供多个选项。一些 `mount` 选项（如 `o` 选项）可以接受逗号分隔的选项。可用的挂载选项的完整列表可以在 [这里](https://man7.org/linux/man-pages/man8/mount.8.html) 找到。

For example, the following creates a `tmpfs` volume called `foo` with a size of 100 megabyte and `uid` of 1000.

​	例如，以下命令创建一个名为 `foo` 的 `tmpfs` 卷，大小为 100MB，`uid` 为 1000：

```console
$ docker volume create --driver local \
    --opt type=tmpfs \
    --opt device=tmpfs \
    --opt o=size=100m,uid=1000 \
    foo
```

Another example that uses `btrfs`:

​	另一个使用 `btrfs` 的示例：

```console
$ docker volume create --driver local \
    --opt type=btrfs \
    --opt device=/dev/sda2 \
    foo
```

Another example that uses `nfs` to mount the `/path/to/dir` in `rw` mode from `192.168.1.1`:

​	使用 `nfs` 将 `/path/to/dir` 从 `192.168.1.1` 以 `rw` 模式挂载的示例：

```console
$ docker volume create --driver local \
    --opt type=nfs \
    --opt o=addr=192.168.1.1,rw \
    --opt device=:/path/to/dir \
    foo
```
