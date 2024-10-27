+++
title = "选择存储驱动程序"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/storage/drivers/select-storage-driver/](https://docs.docker.com/engine/storage/drivers/select-storage-driver/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Select a storage driver - 选择存储驱动程序

Ideally, very little data is written to a container's writable layer, and you use Docker volumes to write data. However, some workloads require you to be able to write to the container's writable layer. This is where storage drivers come in.

​	理想情况下，容器的可写层上只写入少量数据，并使用 Docker 卷来存储数据。然而，有些工作负载需要能够写入容器的可写层，此时存储驱动程序就派上用场了。

Docker supports several storage drivers, using a pluggable architecture. The storage driver controls how images and containers are stored and managed on your Docker host. After you have read the [storage driver overview]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}}), the next step is to choose the best storage driver for your workloads. Use the storage driver with the best overall performance and stability in the most usual scenarios.

​	Docker 使用可插拔架构支持多个存储驱动程序。存储驱动程序控制镜像和容器在 Docker 主机上的存储和管理方式。阅读 [存储驱动概述]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}}) 后，下一步是选择最适合您工作负载的存储驱动。使用在大多数情况下性能和稳定性最佳的存储驱动程序。

The Docker Engine provides the following storage drivers on Linux:

​	Docker Engine 在 Linux 上提供以下存储驱动：

| Driver            | Description                                                  |
| :---------------- | :----------------------------------------------------------- |
| `overlay2`        | `overlay2` is the preferred storage driver for all currently supported Linux distributions, and requires no extra configuration. `overlay2` 是所有当前支持的 Linux 发行版的首选存储驱动，不需要额外配置。 |
| `fuse-overlayfs`  | `fuse-overlayfs`is preferred only for running Rootless Docker on an old host that does not provide support for rootless `overlay2`. The `fuse-overlayfs` driver does not need to be used since Linux kernel 5.11, and `overlay2` works even in rootless mode. Refer to the [rootless mode documentation]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}}) for details. `fuse-overlayfs` 仅在不支持 rootless `overlay2` 的旧主机上运行 Rootless Docker 时首选。Linux 内核 5.11 后无需使用此驱动，`overlay2` 在 rootless 模式下也可用。参阅 [rootless 模式文档]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}}) 了解详情。 |
| `btrfs` and `zfs` | The `btrfs` and `zfs` storage drivers allow for advanced options, such as creating "snapshots", but require more maintenance and setup. Each of these relies on the backing filesystem being configured correctly. `btrfs` 和 `zfs` 支持高级选项，例如创建“快照”，但需要更多维护和设置。这些依赖于正确配置的底层文件系统。 |
| `vfs`             | The `vfs` storage driver is intended for testing purposes, and for situations where no copy-on-write filesystem can be used. Performance of this storage driver is poor, and is not generally recommended for production use. `vfs` 主要用于测试以及无法使用写时复制文件系统的情况。其性能较差，不推荐用于生产环境。 |

The Docker Engine has a prioritized list of which storage driver to use if no storage driver is explicitly configured, assuming that the storage driver meets the prerequisites, and automatically selects a compatible storage driver. You can see the order in the [source code for Docker Engine 27.2.1](https://github.com/moby/moby/blob/v27.2.1/daemon/graphdriver/driver_linux.go#L52-L53).

​	Docker Engine 有一个优先级列表，如果没有明确配置存储驱动且满足前提条件，它会自动选择兼容的存储驱动程序。请参阅 [Docker Engine 27.2.1 的源代码](https://github.com/moby/moby/blob/v27.2.1/daemon/graphdriver/driver_linux.go#L52-L53) 中的顺序。

Some storage drivers require you to use a specific format for the backing filesystem. If you have external requirements to use a specific backing filesystem, this may limit your choices. See [Supported backing filesystems](https://docs.docker.com/engine/storage/drivers/select-storage-driver/#supported-backing-filesystems).

​	有些存储驱动程序需要特定的底层文件系统格式。如果有外部要求使用特定的底层文件系统，可能会限制选择。请参阅 [支持的底层文件系统](https://docs.docker.com/engine/storage/drivers/select-storage-driver/#supported-backing-filesystems)。

After you have narrowed down which storage drivers you can choose from, your choice is determined by the characteristics of your workload and the level of stability you need. See [Other considerations](https://docs.docker.com/engine/storage/drivers/select-storage-driver/#other-considerations) for help in making the final decision.

​	确定了可选的存储驱动后，根据工作负载的特性和稳定性需求做出选择。参见[其他注意事项](https://docs.docker.com/engine/storage/drivers/select-storage-driver/#other-considerations)以帮助做出最终决定。

## 各 Linux 发行版支持的存储驱动 Supported storage drivers per Linux distribution

> **Note**
>
> 
>
> Modifying the storage driver by editing the daemon configuration file isn't supported on Docker Desktop. Only the default `overlay2` driver or the [containerd storage]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}}) are supported. The following table is also not applicable for the Docker Engine in rootless mode. For the drivers available in rootless mode, see the [Rootless mode documentation]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}}).
>
> ​	修改存储驱动的守护进程配置文件不支持 Docker Desktop。仅支持默认的 `overlay2` 驱动或 [containerd 存储]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}})。以下表格也不适用于 Docker Engine 的 rootless 模式。关于 rootless 模式下可用的驱动，请参阅 [rootless 模式文档]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}})。

