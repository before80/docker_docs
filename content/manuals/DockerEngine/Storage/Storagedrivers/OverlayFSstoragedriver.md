+++
title = "OverlayFS 存储驱动"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/storage/drivers/overlayfs-driver/](https://docs.docker.com/engine/storage/drivers/overlayfs-driver/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# OverlayFS storage driver - OverlayFS 存储驱动

OverlayFS is a union filesystem.

​	OverlayFS 是一种联合文件系统。

This page refers to the Linux kernel driver as `OverlayFS` and to the Docker storage driver as `overlay2`.

​	本文将 Linux 内核驱动称为 `OverlayFS`，将 Docker 存储驱动称为 `overlay2`。

> **Note**
>
> 
>
> For `fuse-overlayfs` driver, check [Rootless mode documentation]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}}).
>
> ​	对于 `fuse-overlayfs` 驱动，请查阅[Rootless 模式文档]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}})。

## 前置条件 Prerequisites

OverlayFS is the recommended storage driver, and supported if you meet the following prerequisites:

​	OverlayFS 是推荐的存储驱动，在满足以下前提条件时支持：

- Version 4.0 or higher of the Linux kernel, or RHEL or CentOS using version 3.10.0-514 of the kernel or higher.

  - Linux 内核 4.0 及以上版本，或使用 3.10.0-514 及以上内核版本的 RHEL 或 CentOS。

- The `overlay2` driver is supported on `xfs` backing filesystems, but only with `d_type=true` enabled.

  - `overlay2` 驱动支持 `xfs` 文件系统，但必须启用 `d_type=true`。


  Use `xfs_info` to verify that the `ftype` option is set to `1`. To format an `xfs` filesystem correctly, use the flag `-n ftype=1`.

  ​	使用 `xfs_info` 验证 `ftype` 选项是否设置为 `1`。要正确格式化 `xfs` 文件系统，请使用 `-n ftype=1` 标志。

- Changing the storage driver makes existing containers and images inaccessible on the local system. Use `docker save` to save any images you have built or push them to Docker Hub or a private registry before changing the storage driver, so that you don't need to re-create them later.

  - 更改存储驱动会导致现有容器和镜像在本地系统上不可访问。更改存储驱动前，使用 `docker save` 保存您已构建的镜像或将其推送到 Docker Hub 或私有仓库，以便之后无需重新创建。

  


## 使用 `overlay2` 存储驱动配置 Docker Configure Docker with the `overlay2` storage driver



