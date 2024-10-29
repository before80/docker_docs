+++
title = "清理未使用的 Docker 对象"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/manage-resources/pruning/](https://docs.docker.com/engine/manage-resources/pruning/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Prune unused Docker objects - 清理未使用的 Docker 对象

Docker takes a conservative approach to cleaning up unused objects (often referred to as "garbage collection"), such as images, containers, volumes, and networks. These objects are generally not removed unless you explicitly ask Docker to do so. This can cause Docker to use extra disk space. For each type of object, Docker provides a `prune` command. In addition, you can use `docker system prune` to clean up multiple types of objects at once. This topic shows how to use these `prune` commands.

​	Docker 采取保守的方法来清理未使用的对象（通常称为“垃圾回收”），例如镜像、容器、卷和网络。这些对象通常不会被删除，除非您明确要求 Docker 这样做。这可能会导致 Docker 使用额外的磁盘空间。对于每种类型的对象，Docker 提供了一个 `prune` 命令。此外，您可以使用 `docker system prune` 一次清理多种类型的对象。本主题展示了如何使用这些 `prune` 命令。

## 清理镜像 Prune images

The `docker image prune` command allows you to clean up unused images. By default, `docker image prune` only cleans up *dangling* images. A dangling image is one that isn't tagged, and isn't referenced by any container. To remove dangling images:

​	`docker image prune` 命令允许您清理未使用的镜像。默认情况下，`docker image prune` 只会清理 *悬空* 镜像。悬空镜像是未被标记且未被任何容器引用的镜像。要删除悬空镜像：



```console
$ docker image prune

WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N] y
```

To remove all images which aren't used by existing containers, use the `-a` flag:

​	要删除所有未被现有容器使用的镜像，使用 `-a` 标志：



```console
$ docker image prune -a

WARNING! This will remove all images without at least one container associated to them.
Are you sure you want to continue? [y/N] y
```

By default, you are prompted to continue. To bypass the prompt, use the `-f` or `--force` flag.

​	默认情况下，系统会提示您继续。要跳过提示，使用 `-f` 或 `--force` 标志。

You can limit which images are pruned using filtering expressions with the `--filter` flag. For example, to only consider images created more than 24 hours ago:

​	您可以使用 `--filter` 标志添加过滤表达式以限制清理的镜像范围。例如，仅删除 24 小时前创建的镜像：

```console
$ docker image prune -a --filter "until=24h"
```

Other filtering expressions are available. See the [`docker image prune` reference]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimageprune" >}}) for more examples.

​	其他过滤表达式也可用。请参阅 `docker image prune` 参考 以获取更多示例。

## 清理容器 Prune containers

When you stop a container, it isn't automatically removed unless you started it with the `--rm` flag. To see all containers on the Docker host, including stopped containers, use `docker ps -a`. You may be surprised how many containers exist, especially on a development system! A stopped container's writable layers still take up disk space. To clean this up, you can use the `docker container prune` command.

​	当您停止容器时，除非使用 `--rm` 标志启动容器，否则容器不会自动删除。要查看 Docker 主机上的所有容器（包括已停止的容器），可以使用 `docker ps -a`。您可能会惊讶于存在这么多容器，尤其是在开发系统上！停止的容器的可写层仍然占用磁盘空间。要清理这些空间，可以使用 `docker container prune` 命令。

```console
$ docker container prune

WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
```

By default, you're prompted to continue. To bypass the prompt, use the `-f` or `--force` flag.

​	默认情况下，系统会提示您继续。要跳过提示，使用 `-f` 或 `--force` 标志。

By default, all stopped containers are removed. You can limit the scope using the `--filter` flag. For instance, the following command only removes stopped containers older than 24 hours:

​	默认情况下，所有已停止的容器都会被删除。您可以使用 `--filter` 标志限制范围。例如，以下命令仅删除停止超过 24 小时的容器：

```console
$ docker container prune --filter "until=24h"
```

Other filtering expressions are available. See the [`docker container prune` reference]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerprune" >}}) for more examples.

​	其他过滤表达式也可用。请参阅 `docker container prune` 参考 以获取更多示例。

## 清理卷 Prune volumes

Volumes can be used by one or more containers, and take up space on the Docker host. Volumes are never removed automatically, because to do so could destroy data.

​	卷可以被一个或多个容器使用，占用 Docker 主机上的空间。卷不会自动删除，因为这样做可能会破坏数据。

```console
$ docker volume prune

WARNING! This will remove all volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
```

By default, you are prompted to continue. To bypass the prompt, use the `-f` or `--force` flag.

​	默认情况下，系统会提示您继续。要跳过提示，使用 `-f` 或 `--force` 标志。

By default, all unused volumes are removed. You can limit the scope using the `--filter` flag. For instance, the following command only removes volumes which aren't labelled with the `keep` label:

​	默认情况下，所有未使用的卷都会被删除。您可以使用 `--filter` 标志限制范围。例如，以下命令仅删除未带有 `keep` 标签的卷：

```console
$ docker volume prune --filter "label!=keep"
```

Other filtering expressions are available. See the [`docker volume prune` reference]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumeprune" >}}) for more examples.

​	其他过滤表达式也可用。请参阅 `docker volume prune` 参考 以获取更多示例。

## 清理网络 Prune networks

Docker networks don't take up much disk space, but they do create `iptables` rules, bridge network devices, and routing table entries. To clean these things up, you can use `docker network prune` to clean up networks which aren't used by any containers.

​	Docker 网络不会占用太多磁盘空间，但它们确实会创建 `iptables` 规则、桥接网络设备和路由表条目。要清理这些内容，您可以使用 `docker network prune` 来清理未被任何容器使用的网络。

```console
$ docker network prune

WARNING! This will remove all networks not used by at least one container.
Are you sure you want to continue? [y/N] y
```

By default, you're prompted to continue. To bypass the prompt, use the `-f` or `--force` flag.

​	默认情况下，系统会提示您继续。要跳过提示，使用 `-f` 或 `--force` 标志。

By default, all unused networks are removed. You can limit the scope using the `--filter` flag. For instance, the following command only removes networks older than 24 hours:

​	默认情况下，所有未使用的网络都会被删除。您可以使用 `--filter` 标志限制范围。例如，以下命令仅删除超过 24 小时的网络：

```console
$ docker network prune --filter "until=24h"
```

Other filtering expressions are available. See the [`docker network prune` reference]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkprune" >}}) for more examples.

​	其他过滤表达式也可用。请参阅 `docker network prune` 参考 以获取更多示例。

## 清理所有对象 Prune everything

The `docker system prune` command is a shortcut that prunes images, containers, and networks. Volumes aren't pruned by default, and you must specify the `--volumes` flag for `docker system prune` to prune volumes.

​	`docker system prune` 命令是一个清理镜像、容器和网络的快捷方式。默认情况下，不会清理卷，您必须指定 `--volumes` 标志才能清理卷。



```console
$ docker system prune

WARNING! This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all dangling images
        - unused build cache

Are you sure you want to continue? [y/N] y
```

To also prune volumes, add the `--volumes` flag:

​	要同时清理卷，请添加 `--volumes` 标志：



```console
$ docker system prune --volumes

WARNING! This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all volumes not used by at least one container
        - all dangling images
        - all build cache

Are you sure you want to continue? [y/N] y
```

By default, you're prompted to continue. To bypass the prompt, use the `-f` or `--force` flag.

​	默认情况下，系统会提示您继续。要跳过提示，使用 `-f` 或 `--force` 标志。

By default, all unused containers, networks, and images are removed. You can limit the scope using the `--filter` flag. For instance, the following command removes items older than 24 hours:

​	默认情况下，所有未使用的容器、网络和镜像都会被删除。您可以使用 `--filter` 标志限制范围。例如，以下命令删除超过 24 小时的项：



```console
$ docker system prune --filter "until=24h"
```

Other filtering expressions are available. See the [`docker system prune` reference]({{< ref "/reference/CLIreference/docker/dockersystem/dockersystemprune" >}}) r more examples.

​	其他过滤表达式也可用。请参阅 [`docker system prune` 参考]({{< ref "/reference/CLIreference/docker/dockersystem/dockersystemprune" >}}) 以获取更多示例。