Your operating system and kernel may not support every storage driver. For example, `btrfs` is only supported if your system uses `btrfs` as storage. In general, the following configurations work on recent versions of the Linux distribution:

​	您的操作系统和内核可能不支持所有存储驱动。例如，`btrfs` 仅在系统使用 `btrfs` 存储时支持。通常，以下配置在较新的 Linux 版本上有效：

| Linux distribution - Linux 发行版 | Recommended storage drivers 推荐存储驱动 | Alternative drivers 备选驱动 |
| :-------------------------------- | :--------------------------------------- | :--------------------------- |
| Ubuntu                            | `overlay2`                               | `zfs`, `vfs`                 |
| Debian                            | `overlay2`                               | `vfs`                        |
| CentOS                            | `overlay2`                               | `zfs`, `vfs`                 |
| Fedora                            | `overlay2`                               | `zfs`, `vfs`                 |
| SLES 15                           | `overlay2`                               | `vfs`                        |
| RHEL                              | `overlay2`                               | `vfs`                        |

When in doubt, the best all-around configuration is to use a modern Linux distribution with a kernel that supports the `overlay2` storage driver, and to use Docker volumes for write-heavy workloads instead of relying on writing data to the container's writable layer.

​	如有疑问，最佳配置是在支持 `overlay2` 存储驱动的现代 Linux 发行版和内核上运行，并使用 Docker 卷来处理写入密集型工作负载，而不是依赖于将数据写入容器的可写层。

The `vfs` storage driver is usually not the best choice, and primarily intended for debugging purposes in situations where no other storage-driver is supported. Before using the `vfs` storage driver, be sure to read about [its performance and storage characteristics and limitations]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/VFSstoragedriver" >}}).

​	通常情况下，`vfs` 存储驱动不是最佳选择，主要用于调试且无其他存储驱动可用的情况。使用 `vfs` 之前，请阅读[其性能和存储特性及限制]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/VFSstoragedriver" >}})。

The recommendations in the table above are known to work for a large number of users. If you use a recommended configuration and find a reproducible issue, it's likely to be fixed very quickly. If the driver that you want to use is not recommended according to this table, you can run it at your own risk. You can and should still report any issues you run into. However, such issues have a lower priority than issues encountered when using a recommended configuration.

​	表中的推荐配置已被广泛使用。如果使用推荐配置并遇到可复现的问题，通常会很快得到修复。如果想使用表中未推荐的驱动程序，可以自行承担风险使用，并报告任何遇到的问题。这类问题的优先级低于推荐配置的问题。

Depending on your Linux distribution, other storage-drivers, such as `btrfs` may be available. These storage drivers can have advantages for specific use-cases, but may require additional set-up or maintenance, which make them not recommended for common scenarios. Refer to the documentation for those storage drivers for details.

​	根据 Linux 发行版，可能有其他存储驱动可用，如 `btrfs`。这些驱动对特定用例可能有优势，但可能需要额外设置或维护，因此不推荐在通用场景中使用。详细信息请参考相关存储驱动文档。

## 支持的底层文件系统 Supported backing filesystems

With regard to Docker, the backing filesystem is the filesystem where `/var/lib/docker/` is located. Some storage drivers only work with specific backing filesystems.

​	对于 Docker，底层文件系统是 `/var/lib/docker/` 所在的文件系统。有些存储驱动仅支持特定的底层文件系统。

| Storage driver 存储驱动 | Supported backing filesystems 支持的底层文件系统 |
| :---------------------- | :----------------------------------------------- |
| `overlay2`              | `xfs` with ftype=1, `ext4`                       |
| `fuse-overlayfs`        | any filesystem 任意文件系统                      |
| `btrfs`                 | `btrfs`                                          |
| `zfs`                   | `zfs`                                            |
| `vfs`                   | any filesystem 任意文件系统                      |

## 其他注意事项 Other considerations

### 适合您的工作负载 Suitability for your workload

Among other things, each storage driver has its own performance characteristics that make it more or less suitable for different workloads. Consider the following generalizations:

​	每个存储驱动有其性能特性，适合不同的工作负载。请考虑以下一般性说明：

- `overlay2` operates at the file level rather than the block level. This uses memory more efficiently, but the container's writable layer may grow quite large in write-heavy workloads.
  - `overlay2` 在文件级别操作而非块级别，内存利用率更高，但在写入密集型工作负载中容器的可写层可能会变得很大。