Before following this procedure, you must first meet all the [prerequisites](https://docs.docker.com/engine/storage/drivers/overlayfs-driver/#prerequisites).

​	在执行此操作之前，您必须先满足所有[前提条件](https://docs.docker.com/engine/storage/drivers/overlayfs-driver/#prerequisites)。

The following steps outline how to configure the `overlay2` storage driver.

​	以下步骤概述了如何配置 `overlay2` 存储驱动。

1. Stop Docker. 停止 Docker。

   

   ```console
   $ sudo systemctl stop docker
   ```

2. Copy the contents of `/var/lib/docker` to a temporary location. 将 `/var/lib/docker` 的内容复制到临时位置。

   

   ```console
   $ cp -au /var/lib/docker /var/lib/docker.bk
   ```

3. If you want to use a separate backing filesystem from the one used by `/var/lib/`, format the filesystem and mount it into `/var/lib/docker`. Make sure to add this mount to `/etc/fstab` to make it permanent. 如果要使用与 `/var/lib/` 不同的文件系统作为后备文件系统，请格式化该文件系统并将其挂载到 `/var/lib/docker`。确保将该挂载项添加到 `/etc/fstab` 以使其在重启后依然有效。

4. Edit `/etc/docker/daemon.json`. If it doesn't yet exist, create it. Assuming that the file was empty, add the following contents. 编辑 `/etc/docker/daemon.json`。如果该文件不存在，请创建它。如果文件为空，添加以下内容。

   

   ```json
   {
     "storage-driver": "overlay2"
   }
   ```

   Docker doesn't start if the `daemon.json` file contains invalid JSON.

   ​	如果 `daemon.json` 文件包含无效的 JSON，则 Docker 不会启动。

5. Start Docker. 启动 Docker。

   

   ```console
   $ sudo systemctl start docker
   ```

6. Verify that the daemon is using the `overlay2` storage driver. Use the `docker info` command and look for `Storage Driver` and `Backing filesystem`. 验证守护进程是否使用 `overlay2` 存储驱动。使用 `docker info` 命令并查找 `Storage Driver` 和 `Backing filesystem`。

   

   ```console
   $ docker info
   
   Containers: 0
   Images: 0
   Storage Driver: overlay2
    Backing Filesystem: xfs
    Supports d_type: true
    Native Overlay Diff: true
   <...>
   ```

Docker is now using the `overlay2` storage driver and has automatically created the overlay mount with the required `lowerdir`, `upperdir`, `merged`, and `workdir` constructs.

​	Docker 现在正在使用 `overlay2` 存储驱动，并已自动创建了包含必要的 `lowerdir`、`upperdir`、`merged` 和 `workdir` 结构的 overlay 挂载。

Continue reading for details about how OverlayFS works within your Docker containers, as well as performance advice and information about limitations of its compatibility with different backing filesystems.

​	继续阅读以了解 OverlayFS 在 Docker 容器中的工作原理、性能建议以及与不同文件系统的兼容性限制的信息。

## `overlay2` 驱动的工作原理 How the `overlay2` driver works

OverlayFS layers two directories on a single Linux host and presents them as a single directory. These directories are called layers, and the unification process is referred to as a union mount. OverlayFS refers to the lower directory as `lowerdir` and the upper directory a `upperdir`. The unified view is exposed through its own directory called `merged`.

​	OverlayFS 在单一的 Linux 主机上层叠两个目录并将它们呈现为一个目录。这些目录称为层，而该统一过程称为联合挂载。OverlayFS 将下层目录称为 `lowerdir`，将上层目录称为 `upperdir`。联合视图通过名为 `merged` 的目录暴露。

The `overlay2` driver natively supports up to 128 lower OverlayFS layers. This capability provides better performance for layer-related Docker commands such as `docker build` and `docker commit`, and consumes fewer inodes on the backing filesystem.

​	`overlay2` 驱动原生支持多达 128 个下层 OverlayFS 层。这一功能提供了更好的层相关 Docker 命令性能（例如 `docker build` 和 `docker commit`），并减少了后备文件系统上的 inode 使用。

### 镜像和容器的磁盘层 Image and container layers on-disk

After downloading a five-layer image using `docker pull ubuntu`, you can see six directories under `/var/lib/docker/overlay2`.

​	下载一个包含五层的镜像（使用 `docker pull ubuntu`）后，您会在 `/var/lib/docker/overlay2` 下看到六个目录。

> **Warning**
>
> 
>
> Don't directly manipulate any files or directories within `/var/lib/docker/`. These files and directories are managed by Docker.
>
> ​	不要直接操作 `/var/lib/docker/` 下的任何文件或目录，这些文件和目录由 Docker 管理。



```console
$ ls -l /var/lib/docker/overlay2

total 24
drwx------ 5 root root 4096 Jun 20 07:36 223c2864175491657d238e2664251df13b63adb8d050924fd1bfcdb278b866f7
drwx------ 3 root root 4096 Jun 20 07:36 3a36935c9df35472229c57f4a27105a136f5e4dbef0f87905b2e506e494e348b
drwx------ 5 root root 4096 Jun 20 07:36 4e9fa83caff3e8f4cc83693fa407a4a9fac9573deaf481506c102d484dd1e6a1
drwx------ 5 root root 4096 Jun 20 07:36 e8876a226237217ec61c4baf238a32992291d059fdac95ed6303bdff3f59cff5
drwx------ 5 root root 4096 Jun 20 07:36 eca1e4e1694283e001f200a667bb3cb40853cf2d1b12c29feda7422fed78afed
drwx------ 2 root root 4096 Jun 20 07:36 l
```

The new `l` (lowercase `L`) directory contains shortened layer identifiers as symbolic links. These identifiers are used to avoid hitting the page size limitation on arguments to the `mount` command.

​	新创建的 `l` 目录（小写字母 "L"）包含作为符号链接的简化层标识符。使用这些标识符可以避免在 `mount` 命令的参数中遇到页面大小的限制。

```console
$ ls -l /var/lib/docker/overlay2/l

total 20
lrwxrwxrwx 1 root root 72 Jun 20 07:36 6Y5IM2XC7TSNIJZZFLJCS6I4I4 -> ../3a36935c9df35472229c57f4a27105a136f5e4dbef0f87905b2e506e494e348b/diff
lrwxrwxrwx 1 root root 72 Jun 20 07:36 B3WWEFKBG3PLLV737KZFIASSW7 -> ../4e9fa83caff3e8f4cc83693fa407a4a9fac9573deaf481506c102d484dd1e6a1/diff
lrwxrwxrwx 1 root root 72 Jun 20 07:36 JEYMODZYFCZFYSDABYXD5MF6YO -> ../eca1e4e1694283e001f200a667bb3cb40853cf2d1b12c29feda7422fed78afed/diff
lrwxrwxrwx 1 root root 72 Jun 20 07:36 NFYKDW6APBCCUCTOUSYDH4DXAT -> ../223c2864175491657d238e2664251df13b63adb8d050924fd1bfcdb278b866f7/diff
lrwxrwxrwx 1 root root 72 Jun 20 07:36 UL2MW33MSE3Q5VYIKBRN4ZAGQP -> ../e8876a226237217ec61c4baf238a32992291d059fdac95ed6303bdff3f59cff5/diff
```

The lowest layer contains a file called `link`, which contains the name of the shortened identifier, and a directory called `diff` which contains the layer's contents.

​	最底层包含一个名为 `link` 的文件，该文件包含简化的标识符名称，以及一个包含该层内容的 `diff` 目录。

```console
$ ls /var/lib/docker/overlay2/3a36935c9df35472229c57f4a27105a136f5e4dbef0f87905b2e506e494e348b/

diff  link

$ cat /var/lib/docker/overlay2/3a36935c9df35472229c57f4a27105a136f5e4dbef0f87905b2e506e494e348b/link

6Y5IM2XC7TSNIJZZFLJCS6I4I4

$ ls  /var/lib/docker/overlay2/3a36935c9df35472229c57f4a27105a136f5e4dbef0f87905b2e506e494e348b/diff

bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

The second-lowest layer, and each higher layer, contain a file called `lower`, which denotes its parent, and a directory called `diff` which contains its contents. It also contains a `merged` directory, which contains the unified contents of its parent layer and itself, and a `work` directory which is used internally by OverlayFS.

​	倒数第二层及每个更高层包含一个名为 `lower` 的文件，该文件表示其父层，还有一个包含层内容的 `diff` 目录。此外，还包含一个 `merged` 目录，该目录包含其父层及自身的统一内容，和一个内部供 OverlayFS 使用的 `work` 目录。

```console
$ ls /var/lib/docker/overlay2/223c2864175491657d238e2664251df13b63adb8d050924fd1bfcdb278b866f7

diff  link  lower  merged  work

$ cat /var/lib/docker/overlay2/223c2864175491657d238e2664251df13b63adb8d050924fd1bfcdb278b866f7/lower

l/6Y5IM2XC7TSNIJZZFLJCS6I4I4

$ ls /var/lib/docker/overlay2/223c2864175491657d238e2664251df13b63adb8d050924fd1bfcdb278b866f7/diff/

etc  sbin  usr  var
```

To view the mounts which exist when you use the `overlay` storage driver with Docker, use the `mount` command. The output below is truncated for readability.

​	要查看使用 `overlay` 存储驱动程序的 Docker 现有挂载，请使用 `mount` 命令。以下输出为简化版以便阅读。

```console
$ mount | grep overlay

overlay on /var/lib/docker/overlay2/9186877cdf386d0a3b016149cf30c208f326dca307529e646afce5b3f83f5304/merged
type overlay (rw,relatime,
lowerdir=l/DJA75GUWHWG7EWICFYX54FIOVT:l/B3WWEFKBG3PLLV737KZFIASSW7:l/JEYMODZYFCZFYSDABYXD5MF6YO:l/UL2MW33MSE3Q5VYIKBRN4ZAGQP:l/NFYKDW6APBCCUCTOUSYDH4DXAT:l/6Y5IM2XC7TSNIJZZFLJCS6I4I4,
upperdir=9186877cdf386d0a3b016149cf30c208f326dca307529e646afce5b3f83f5304/diff,
workdir=9186877cdf386d0a3b016149cf30c208f326dca307529e646afce5b3f83f5304/work)
```

The `rw` on the second line shows that the `overlay` mount is read-write.

​	第二行的 `rw` 显示 `overlay` 挂载为可读写模式。

The following diagram shows how a Docker image and a Docker container are layered. The image layer is the `lowerdir` and the container layer is the `upperdir`. If the image has multiple layers, multiple `lowerdir` directories are used. The unified view is exposed through a directory called `merged` which is effectively the containers mount point.

​	下图展示了 Docker 镜像与 Docker 容器如何分层。镜像层为 `lowerdir`，容器层为 `upperdir`。如果镜像包含多个层，则会使用多个 `lowerdir` 目录。统一视图通过名为 `merged` 的目录暴露出来，这个目录即容器的挂载点。![How Docker constructs map to OverlayFS constructs](OverlayFSstoragedriver_img/overlay_constructs.webp)

Where the image layer and the container layer contain the same files, the container layer (`upperdir`) takes precedence and obscures the existence of the same files in the image layer.

​	当镜像层和容器层包含相同文件时，容器层 (`upperdir`) 优先显示，并遮蔽镜像层中的相同文件。

To create a container, the `overlay2` driver combines the directory representing the image's top layer plus a new directory for the container. The image's layers are the `lowerdirs` in the overlay and are read-only. The new directory for the container is the `upperdir` and is writable.

​	为了创建一个容器，`overlay2` 驱动程序将表示镜像顶层的目录与容器的新目录组合。镜像的各层是只读的 `lowerdirs`，而容器的新目录是可写的 `upperdir`。

### 磁盘上的镜像和容器层 Image and container layers on-disk

The following `docker pull` command shows a Docker host downloading a Docker image comprising five layers.

​	以下 `docker pull` 命令展示了 Docker 主机下载包含五层的 Docker 镜像的过程。

```console
$ docker pull ubuntu

Using default tag: latest
latest: Pulling from library/ubuntu

5ba4f30e5bea: Pull complete
9d7d19c9dc56: Pull complete
ac6ad7efd0f9: Pull complete
e7491a747824: Pull complete
a3ed95caeb02: Pull complete
Digest: sha256:46fb5d001b88ad904c5c732b086b596b92cfb4a4840a3abd0e35dbb6870585e4
Status: Downloaded newer image for ubuntu:latest
```

#### 镜像层 The image layers

Each image layer has its own directory within `/var/lib/docker/overlay/`, which contains its contents, as shown in the following example. The image layer IDs don't correspond to the directory IDs.

​	每个镜像层在 `/var/lib/docker/overlay/` 下都有其自己的目录，其中包含该层的内容，如以下示例所示。镜像层 ID 与目录 ID 并不对应。

> **Warning**
>
> 
>
> Don't directly manipulate any files or directories within `/var/lib/docker/`. These files and directories are managed by Docker.
>
> ​	不要直接操作 `/var/lib/docker/` 中的任何文件或目录。这些文件和目录由 Docker 管理。



```console
$ ls -l /var/lib/docker/overlay/

total 20
drwx------ 3 root root 4096 Jun 20 16:11 38f3ed2eac129654acef11c32670b534670c3a06e483fce313d72e3e0a15baa8
drwx------ 3 root root 4096 Jun 20 16:11 55f1e14c361b90570df46371b20ce6d480c434981cbda5fd68c6ff61aa0a5358
drwx------ 3 root root 4096 Jun 20 16:11 824c8a961a4f5e8fe4f4243dab57c5be798e7fd195f6d88ab06aea92ba931654
drwx------ 3 root root 4096 Jun 20 16:11 ad0fe55125ebf599da124da175174a4b8c1878afe6907bf7c78570341f308461
drwx------ 3 root root 4096 Jun 20 16:11 edab9b5e5bf73f2997524eebeac1de4cf9c8b904fa8ad3ec43b3504196aa3801
```

The image layer directories contain the files unique to that layer as well as hard links to the data shared with lower layers. This allows for efficient use of disk space.

​	镜像层目录包含该层独有的文件以及与下层共享的数据的硬链接。这可以有效利用磁盘空间。

```console
$ ls -i /var/lib/docker/overlay2/38f3ed2eac129654acef11c32670b534670c3a06e483fce313d72e3e0a15baa8/root/bin/ls

19793696 /var/lib/docker/overlay2/38f3ed2eac129654acef11c32670b534670c3a06e483fce313d72e3e0a15baa8/root/bin/ls

$ ls -i /var/lib/docker/overlay2/55f1e14c361b90570df46371b20ce6d480c434981cbda5fd68c6ff61aa0a5358/root/bin/ls

19793696 /var/lib/docker/overlay2/55f1e14c361b90570df46371b20ce6d480c434981cbda5fd68c6ff61aa0a5358/root/bin/ls
```

#### 容器层 The container layer

Containers also exist on-disk in the Docker host's filesystem under `/var/lib/docker/overlay/`. If you list a running container's subdirectory using the `ls -l` command, three directories and one file exist:

​	容器在 Docker 主机的文件系统 `/var/lib/docker/overlay/` 下以磁盘文件形式存在。如果使用 `ls -l` 命令列出运行中容器的子目录，将看到三个目录和一个文件：

```console
$ ls -l /var/lib/docker/overlay2/<directory-of-running-container>

total 16
-rw-r--r-- 1 root root   64 Jun 20 16:39 lower-id
drwxr-xr-x 1 root root 4096 Jun 20 16:39 merged
drwxr-xr-x 4 root root 4096 Jun 20 16:39 upper
drwx------ 3 root root 4096 Jun 20 16:39 work
```

The `lower-id` file contains the ID of the top layer of the image the container is based on, which is the OverlayFS `lowerdir`.

​	`lower-id` 文件包含该容器所基于的镜像的顶层 ID，即 OverlayFS 的 `lowerdir`。

```console
$ cat /var/lib/docker/overlay2/ec444863a55a9f1ca2df72223d459c5d940a721b2288ff86a3f27be28b53be6c/lower-id

55f1e14c361b90570df46371b20ce6d480c434981cbda5fd68c6ff61aa0a5358
```

The `upper` directory contains the contents of the container's read-write layer, which corresponds to the OverlayFS `upperdir`.

​	`upper` 目录包含容器的可读写层内容，对应于 OverlayFS 的 `upperdir`。

The `merged` directory is the union mount of the `lowerdir` and `upperdirs`, which comprises the view of the filesystem from within the running container.

​	`merged` 目录是 `lowerdir` 和 `upperdirs` 的联合挂载，展示了运行中的容器内的文件系统视图。

The `work` directory is internal to OverlayFS.

​	`work` 目录是 OverlayFS 的内部使用目录。

To view the mounts which exist when you use the `overlay2` storage driver with Docker, use the `mount` command. The following output is truncated for readability.

​	要查看 Docker 使用 `overlay2` 存储驱动程序时现有的挂载，请使用 `mount` 命令。以下输出为简化版以便阅读。



```console
$ mount | grep overlay

overlay on /var/lib/docker/overlay2/l/ec444863a55a.../merged
type overlay (rw,relatime,lowerdir=/var/lib/docker/overlay2/l/55f1e14c361b.../root,
upperdir=/var/lib/docker/overlay2/l/ec444863a55a.../upper,
workdir=/var/lib/docker/overlay2/l/ec444863a55a.../work)
```

The `rw` on the second line shows that the `overlay` mount is read-write.

​	第二行中的 `rw` 表示 `overlay` 挂载为可读写模式。

## 使用 `overlay2` 进行容器读写操作的方式 How container reads and writes work with `overlay2`



### 读取文件 Reading files

Consider three scenarios where a container opens a file for read access with overlay.

​	假设容器使用 overlay 打开一个文件进行读取操作，有以下三种情况：

#### 文件在容器层不存在 The file does not exist in the container layer

If a container opens a file for read access and the file does not already exist in the container (`upperdir`) it is read from the image (`lowerdir`). This incurs very little performance overhead.

​	如果容器打开一个文件进行读取，但该文件在容器层 (`upperdir`) 中不存在，则会从镜像层 (`lowerdir`) 中读取，这几乎不会影响性能。

#### 文件仅存在于容器层 The file only exists in the container layer

If a container opens a file for read access and the file exists in the container (`upperdir`) and not in the image (`lowerdir`), it's read directly from the container.

​	如果容器打开一个文件进行读取，并且该文件仅存在于容器层 (`upperdir`) 中而非镜像层 (`lowerdir`)，则会直接从容器中读取。

#### 文件在容器层和镜像层中都存在 The file exists in both the container layer and the image layer

If a container opens a file for read access and the file exists in the image layer and the container layer, the file's version in the container layer is read. Files in the container layer (`upperdir`) obscure files with the same name in the image layer (`lowerdir`).

​	如果容器打开一个文件进行读取，该文件同时存在于镜像层和容器层中，则读取容器层中的版本。容器层 (`upperdir`) 的文件会遮蔽镜像层 (`lowerdir`) 中同名的文件。

### 修改文件或目录 Modifying files or directories

Consider some scenarios where files in a container are modified.

​	假设容器中的文件被修改，有以下情况：

#### 首次写入文件 Writing to a file for the first time

The first time a container writes to an existing file, that file does not exist in the container (`upperdir`). The `overlay2` driver performs a `copy_up` operation to copy the file from the image (`lowerdir`) to the container (`upperdir`). The container then writes the changes to the new copy of the file in the container layer.

​	当容器首次写入一个已有文件时，该文件在容器层 (`upperdir`) 中不存在。`overlay2` 驱动执行 `copy_up` 操作，将文件从镜像层 (`lowerdir`) 复制到容器层 (`upperdir`)，容器随后在该文件的副本上写入更改。

However, OverlayFS works at the file level rather than the block level. This means that all OverlayFS `copy_up` operations copy the entire file, even if the file is large and only a small part of it's being modified. This can have a noticeable impact on container write performance. However, two things are worth noting:

​	OverlayFS 在文件层级操作，而不是块层级。这意味着所有 OverlayFS 的 `copy_up` 操作会复制整个文件，即使文件很大而只修改了其中一小部分。这可能会对容器写入性能产生显著影响，不过值得注意以下两点：

- The `copy_up` operation only occurs the first time a given file is written to. Subsequent writes to the same file operate against the copy of the file already copied up to the container.
  - `copy_up` 操作仅在文件首次写入时发生，后续对同一文件的写入直接在已复制到容器的文件上操作。

- OverlayFS works with multiple layers. This means that performance can be impacted when searching for files in images with many layers.
  - OverlayFS 支持多层，因此在包含许多层的镜像中查找文件时性能可能会受到影响。

#### 删除文件和目录 Deleting files and directories

- When a *file* is deleted within a container, a *whiteout* file is created in the container (`upperdir`). The version of the file in the image layer (`lowerdir`) is not deleted (because the `lowerdir` is read-only). However, the whiteout file prevents it from being available to the container.
  - 当容器内删除一个*文件*时，会在容器层 (`upperdir`) 中创建一个*白化*文件。镜像层 (`lowerdir`) 中的文件并未删除（因为 `lowerdir` 是只读的），但白化文件会阻止容器访问该文件。

- When a *directory* is deleted within a container, an *opaque directory* is created within the container (`upperdir`). This works in the same way as a whiteout file and effectively prevents the directory from being accessed, even though it still exists in the image (`lowerdir`).
  - 当容器内删除一个*目录*时，会在容器层 (`upperdir`) 中创建一个*不透明目录*。它与白化文件的作用相同，有效地阻止访问该目录，即使它仍然存在于镜像层 (`lowerdir`) 中。

#### 重命名目录 Renaming directories

Calling `rename(2)` for a directory is allowed only when both the source and the destination path are on the top layer. Otherwise, it returns `EXDEV` error ("cross-device link not permitted"). Your application needs to be designed to handle `EXDEV` and fall back to a "copy and unlink" strategy.

​	仅当源路径和目标路径都在顶层时，`rename(2)` 操作才被允许，否则会返回 `EXDEV` 错误（“跨设备链接不允许”）。应用程序需要设计为处理 `EXDEV` 错误并回退到“复制和取消链接”策略。

### OverlayFS 和 Docker 性能 OverlayFS and Docker Performance

`overlay2` may perform better than `btrfs`. However, be aware of the following details:

​	`overlay2` 在性能上可能优于 `btrfs`。不过需注意以下细节：

#### 页面缓存 Page caching

OverlayFS supports page cache sharing. Multiple containers accessing the same file share a single page cache entry for that file. This makes the `overlay2` drivers efficient with memory and a good option for high-density use cases such as PaaS.

​	OverlayFS 支持页面缓存共享。多个容器访问相同文件时会共享该文件的单一页面缓存条目。这使得 `overlay2` 驱动在内存使用上非常高效，并适合高密度的使用场景，如 PaaS。

### Copyup

As with other copy-on-write filesystems, OverlayFS performs copy-up operations whenever a container writes to a file for the first time. This can add latency into the write operation, especially for large files. However, once the file has been copied up, all subsequent writes to that file occur in the upper layer, without the need for further copy-up operations.

​	与其他写时复制文件系统一样，OverlayFS 在容器首次写入文件时执行 copy-up 操作。对于大文件，这可能会增加写入操作的延迟。然而，一旦文件被复制上去，所有后续对该文件的写入都发生在上层，不再需要进一步的 copy-up 操作。

#### 性能最佳实践 Performance best practices

The following generic performance best practices apply to OverlayFS.

​	以下是适用于 OverlayFS 的通用性能最佳实践：

##### 使用快速存储 Use fast storage

Solid-state drives (SSDs) provide faster reads and writes than spinning disks.

​	固态硬盘（SSD）在读写速度上优于机械硬盘。

##### 对写密集型工作负载使用卷 Use volumes for write-heavy workloads

Volumes provide the best and most predictable performance for write-heavy workloads. This is because they bypass the storage driver and don't incur any of the potential overheads introduced by thin provisioning and copy-on-write. Volumes have other benefits, such as allowing you to share data among containers and persisting your data even if no running container is using them.

​	卷提供了最佳和最稳定的性能，因为它们绕过了存储驱动，不会引入细分配置和写时复制带来的潜在开销。卷还具有其他优势，例如允许在容器之间共享数据，并在没有运行的容器使用它们时保持数据的持久性。

## OverlayFS 兼容性的限制 Limitations on OverlayFS compatibility

To summarize the OverlayFS's aspect which is incompatible with other filesystems:

​	OverlayFS 具有与其他文件系统不兼容的特性，包括：

- [`open(2)`](https://linux.die.net/man/2/open)

- OverlayFS only implements a subset of the POSIX standards. This can result in certain OverlayFS operations breaking POSIX standards. One such operation is the copy-up operation. Suppose that your application calls `fd1=open("foo", O_RDONLY)` and then `fd2=open("foo", O_RDWR)`. In this case, your application expects `fd1` and `fd2` to refer to the same file. However, due to a copy-up operation that occurs after the second calling to `open(2)`, the descriptors refer to different files. The `fd1` continues to reference the file in the image (`lowerdir`) and the `fd2` references the file in the container (`upperdir`). A workaround for this is to `touch` the files which causes the copy-up operation to happen. All subsequent `open(2)` operations regardless of read-only or read-write access mode reference the file in the container (`upperdir`).`yum` is known to be affected unless the `yum-plugin-ovl` package is installed. If the `yum-plugin-ovl` package is not available in your distribution such as RHEL/CentOS prior to 6.8 or 7.2, you may need to run `touch /var/lib/rpm/*` before running `yum install`. This package implements the `touch` workaround referenced above for `yum`.

  - OverlayFS 只实现了 POSIX 标准的一个子集，这可能导致某些 OverlayFS 操作不符合 POSIX 标准。例如，copy-up 操作。假设应用程序调用 `fd1=open("foo", O_RDONLY)` 然后调用 `fd2=open("foo", O_RDWR)`，此时应用程序期望 `fd1` 和 `fd2` 引用同一文件。但由于第二次调用 `open(2)` 时发生了 copy-up 操作，文件描述符将引用不同的文件。可以通过 `touch` 该文件来强制触发 copy-up 操作，使后续所有 `open(2)` 操作都引用容器层 (`upperdir`) 中的文件。`yum` 在未安装 `yum-plugin-ovl` 插件时可能会受到影响。对于 RHEL/CentOS 低于 6.8 或 7.2 的版本，如果此插件不可用，您可能需要在运行 `yum install` 之前执行 `touch /var/lib/rpm/*`。

- [`rename(2)`](https://linux.die.net/man/2/rename)

  OverlayFS does not fully support the `rename(2)` system call. Your application needs to detect its failure and fall back to a "copy and unlink" strategy.

  ​	OverlayFS 不完全支持 `rename(2)` 系统调用。应用程序需要检测该调用的失败并回退到“复制和取消链接”策略。
