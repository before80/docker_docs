+++
title = "VFS 存储驱动程序"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/storage/drivers/vfs-driver/](https://docs.docker.com/engine/storage/drivers/vfs-driver/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# VFS storage driver

The VFS storage driver isn't a union filesystem. Each layer is a directory on disk, and there is no copy-on-write support. To create a new layer, a "deep copy" is done of the previous layer. This leads to lower performance and more space used on disk than other storage drivers. However, it is robust, stable, and works in every environment. It can also be used as a mechanism to verify other storage back-ends against, in a testing environment.

​	VFS 存储驱动程序不是联合文件系统。每一层都是磁盘上的一个目录，且不支持写时复制（COW）。创建新层时，会对上一层进行“深度复制”，这会导致性能较低且占用更多磁盘空间。尽管如此，它依然稳健、稳定，并且适用于任何环境。此外，它还可以用作测试环境中其他存储后端的验证机制。

## 使用 `vfs` 存储驱动程序配置 Docker - Configure Docker with the `vfs` storage driver

1. Stop Docker.

   停止 Docker。

   ```console
   $ sudo systemctl stop docker
   ```

2. Edit `/etc/docker/daemon.json`. If it doesn't yet exist, create it. Assuming that the file was empty, add the following contents.

   编辑 `/etc/docker/daemon.json`。如果该文件不存在，则创建它。假设文件为空，添加以下内容：

   ```json
   {
     "storage-driver": "vfs"
   }
   ```

   If you want to set a quota to control the maximum size the VFS storage driver can use, set the `size` option on the `storage-opts` key.

   ​	如果您希望设置配额来控制 VFS 存储驱动程序的最大使用大小，可以在 `storage-opts` 键中设置 `size` 选项。

   ```json
   {
     "storage-driver": "vfs",
     "storage-opts": ["size=256M"]
   }
   ```

   Docker doesn't start if the `daemon.json` file contains invalid JSON.

   ​	如果 `daemon.json` 文件包含无效的 JSON 格式，Docker 将无法启动。

3. Start Docker.

   启动 Docker。

   ```console
   $ sudo systemctl start docker
   ```

4. Verify that the daemon is using the `vfs` storage driver. Use the `docker info` command and look for `Storage Driver`.

   验证守护进程是否正在使用 `vfs` 存储驱动。使用 `docker info` 命令并查找 `Storage Driver`。

   ```console
   $ docker info
   
   Storage Driver: vfs
   ...
   ```

Docker is now using the `vfs` storage driver. Docker has automatically created the `/var/lib/docker/vfs/` directory, which contains all the layers used by running containers.

​	Docker 现在使用 `vfs` 存储驱动程序，并自动创建了 `/var/lib/docker/vfs/` 目录，该目录包含了运行中的容器使用的所有层。

## `vfs` 存储驱动的工作原理 How the `vfs` storage driver works

Each image layer and the writable container layer are represented on the Docker host as subdirectories within `/var/lib/docker/`. The union mount provides the unified view of all layers. The directory names don't directly correspond to the IDs of the layers themselves.

​	每个镜像层和可写容器层在 Docker 主机上都表现为 `/var/lib/docker/` 内的子目录。联合挂载提供了所有层的统一视图。目录名称并不直接对应于层的 ID。

VFS doesn't support copy-on-write (COW). Each time a new layer is created, it's a deep copy of its parent layer. These layers are all located under `/var/lib/docker/vfs/dir/`.

​	VFS 不支持写时复制（COW）。每当创建新层时，它会对其父层进行深度复制。这些层都位于 `/var/lib/docker/vfs/dir/` 下。

### 示例：磁盘上的镜像和容器结构 Example: Image and container on-disk constructs

The following `docker pull` command shows a Docker host downloading a Docker image comprising five layers.

​	以下 `docker pull` 命令展示了 Docker 主机下载一个包含五层的 Docker 镜像的过程。

```console
$ docker pull ubuntu

Using default tag: latest
latest: Pulling from library/ubuntu
e0a742c2abfd: Pull complete
486cb8339a27: Pull complete
dc6f0d824617: Pull complete
4f7a5649a30e: Pull complete
672363445ad2: Pull complete
Digest: sha256:84c334414e2bfdcae99509a6add166bbb4fa4041dc3fa6af08046a66fed3005f
Status: Downloaded newer image for ubuntu:latest
```

After pulling, each of these layers is represented as a subdirectory of `/var/lib/docker/vfs/dir/`. The directory names do not correlate with the image layer IDs shown in the `docker pull` command. To see the size taken up on disk by each layer, you can use the `du -sh` command, which gives the size as a human-readable value.

​	拉取完成后，每一层在 `/var/lib/docker/vfs/dir/` 中表现为一个子目录。目录名称与 `docker pull` 命令中显示的镜像层 ID 并不对应。可以使用 `du -sh` 命令查看每一层在磁盘上占用的空间大小，以便查看人类可读的值。

```console
$ ls -l /var/lib/docker/vfs/dir/

total 0
drwxr-xr-x.  2 root root  19 Aug  2 18:19 3262dfbe53dac3e1ab7dcc8ad5d8c4d586a11d2ac3c4234892e34bff7f6b821e
drwxr-xr-x. 21 root root 224 Aug  2 18:23 6af21814449345f55d88c403e66564faad965d6afa84b294ae6e740c9ded2561
drwxr-xr-x. 21 root root 224 Aug  2 18:23 6d3be4585ba32f9f5cbff0110e8d07aea5f5b9fbb1439677c27e7dfee263171c
drwxr-xr-x. 21 root root 224 Aug  2 18:23 9ecd2d88ca177413ab89f987e1507325285a7418fc76d0dcb4bc021447ba2bab
drwxr-xr-x. 21 root root 224 Aug  2 18:23 a292ac6341a65bf3a5da7b7c251e19de1294bd2ec32828de621d41c7ad31f895
drwxr-xr-x. 21 root root 224 Aug  2 18:23 e92be7a4a4e3ccbb7dd87695bca1a0ea373d4f673f455491b1342b33ed91446b
```



```console
$ du -sh /var/lib/docker/vfs/dir/*

4.0K	/var/lib/docker/vfs/dir/3262dfbe53dac3e1ab7dcc8ad5d8c4d586a11d2ac3c4234892e34bff7f6b821e
125M	/var/lib/docker/vfs/dir/6af21814449345f55d88c403e66564faad965d6afa84b294ae6e740c9ded2561
104M	/var/lib/docker/vfs/dir/6d3be4585ba32f9f5cbff0110e8d07aea5f5b9fbb1439677c27e7dfee263171c
125M	/var/lib/docker/vfs/dir/9ecd2d88ca177413ab89f987e1507325285a7418fc76d0dcb4bc021447ba2bab
104M	/var/lib/docker/vfs/dir/a292ac6341a65bf3a5da7b7c251e19de1294bd2ec32828de621d41c7ad31f895
104M	/var/lib/docker/vfs/dir/e92be7a4a4e3ccbb7dd87695bca1a0ea373d4f673f455491b1342b33ed91446b
```

The above output shows that three layers each take 104M and two take 125M. These directories have only small differences from each other, but they all consume the same amount of disk space. This is one of the disadvantages of using the `vfs` storage driver.

​	上述输出显示三层分别占用 104M，两层占用 125M。这些目录间的差异很小，但它们都占用了相同的磁盘空间。这是使用 `vfs` 存储驱动的缺点之一。

## 相关信息 Related information

- [Understand images, containers, and storage drivers 了解镜像、容器和存储驱动]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}})
- [Select a storage driver 选择存储驱动]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/Selectastoragedriver" >}})
