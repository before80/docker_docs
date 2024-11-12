+++
title = "管理构建器"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/builders/manage/](https://docs.docker.com/build/builders/manage/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Manage builders - 管理构建器

You can create, inspect, and manage builders using `docker buildx` commands, or [using Docker Desktop](https://docs.docker.com/build/builders/manage/#manage-builders-with-docker-desktop).

​	您可以使用 `docker buildx` 命令创建、检查和管理构建器，或者[使用 Docker Desktop](https://docs.docker.com/build/builders/manage/#manage-builders-with-docker-desktop)进行管理。

## 创建一个新的构建器 Create a new builder

The default builder uses the [`docker` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}}). You can't manually create new `docker` builders, but you can create builders that use other drivers, such as the [`docker-container` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}}), which runs the BuildKit daemon in a container.

​	默认构建器使用 [`docker` 驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}})。您无法手动创建新的 `docker` 构建器，但可以创建使用其他驱动的构建器，例如 [`docker-container` 驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}})，它在容器中运行 BuildKit 守护进程。

Use the [`docker buildx create`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxcreate" >}}) command to create a builder.

​	使用 [`docker buildx create`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxcreate" >}}) 命令来创建一个构建器。

```console
$ docker buildx create --name=<builder-name>
```

Buildx uses the `docker-container` driver by default if you omit the `--driver` flag. For more information about available drivers, see [Build drivers]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}).

​	如果省略 `--driver` 标志，Buildx 默认使用 `docker-container` 驱动。有关可用驱动的更多信息，请参阅[构建驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}})。

## 列出可用的构建器 List available builders

Use `docker buildx ls` to see builder instances available on your system, and the drivers they're using.

​	使用 `docker buildx ls` 查看系统上可用的构建器实例及其使用的驱动。



```console
$ docker buildx ls
NAME/NODE       DRIVER/ENDPOINT      STATUS   BUILDKIT PLATFORMS
default *       docker
  default       default              running  v0.11.6  linux/amd64, linux/amd64/v2, linux/amd64/v3, linux/386
my_builder      docker-container
  my_builder0   default              running  v0.11.6  linux/amd64, linux/amd64/v2, linux/amd64/v3, linux/386
```

The asterisk (`*`) next to the builder name indicates the [selected builder](https://docs.docker.com/build/builders/#selected-builder).

​	构建器名称旁的星号 (`*`) 表示[选中的构建器](https://docs.docker.com/build/builders/#selected-builder)。

## Inspect a builder

To inspect a builder with the CLI, use `docker buildx inspect <name>`. You can only inspect a builder if the builder is active. You can add the `--bootstrap` flag to the command to start the builder.

​	使用 `docker buildx inspect <name>` 来检查构建器。只有激活的构建器可以被检查。可以添加 `--bootstrap` 标志来启动构建器。



```console
$ docker buildx inspect --bootstrap my_builder
[+] Building 1.7s (1/1) FINISHED                                                                  
 => [internal] booting buildkit                                                              1.7s
 => => pulling image moby/buildkit:buildx-stable-1                                           1.3s
 => => creating container buildx_buildkit_my_builder0                                        0.4s
Name:          my_builder
Driver:        docker-container
Last Activity: 2023-06-21 18:28:37 +0000 UTC

Nodes:
Name:      my_builder0
Endpoint:  unix:///var/run/docker.sock
Status:    running
Buildkit:  v0.11.6
Platforms: linux/arm64, linux/amd64, linux/amd64/v2, linux/riscv64, linux/ppc64le, linux/s390x, linux/386, linux/mips64le, linux/mips64, linux/arm/v7, linux/arm/v6
```

If you want to see how much disk space a builder is using, use the `docker buildx du` command. By default, this command shows the total disk usage for all available builders. To see usage for a specific builder, use the `--builder` flag.

​	如果想查看构建器的磁盘空间使用情况，可以使用 `docker buildx du` 命令。默认情况下，该命令显示所有可用构建器的总磁盘使用情况。要查看特定构建器的使用情况，请使用 `--builder` 标志。



```console
$ docker buildx du --builder my_builder
ID                                        RECLAIMABLE SIZE        LAST ACCESSED
olkri5gq6zsh8q2819i69aq6l                 true        797.2MB     37 seconds ago
6km4kasxgsywxkm6cxybdumbb*                true        438.5MB     36 seconds ago
qh3wwwda7gx2s5u4hsk0kp4w7                 true        213.8MB     37 seconds ago
54qq1egqem8max3lxq6180cj8                 true        200.2MB     37 seconds ago
ndlp969ku0950bmrw9muolw0c*                true        116.7MB     37 seconds ago
u52rcsnfd1brwc0chwsesb3io*                true        116.7MB     37 seconds ago
rzoeay0s4nmss8ub59z6lwj7d                 true        46.25MB     4 minutes ago
itk1iibhmv7awmidiwbef633q                 true        33.33MB     37 seconds ago
4p78yqnbmgt6xhcxqitdieeln                 true        19.46MB     4 minutes ago
dgkjvv4ay0szmr9bl7ynla7fy*                true        19.24MB     36 seconds ago
tuep198kmcw299qc9e4d1a8q2                 true        8.663MB     4 minutes ago
n1wzhauk9rpmt6ib1es7dktvj                 true        20.7kB      4 minutes ago
0a2xfhinvndki99y69157udlm                 true        16.56kB     37 seconds ago
gf0z1ypz54npfererqfeyhinn                 true        16.38kB     37 seconds ago
nz505f12cnsu739dw2pw0q78c                 true        8.192kB     37 seconds ago
hwpcyq5hdfvioltmkxu7fzwhb*                true        8.192kB     37 seconds ago
acekq89snc7j6im1rjdizvsg1*                true        8.192kB     37 seconds ago
Reclaimable:  2.01GB
Total:        2.01GB
```

## 移除构建器 Remove a builder

Use the [`docker buildx remove`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxcreate" >}}) command to remove a builder.

​	使用 [`docker buildx remove`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxcreate" >}}) 命令来移除构建器。



```console
$ docker buildx rm <builder-name>
```

If you remove your currently selected builder, the default `docker` builder is automatically selected. You can't remove the default builder.

​	如果移除了当前选中的构建器，默认的 `docker` 构建器会被自动选中。无法移除默认构建器。

Local build cache for the builder is also removed.

​	构建器的本地构建缓存也会被移除。

### 移除远程构建器 Removing remote builders

Removing a remote builder doesn't affect the remote build cache. It also doesn't stop the remote BuildKit daemon. It only removes your connection to the builder.

​	移除远程构建器不会影响远程构建缓存，也不会停止远程 BuildKit 守护进程。仅会删除与构建器的连接。

## 使用 Docker Desktop 管理构建器 Manage builders with Docker Desktop

If you have turned on the [Docker Desktop Builds view]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Builds" >}}), you can inspect builders in [Docker Desktop settings](https://docs.docker.com/desktop/settings/#builders).

​	如果已开启 [Docker Desktop 构建视图]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Builds" >}})，可以在 [Docker Desktop 设置]({{< ref "/manuals/DockerDesktop/Changesettings#构建器-builders">}})中检查构建器。
