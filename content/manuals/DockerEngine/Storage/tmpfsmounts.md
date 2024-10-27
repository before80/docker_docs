+++
title = "tmpfs mounts"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/storage/tmpfs/](https://docs.docker.com/engine/storage/tmpfs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# tmpfs mounts - tmpfs 挂载

[Volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) and [bind mounts]({{< ref "/manuals/DockerEngine/Storage/Bindmounts" >}}) let you share files between the host machine and container so that you can persist data even after the container is stopped.

​	[卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})和[绑定挂载]({{< ref "/manuals/DockerEngine/Storage/Bindmounts" >}})允许在主机和容器之间共享文件，因此即使容器停止后也可以持久化数据。

If you're running Docker on Linux, you have a third option: `tmpfs` mounts. When you create a container with a `tmpfs` mount, the container can create files outside the container's writable layer.

​	如果在 Linux 上运行 Docker，你可以选择第三种选项：`tmpfs` 挂载。当你创建带有 `tmpfs` 挂载的容器时，容器可以在其可写层之外创建文件。

As opposed to volumes and bind mounts, a `tmpfs` mount is temporary, and only persisted in the host memory. When the container stops, the `tmpfs` mount is removed, and files written there won't be persisted.

​	与卷和绑定挂载不同，`tmpfs` 挂载是临时的，仅在主机内存中保留。当容器停止时，`tmpfs` 挂载被移除，写入其中的文件不会持久化。

![tmpfs on the Docker host](tmpfsmounts_img/types-of-mounts-tmpfs.webp)

This is useful to temporarily store sensitive files that you don't want to persist in either the host or the container writable layer.

​	这对于临时存储敏感文件很有用，因为你可能不希望这些文件在主机或容器的可写层中持久保存。

## tmpfs 挂载的限制 Limitations of tmpfs mounts

- Unlike volumes and bind mounts, you can't share `tmpfs` mounts between containers.
  - 与卷和绑定挂载不同，`tmpfs` 挂载无法在多个容器之间共享。

- This functionality is only available if you're running Docker on Linux.
  - 此功能仅在 Linux 上运行的 Docker 可用。

