+++
title = "containerd 镜像存储"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/storage/containerd/](https://docs.docker.com/engine/storage/containerd/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# containerd image store with Docker Engine - containerd 镜像存储与 Docker Engine

> **Note**
>
> 
>
> The containerd image store is an experimental feature of Docker Engine. If you're using Docker Desktop, refer to the instructions on the [containerd image store with Docker Desktop page]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}}).
>
> ​	containerd 镜像存储是 Docker Engine的实验性功能。如果您使用的是 Docker Desktop，请参考 [Docker Desktop 上的 containerd 镜像存储页面]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}})中的说明。

containerd, the industry-standard container runtime, uses snapshotters instead of the classic storage drivers for storing image and container data. While the `overlay2` driver still remains the default driver for Docker Engine, you can opt in to using containerd snapshotters as an experimental feature.

​	containerd 是业界标准的容器运行时，它使用快照器（snapshotter）而不是传统的存储驱动来存储镜像和容器数据。虽然 `overlay2` 驱动仍然是 Docker Engine的默认驱动，但您可以选择启用 containerd 快照器作为实验性功能。

To learn more about the containerd image store and its benefits, refer to [containerd image store on Docker Desktop]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}}).

​	要了解有关 containerd 镜像存储及其优势的更多信息，请参阅 [Docker Desktop 上的 containerd 镜像存储]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}})。

## 在 Docker Engine上启用 containerd 镜像存储 Enable containerd image store on Docker Engine

Switching to containerd snapshotters causes you to temporarily lose images and containers created using the classic storage drivers. Those resources still exist on your filesystem, and you can retrieve them by turning off the containerd snapshotters feature.

​	切换到 containerd 快照器会导致您暂时无法使用经典存储驱动创建的镜像和容器。这些资源仍然存在于您的文件系统中，您可以通过关闭 containerd 快照器功能来找回它们。

The following steps explain how to enable the containerd snapshotters feature.

​	以下步骤说明了如何启用 containerd 快照器功能：

1. Add the following configuration to your `/etc/docker/daemon.json` configuration file:

   将以下配置添加到您的 `/etc/docker/daemon.json` 配置文件中：

   ```json
   {
     "features": {
       "containerd-snapshotter": true
     }
   }
   ```

2. Save the file. 保存文件。

3. Restart the daemon for the changes to take effect.

   重启守护进程以使更改生效。

   ```console
   $ sudo systemctl restart docker
   ```

After restarting the daemon, running `docker info` shows that you're using containerd snapshotter storage drivers.

​	重启守护进程后，运行 `docker info` 将显示您正在使用 containerd 快照存储驱动。

```console
$ docker info -f '{{ .DriverStatus }}'
[[driver-type io.containerd.snapshotter.v1]]
```

Docker Engine uses the `overlayfs` containerd snapshotter by default.

​	Docker Engine默认使用 `overlayfs` 作为 containerd 快照器。