- Block-level storage drivers such as `btrfs`, and `zfs` perform better for write-heavy workloads (though not as well as Docker volumes).
  - 块级存储驱动如 `btrfs` 和 `zfs` 在写入密集型工作负载中性能较佳（虽然不如 Docker 卷）。

- `btrfs` and `zfs` require a lot of memory.
  - `btrfs` 和 `zfs` 需要较多内存。

- `zfs` is a good choice for high-density workloads such as PaaS.
  - `zfs` 是高密度工作负载（如 PaaS）的不错选择。


More information about performance, suitability, and best practices is available in the documentation for each storage driver.

​	关于性能、适用性和最佳实践的更多信息请参阅各存储驱动文档。

### 共享存储系统与存储驱动 Shared storage systems and the storage driver

If you use SAN, NAS, hardware RAID, or other shared storage systems, those systems may provide high availability, increased performance, thin provisioning, deduplication, and compression. In many cases, Docker can work on top of these storage systems, but Docker doesn't closely integrate with them.

​	如果使用 SAN、NAS、硬件 RAID 或其他共享存储系统，这些系统可能提供高可用性、性能提升、精简配置、重复数据删除和压缩。在许多情况下，Docker 可以在这些存储系统上工作，但 Docker 并未与之紧密集成。

Each Docker storage driver is based on a Linux filesystem or volume manager. Be sure to follow existing best practices for operating your storage driver (filesystem or volume manager) on top of your shared storage system. For example, if using the ZFS storage driver on top of a shared storage system, be sure to follow best practices for operating ZFS filesystems on top of that specific shared storage system.

​	每个 Docker 存储驱动基于 Linux 文件系统或卷管理器。请确保在共享存储系统上操作存储驱动（文件系统或卷管理器）时遵循现有最佳实践。例如，在共享存储系统上使用 ZFS 存储驱动时，请遵循该共享存储系统上运行 ZFS 文件系统的最佳实践。

### 稳定性 Stability

For some users, stability is more important than performance. Though Docker considers all of the storage drivers mentioned here to be stable, some are newer and are still under active development. In general, `overlay2` provides the highest stability.

​	对一些用户而言，稳定性比性能更重要。尽管 Docker 认为此处提到的存储驱动均为稳定的，但有些较新，仍在积极开发中。总体而言，`overlay2` 提供最高的稳定性。

### 使用您的工作负载进行测试 Test with your own workloads

You can test Docker's performance when running your own workloads on different storage drivers. Make sure to use equivalent hardware and workloads to match production conditions, so you can see which storage driver offers the best overall performance.

​	可以通过在不同存储驱动上运行实际工作负载来测试 Docker 的性能。确保使用与生产环境相同的硬件和工作负载，以便观察哪个存储驱动提供最佳的综合性能。

## 检查当前的存储驱动 Check your current storage driver

The detailed documentation for each individual storage driver details all of the set-up steps to use a given storage driver.

​	每个存储驱动的详细文档中详细介绍了使用该驱动的所有设置步骤。

To see what storage driver Docker is currently using, use `docker info` and look for the `Storage Driver` line:

​	要查看 Docker 当前使用的存储驱动，请使用 `docker info` 并查找 `Storage Driver` 行：

```console
$ docker info

Containers: 0
Images: 0
Storage Driver: overlay2
 Backing Filesystem: xfs
<...>
```

To change the storage driver, see the specific instructions for the new storage driver. Some drivers require additional configuration, including configuration to physical or logical disks on the Docker host.

​	要更改存储驱动，请参阅新存储驱动的具体说明。有些驱动需要额外配置，包括 Docker 主机上的物理或逻辑磁盘配置。

> **Important**
>
> 
>
> When you change the storage driver, any existing images and containers become inaccessible. This is because their layers can't be used by the new storage driver. If you revert your changes, you can access the old images and containers again, but any that you pulled or created using the new driver are then inaccessible.
>
> ​	更改存储驱动后，所有现有镜像和容器将不可访问，因为新驱动无法使用其层。如果还原更改，可以再次访问旧镜像和容器，但使用新驱动拉取或创建的任何镜像和容器将不可访问。

## 相关信息 Related information

- [About images, containers, and storage drivers]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}})
  - [关于镜像、容器和存储驱动]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}})
- [`overlay2` storage driver in practice]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/OverlayFSstoragedriver" >}})
  - [`overlay2` 存储驱动的实践]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/OverlayFSstoragedriver" >}})
- [`btrfs` storage driver in practice]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/BTRFSstoragedriver" >}})
  - [`btrfs` 存储驱动的实践]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/BTRFSstoragedriver" >}})
- [`zfs` storage driver in practice]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/ZFSstoragedriver" >}})
  - [`zfs` 存储驱动的实践]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/ZFSstoragedriver" >}})