- Setting permissions on tmpfs may cause them to [reset after container restart](https://github.com/docker/for-linux/issues/138). In some cases [setting the uid/gid](https://github.com/docker/compose/issues/3425#issuecomment-423091370) can serve as a workaround.
  - 设置 tmpfs 权限可能会导致[在容器重启后重置](https://github.com/docker/for-linux/issues/138)。在某些情况下，可以[设置 uid/gid](https://github.com/docker/compose/issues/3425#issuecomment-423091370)来解决。

## Choose the --tmpfs or --mount flag

In general, `--mount` is more explicit and verbose. The biggest difference is that the `--tmpfs` flag does not support any configurable options.

​	一般来说，`--mount` 更加明确和详尽。两者的主要区别在于 `--tmpfs` 标志不支持任何可配置选项。

- `--tmpfs`: Mounts a `tmpfs` mount without allowing you to specify any configurable options, and can only be used with standalone containers.
  - `--tmpfs`：挂载 `tmpfs`，但不允许指定任何可配置选项，仅用于独立容器。

- `--mount`: Consists of multiple key-value pairs, separated by commas and each consisting of a `<key>=<value>` tuple. The `--mount` syntax is more verbose than `--tmpfs`: `--mount`：包含多个键值对，用逗号分隔，每个由 `<key>=<value>` 组成。`--mount` 语法比 `--tmpfs` 更详细：
  - The `type` of the mount, which can be [`bind`]({{< ref "/manuals/DockerEngine/Storage/Bindmounts" >}}), `volume`, or [`tmpfs`]({{< ref "/manuals/DockerEngine/Storage/tmpfsmounts" >}}). This topic discusses `tmpfs`, so the type is always `tmpfs`.
    - `type`：挂载类型，可为[`bind`]({{< ref "/manuals/DockerEngine/Storage/Bindmounts" >}})、`volume` 或 [`tmpfs`]({{< ref "/manuals/DockerEngine/Storage/tmpfsmounts" >}})。此处使用 `tmpfs`。
  - The `destination` takes as its value the path where the `tmpfs` mount is mounted in the container. May be specified as `destination`, `dst`, or `target`.
    - `destination`：tmpfs 挂载在容器中的路径，可使用 `destination`、`dst` 或 `target`。
  - The `tmpfs-size` and `tmpfs-mode` options. See [tmpfs options](https://docs.docker.com/engine/storage/tmpfs/#specify-tmpfs-options).
    - `tmpfs-size` 和 `tmpfs-mode` 选项。详见 [tmpfs 选项](https://docs.docker.com/engine/storage/tmpfs/#specify-tmpfs-options)。

The examples below show both the `--mount` and `--tmpfs` syntax where possible, and `--mount` is presented first.

​	以下示例展示了 `--mount` 和 `--tmpfs` 语法的用法，首选 `--mount`。

### Differences between `--tmpfs` and `--mount` behavior

- The `--tmpfs` flag does not allow you to specify any configurable options.
  - `--tmpfs` 标志不允许指定任何可配置选项。

- The `--tmpfs` flag cannot be used with swarm services. You must use `--mount`.
  - `--tmpfs` 标志不能与 swarm 服务一起使用。必须使用 `--mount`。

## 在容器中使用 tmpfs 挂载 Use a tmpfs mount in a container

To use a `tmpfs` mount in a container, use the `--tmpfs` flag, or use the `--mount` flag with `type=tmpfs` and `destination` options. There is no `source` for `tmpfs` mounts. The following example creates a `tmpfs` mount at `/app` in a Nginx container. The first example uses the `--mount` flag and the second uses the `--tmpfs` flag.

​	在容器中使用 `tmpfs` 挂载，可以使用 `--tmpfs` 标志，或使用 `--mount` 标志并设置 `type=tmpfs` 和 `destination` 选项。`tmpfs` 挂载没有 `source`。以下示例在 Nginx 容器中创建了一个挂载在 `/app` 的 `tmpfs`。第一个示例使用 `--mount` 标志，第二个使用 `--tmpfs`。

{{< tabpane text=true persist=disabled >}}

{{% tab header="`--mount`" %}}

```console
 docker run -d \
  -it \
  --name tmptest \
  --mount type=tmpfs,destination=/app \
  nginx:latest
```

{{% /tab  %}}

{{% tab header="`--tmpfs`" %}}

```console
 docker run -d \
  -it \
  --name tmptest \
  --tmpfs /app \
  nginx:latest
```

{{% /tab  %}}

{{< /tabpane >}}

------

Verify that the mount is a `tmpfs` mount by looking in the `Mounts` section of the `docker inspect` output:

​	通过查看 `docker inspect` 输出中的 `Mounts` 部分，验证挂载是否为 `tmpfs` 类型：

```console
$ docker inspect tmptest --format '{{ json .Mounts }}'
[{"Type":"tmpfs","Source":"","Destination":"/app","Mode":"","RW":true,"Propagation":""}]
```

Stop and remove the container:

​	停止并删除容器：

```console
$ docker stop tmptest
$ docker rm tmptest
```

### 指定 tmpfs 选项 Specify tmpfs options

`tmpfs` mounts allow for two configuration options, neither of which is required. If you need to specify these options, you must use the `--mount` flag, as the `--tmpfs` flag does not support them.

​	`tmpfs` 挂载支持两个配置选项，均为可选。如果需要指定这些选项，必须使用 `--mount` 标志，因为 `--tmpfs` 不支持它们。

| Option       | Description                                                  |
| :----------- | :----------------------------------------------------------- |
| `tmpfs-size` | Size of the tmpfs mount in bytes. If unset, the default maximum size of a tmpfs volume is 50% of the host's total RAM. tmpfs 挂载的大小（以字节为单位）。默认最大大小为主机总 RAM 的 50%。 |
| `tmpfs-mode` | File mode of the tmpfs in octal. For instance, `700` or `0770`. Defaults to `1777` or world-writable. tmpfs 的文件模式，采用八进制格式。例如 `700` 或 `0770`。默认为 `1777`。 |

The following example sets the `tmpfs-mode` to `1770`, so that it is not world-readable within the container.

​	以下示例将 `tmpfs-mode` 设置为 `1770`，使其在容器中不可被全局读取。



```console
docker run -d \
  -it \
  --name tmptest \
  --mount type=tmpfs,destination=/app,tmpfs-mode=1770 \
  nginx:latest
```

## 接下来 Next steps

- Learn about [volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})
  - 了解更多关于[卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})的信息。
- Learn about [bind mounts]({{< ref "/manuals/DockerEngine/Storage/Bindmounts" >}})
  - 了解更多关于[绑定挂载]({{< ref "/manuals/DockerEngine/Storage/Bindmounts" >}})的信息。
- Learn about [storage drivers]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}})
  - 了解更多关于[存储驱动]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}})的信息。
