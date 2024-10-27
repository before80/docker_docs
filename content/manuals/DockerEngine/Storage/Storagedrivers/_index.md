+++
title = "Storage drivers"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/storage/drivers/](https://docs.docker.com/engine/storage/drivers/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Storage drivers - 存储驱动程序

To use storage drivers effectively, it's important to know how Docker builds and stores images, and how these images are used by containers. You can use this information to make informed choices about the best way to persist data from your applications and avoid performance problems along the way.

​	为了有效使用存储驱动程序，了解 Docker 如何构建和存储镜像以及容器如何使用这些镜像是很重要的。您可以利用这些信息来做出明智的选择，找到最佳的方式来持久化应用程序的数据并避免性能问题。

## 存储驱动程序与 Docker 卷 Storage drivers versus Docker volumes

Docker uses storage drivers to store image layers, and to store data in the writable layer of a container. The container's writable layer doesn't persist after the container is deleted, but is suitable for storing ephemeral data that is generated at runtime. Storage drivers are optimized for space efficiency, but (depending on the storage driver) write speeds are lower than native file system performance, especially for storage drivers that use a copy-on-write filesystem. Write-intensive applications, such as database storage, are impacted by a performance overhead, particularly if pre-existing data exists in the read-only layer.

​	Docker 使用存储驱动程序来存储镜像层，以及存储容器中可写层的数据。容器的可写层在容器删除后不会持久化，但适合用于存储运行时生成的临时数据。存储驱动程序经过优化以节省空间，但（取决于存储驱动程序）写入速度低于本地文件系统的性能，特别是对于使用写时复制（CoW）文件系统的存储驱动程序。写密集型应用程序（例如数据库存储）会受到性能开销的影响，特别是在只读层中存在预先存在的数据时。

Use Docker volumes for write-intensive data, data that must persist beyond the container's lifespan, and data that must be shared between containers. Refer to the [volumes section]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) to learn how to use volumes to persist data and improve performance.

​	对于写密集型数据、需要在容器生命周期之外持久化的数据，以及需要在多个容器之间共享的数据，请使用 Docker 卷。请参考[卷部分]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})，了解如何使用卷来持久化数据和提升性能。

## 镜像和层 Images and layers

A Docker image is built up from a series of layers. Each layer represents an instruction in the image's Dockerfile. Each layer except the very last one is read-only. Consider the following Dockerfile:

​	Docker 镜像是由一系列层构建的。每一层代表了 Dockerfile 中的一条指令，除了最后一层之外，每一层都是只读的。以下是一个示例 Dockerfile：



```dockerfile
# syntax=docker/dockerfile:1

FROM ubuntu:22.04
LABEL org.opencontainers.image.authors="org@example.com"
COPY . /app
RUN make /app
RUN rm -r $HOME/.cache
CMD python /app/app.py
```

This Dockerfile contains four commands. Commands that modify the filesystem create a layer. The `FROM` statement starts out by creating a layer from the `ubuntu:22.04` image. The `LABEL` command only modifies the image's metadata, and doesn't produce a new layer. The `COPY` command adds some files from your Docker client's current directory. The first `RUN` command builds your application using the `make` command, and writes the result to a new layer. The second `RUN` command removes a cache directory, and writes the result to a new layer. Finally, the `CMD` instruction specifies what command to run within the container, which only modifies the image's metadata, which doesn't produce an image layer.

​	这个 Dockerfile 包含四条命令。修改文件系统的命令会创建一个新层。`FROM` 语句从 `ubuntu:22.04` 镜像开始创建一层。`LABEL` 命令只修改镜像的元数据，不会产生新层。`COPY` 命令将一些文件从 Docker 客户端的当前目录复制到镜像中。第一个 `RUN` 命令使用 `make` 命令构建应用程序，并将结果写入新层。第二个 `RUN` 命令删除缓存目录，并将结果写入新层。最后，`CMD` 指令指定容器内要运行的命令，这只修改了镜像的元数据，因此不会产生镜像层。

Each layer is only a set of differences from the layer before it. Note that both *adding*, and *removing* files will result in a new layer. In the example above, the `$HOME/.cache` directory is removed, but will still be available in the previous layer and add up to the image's total size. Refer to the [Best practices for writing Dockerfiles]({{< ref "/manuals/DockerBuild/Building/Bestpractices" >}}) and [use multi-stage builds]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}}) sections to learn how to optimize your Dockerfiles for efficient images.

​	每一层仅仅是一组相对于前一层的差异。请注意，*添加*和*删除*文件都会生成一个新层。在上例中，`$HOME/.cache` 目录被删除，但它仍会在前一层中存在，并增加镜像的总大小。请参考[编写 Dockerfile 的最佳实践]({{< ref "/manuals/DockerBuild/Building/Bestpractices" >}})和[使用多阶段构建]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}})部分，了解如何优化 Dockerfile 以获得高效的镜像。

The layers are stacked on top of each other. When you create a new container, you add a new writable layer on top of the underlying layers. This layer is often called the "container layer". All changes made to the running container, such as writing new files, modifying existing files, and deleting files, are written to this thin writable container layer. The diagram below shows a container based on an `ubuntu:15.04` image.

​	这些层是堆叠在一起的。当您创建一个新容器时，会在底层层之上添加一个新的可写层。这一层通常称为“容器层”。在运行容器时进行的所有更改，例如写入新文件、修改现有文件和删除文件，都会写入这个薄的可写容器层。下图展示了基于 `ubuntu:15.04` 镜像的容器结构。

![Layers of a container based on the Ubuntu image](_index_img/container-layers.webp)

A storage driver handles the details about the way these layers interact with each other. Different storage drivers are available, which have advantages and disadvantages in different situations.

​	存储驱动程序处理这些层之间的交互细节。不同的存储驱动程序可供选择，每种都有其在不同情况下的优势和劣势。

## 容器和层 Container and layers

The major difference between a container and an image is the top writable layer. All writes to the container that add new or modify existing data are stored in this writable layer. When the container is deleted, the writable layer is also deleted. The underlying image remains unchanged.

​	容器和镜像的主要区别在于顶部的可写层。容器的所有写入操作（如添加新数据或修改现有数据）都存储在这个可写层中。当容器被删除时，这个可写层也会被删除，而底层的镜像保持不变。

Because each container has its own writable container layer, and all changes are stored in this container layer, multiple containers can share access to the same underlying image and yet have their own data state. The diagram below shows multiple containers sharing the same Ubuntu 15.04 image.

​	由于每个容器都有自己独立的可写层，并且所有更改都存储在此容器层中，因此多个容器可以共享对同一底层镜像的访问，同时保持各自的数据状态。下图展示了多个容器共享同一 Ubuntu 15.04 镜像的情况。

![Containers sharing the same image](_index_img/sharing-layers.webp)

Docker uses storage drivers to manage the contents of the image layers and the writable container layer. Each storage driver handles the implementation differently, but all drivers use stackable image layers and the copy-on-write (CoW) strategy.

​	Docker 使用存储驱动程序来管理镜像层和可写容器层的内容。每个存储驱动程序的实现有所不同，但所有驱动程序都使用可堆叠的镜像层和写时复制（CoW）策略。

> **Note**
>
> 
>
> Use Docker volumes if you need multiple containers to have shared access to the exact same data. Refer to the [volumes section]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) to learn about volumes.
>
> ​	如果您需要多个容器访问完全相同的数据，请使用 Docker 卷。请参考[卷部分]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})了解卷的更多信息。

## 容器在磁盘上的大小 Container size on disk

To view the approximate size of a running container, you can use the `docker ps -s` command. Two different columns relate to size.

​	要查看运行中容器的大致大小，可以使用 `docker ps -s` 命令。与大小相关的有两个不同的列：

- `size`: the amount of data (on disk) that's used for the writable layer of each container.
  - `size`：每个容器的可写层在磁盘上所使用的数据量。

- `virtual size`: the amount of data used for the read-only image data used by the container plus the container's writable layer `size`. Multiple containers may share some or all read-only image data. Two containers started from the same image share 100% of the read-only data, while two containers with different images which have layers in common share those common layers. Therefore, you can't just total the virtual sizes. This over-estimates the total disk usage by a potentially non-trivial amount.
  - `virtual size`：容器所使用的只读镜像数据加上容器的可写层 `size`。多个容器可能共享部分或全部的只读镜像数据。两个从同一镜像启动的容器共享 100% 的只读数据，而从不同镜像启动的两个容器如果有共同层也会共享这些共同层。因此，不能简单地累加虚拟大小，这会对总磁盘使用量造成明显的高估。


The total disk space used by all of the running containers on disk is some combination of each container's `size` and the `virtual size` values. If multiple containers started from the same exact image, the total size on disk for these containers would be SUM (`size` of containers) plus one image size (`virtual size` - `size`).

​	所有运行中容器在磁盘上所使用的总磁盘空间是每个容器的 `size` 和 `virtual size` 值的某种组合。如果多个容器从同一个镜像启动，这些容器的总磁盘使用量将是 SUM（容器的 `size`）加上一个镜像大小（`virtual size` - `size`）。

This also doesn't count the following additional ways a container can take up disk space:

​	这还不包括容器占用磁盘空间的其他方式：

- Disk space used for log files stored by the [logging-driver]({{< ref "/manuals/DockerEngine/Logsandmetrics" >}}). This can be non-trivial if your container generates a large amount of logging data and log rotation isn't configured.
  - 存储在[日志驱动程序]({{< ref "/manuals/DockerEngine/Logsandmetrics" >}})中的日志文件所占用的磁盘空间。如果容器生成大量日志数据并且未配置日志轮换，则此部分的空间会非常可观。

- Volumes and bind mounts used by the container.
  - 容器使用的卷和绑定挂载。

- Disk space used for the container's configuration files, which are typically small.
  - 容器配置文件所占用的磁盘空间，通常很小。

- Memory written to disk (if swapping is enabled).
  - 写入磁盘的内存（如果启用了交换）。

- Checkpoints, if you're using the experimental checkpoint/restore feature.
  - 检查点，如果使用的是实验性的检查点/恢复功能。

## 写时复制（CoW）策略 The copy-on-write (CoW) strategy

Copy-on-write is a strategy of sharing and copying files for maximum efficiency. If a file or directory exists in a lower layer within the image, and another layer (including the writable layer) needs read access to it, it just uses the existing file. The first time another layer needs to modify the file (when building the image or running the container), the file is copied into that layer and modified. This minimizes I/O and the size of each of the subsequent layers. These advantages are explained in more depth below.

​	写时复制是一种为了最大效率而共享和复制文件的策略。如果文件或目录存在于镜像的下层中，而另一层（包括可写层）需要读取它，它只会使用现有的文件。当另一层第一次需要修改该文件时（构建镜像或运行容器时），文件会被复制到该层中并修改。这样可以最小化 I/O 操作并减少每个后续层的大小。下面进一步解释了这些优点。

### 共享有助于缩小镜像大小 Sharing promotes smaller images

When you use `docker pull` to pull down an image from a repository, or when you create a container from an image that doesn't yet exist locally, each layer is pulled down separately, and stored in Docker's local storage area, which is usually `/var/lib/docker/` on Linux hosts. You can see these layers being pulled in this example:

​	当您使用 `docker pull` 从仓库拉取镜像，或者从本地不存在的镜像创建容器时，每个层都会单独拉取，并存储在 Docker 的本地存储区域中，通常位于 Linux 主机的 `/var/lib/docker/` 目录。可以在此示例中看到这些层的拉取过程：

```console
$ docker pull ubuntu:22.04
22.04: Pulling from library/ubuntu
f476d66f5408: Pull complete
8882c27f669e: Pull complete
d9af21273955: Pull complete
f5029279ec12: Pull complete
Digest: sha256:6120be6a2b7ce665d0cbddc3ce6eae60fe94637c6a66985312d1f02f63cc0bcd
Status: Downloaded newer image for ubuntu:22.04
docker.io/library/ubuntu:22.04
```

Each of these layers is stored in its own directory inside the Docker host's local storage area. To examine the layers on the filesystem, list the contents of `/var/lib/docker/<storage-driver>`. This example uses the `overlay2` storage driver:

​	每一层都存储在 Docker 主机本地存储区域的单独目录中。要检查文件系统中的层内容，可以列出 `/var/lib/docker/<storage-driver>` 的内容。此示例使用 `overlay2` 存储驱动程序：



```console
$ ls /var/lib/docker/overlay2
16802227a96c24dcbeab5b37821e2b67a9f921749cd9a2e386d5a6d5bc6fc6d3
377d73dbb466e0bc7c9ee23166771b35ebdbe02ef17753d79fd3571d4ce659d7
3f02d96212b03e3383160d31d7c6aeca750d2d8a1879965b89fe8146594c453d
ec1ec45792908e90484f7e629330666e7eee599f08729c93890a7205a6ba35f5
l
```

The directory names don't correspond to the layer IDs.

​	目录名并不对应于层 ID。

Now imagine that you have two different Dockerfiles. You use the first one to create an image called `acme/my-base-image:1.0`.

​	假设您有两个不同的 Dockerfile。第一个用于创建一个名为 `acme/my-base-image:1.0` 的镜像。

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN apk add --no-cache bash
```

The second one is based on `acme/my-base-image:1.0`, but has some additional layers:

​	第二个基于 `acme/my-base-image:1.0`，但增加了额外的层：

```dockerfile
# syntax=docker/dockerfile:1
FROM acme/my-base-image:1.0
COPY . /app
RUN chmod +x /app/hello.sh
CMD /app/hello.sh
```

The second image contains all the layers from the first image, plus new layers created by the `COPY` and `RUN` instructions, and a read-write container layer. Docker already has all the layers from the first image, so it doesn't need to pull them again. The two images share any layers they have in common.

​	第二个镜像包含了第一个镜像的所有层，以及 `COPY` 和 `RUN` 指令创建的新层，以及一个可读写的容器层。Docker 已经拥有了第一个镜像的所有层，因此无需再次拉取。两个镜像共享所有相同的层。

If you build images from the two Dockerfiles, you can use `docker image ls` and `docker image history` commands to verify that the cryptographic IDs of the shared layers are the same.

​	如果您从这两个 Dockerfile 构建镜像，可以使用 `docker image ls` 和 `docker image history` 命令验证共享层的加密 ID 是否相同。

1. Make a new directory `cow-test/` and change into it. 创建一个名为 `cow-test/` 的新目录并进入该目录。

2. Within `cow-test/`, create a new file called `hello.sh` with the following contents. 在 `cow-test/` 目录中，创建一个名为 `hello.sh` 的新文件，内容如下：

   

   ```bash
   #!/usr/bin/env bash
   echo "Hello world"
   ```

3. Copy the contents of the first Dockerfile above into a new file called `Dockerfile.base`. 将第一个 Dockerfile 的内容复制到一个名为 `Dockerfile.base` 的新文件中。

4. Copy the contents of the second Dockerfile above into a new file called `Dockerfile`. 将第二个 Dockerfile 的内容复制到一个名为 `Dockerfile` 的新文件中。

5. Within the `cow-test/` directory, build the first image. Don't forget to include the final `.` in the command. That sets the `PATH`, which tells Docker where to look for any files that need to be added to the image. 在 `cow-test/` 目录中构建第一个镜像。别忘了命令中的最后一个 `.`。这会设置 `PATH`，告诉 Docker 在哪里查找需要添加到镜像中的文件。

   

   ```console
   $ docker build -t acme/my-base-image:1.0 -f Dockerfile.base .
   [+] Building 6.0s (11/11) FINISHED
   => [internal] load build definition from Dockerfile.base                                      0.4s
   => => transferring dockerfile: 116B                                                           0.0s
   => [internal] load .dockerignore                                                              0.3s
   => => transferring context: 2B                                                                0.0s
   => resolve image config for docker.io/docker/dockerfile:1                                     1.5s
   => [auth] docker/dockerfile:pull token for registry-1.docker.io                               0.0s
   => CACHED docker-image://docker.io/docker/dockerfile:1@sha256:9e2c9eca7367393aecc68795c671... 0.0s
   => [internal] load .dockerignore                                                              0.0s
   => [internal] load build definition from Dockerfile.base                                      0.0s
   => [internal] load metadata for docker.io/library/alpine:latest                               0.0s
   => CACHED [1/2] FROM docker.io/library/alpine                                                 0.0s
   => [2/2] RUN apk add --no-cache bash                                                          3.1s
   => exporting to image                                                                         0.2s
   => => exporting layers                                                                        0.2s
   => => writing image sha256:da3cf8df55ee9777ddcd5afc40fffc3ead816bda99430bad2257de4459625eaa   0.0s
   => => naming to docker.io/acme/my-base-image:1.0                                              0.0s
   ```

6. Build the second image.

   构建第二个镜像：

   ```console
   $ docker build -t acme/my-final-image:1.0 -f Dockerfile .
   
   [+] Building 3.6s (12/12) FINISHED
   => [internal] load build definition from Dockerfile                                            0.1s
   => => transferring dockerfile: 156B                                                            0.0s
   => [internal] load .dockerignore                                                               0.1s
   => => transferring context: 2B                                                                 0.0s
   => resolve image config for docker.io/docker/dockerfile:1                                      0.5s
   => CACHED docker-image://docker.io/docker/dockerfile:1@sha256:9e2c9eca7367393aecc68795c671...  0.0s
   => [internal] load .dockerignore                                                               0.0s
   => [internal] load build definition from Dockerfile                                            0.0s
   => [internal] load metadata for docker.io/acme/my-base-image:1.0                               0.0s
   => [internal] load build context                                                               0.2s
   => => transferring context: 340B                                                               0.0s
   => [1/3] FROM docker.io/acme/my-base-image:1.0                                                 0.2s
   => [2/3] COPY . /app                                                                           0.1s
   => [3/3] RUN chmod +x /app/hello.sh                                                            0.4s
   => exporting to image                                                                          0.1s
   => => exporting layers                                                                         0.1s
   => => writing image sha256:8bd85c42fa7ff6b33902ada7dcefaaae112bf5673873a089d73583b0074313dd    0.0s
   => => naming to docker.io/acme/my-final-image:1.0                                              0.0s
   ```

7. Check out the sizes of the images.

   查看镜像大小：

   ```console
   $ docker image ls
   
   REPOSITORY             TAG     IMAGE ID         CREATED               SIZE
   acme/my-final-image    1.0     8bd85c42fa7f     About a minute ago    7.75MB
   acme/my-base-image     1.0     da3cf8df55ee     2 minutes ago         7.75MB
   ```

8. Check out the history of each image.

   检查每个镜像的历史记录：

   ```console
   $ docker image history acme/my-base-image:1.0
   
   IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
   da3cf8df55ee   5 minutes ago   RUN /bin/sh -c apk add --no-cache bash # bui…   2.15MB    buildkit.dockerfile.v0
   <missing>      7 weeks ago     /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
   <missing>      7 weeks ago     /bin/sh -c #(nop) ADD file:f278386b0cef68136…   5.6MB
   ```

   Some steps don't have a size (`0B`), and are metadata-only changes, which do not produce an image layer and don't take up any size, other than the metadata itself. The output above shows that this image consists of 2 image layers.

   有些步骤没有大小（`0B`），它们只是元数据更改，不会生成镜像层，也不会占用任何空间，除了元数据本身。上面的输出显示该镜像由两层镜像层组成。

   ```console
   $ docker image history  acme/my-final-image:1.0
   
   IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
   8bd85c42fa7f   3 minutes ago   CMD ["/bin/sh" "-c" "/app/hello.sh"]            0B        buildkit.dockerfile.v0
   <missing>      3 minutes ago   RUN /bin/sh -c chmod +x /app/hello.sh # buil…   39B       buildkit.dockerfile.v0
   <missing>      3 minutes ago   COPY . /app # buildkit                          222B      buildkit.dockerfile.v0
   <missing>      4 minutes ago   RUN /bin/sh -c apk add --no-cache bash # bui…   2.15MB    buildkit.dockerfile.v0
   <missing>      7 weeks ago     /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
   <missing>      7 weeks ago     /bin/sh -c #(nop) ADD file:f278386b0cef68136…   5.6MB
   ```

   Notice that all steps of the first image are also included in the final image. The final image includes the two layers from the first image, and two layers that were added in the second image.

   ​	注意，最终镜像中也包含了第一个镜像的所有步骤。最终镜像包括来自第一个镜像的两个层以及在第二个镜像中添加的两个层。

   The `<missing>` lines in the `docker history` output indicate that those steps were either built on another system and part of the `alpine` image that was pulled from Docker Hub, or were built with BuildKit as builder. Before BuildKit, the "classic" builder would produce a new "intermediate" image for each step for caching purposes, and the `IMAGE` column would show the ID of that image.

   ​	`docker history` 输出中的 `<missing>` 行表明这些步骤要么是在另一个系统上构建的并作为从 Docker Hub 拉取的 `alpine` 镜像的一部分，要么是使用 BuildKit 构建的。在 BuildKit 之前，“经典”构建器会为每一步生成一个新的“中间”镜像以供缓存，并且 `IMAGE` 列会显示该镜像的 ID。

   BuildKit uses its own caching mechanism, and no longer requires intermediate images for caching. Refer to [BuildKit]({{< ref "/manuals/DockerBuild/BuildKit" >}}) to learn more about other enhancements made in BuildKit.

   ​	BuildKit 使用自己的缓存机制，不再需要用于缓存的中间镜像。有关 BuildKit 的其他增强功能，请参考 [BuildKit]({{< ref "/manuals/DockerBuild/BuildKit" >}})。

9. Check out the layers for each image 查看每个镜像的层

   Use the `docker image inspect` command to view the cryptographic IDs of the layers in each image:

   使用 `docker image inspect` 命令查看每个镜像层的加密 ID：

   ```console
   $ docker image inspect --format "{{json .RootFS.Layers}}" acme/my-base-image:1.0
   [
     "sha256:72e830a4dff5f0d5225cdc0a320e85ab1ce06ea5673acfe8d83a7645cbd0e9cf",
     "sha256:07b4a9068b6af337e8b8f1f1dae3dd14185b2c0003a9a1f0a6fd2587495b204a"
   ]
   ```

   

   ```console
   $ docker image inspect --format "{{json .RootFS.Layers}}" acme/my-final-image:1.0
   [
     "sha256:72e830a4dff5f0d5225cdc0a320e85ab1ce06ea5673acfe8d83a7645cbd0e9cf",
     "sha256:07b4a9068b6af337e8b8f1f1dae3dd14185b2c0003a9a1f0a6fd2587495b204a",
     "sha256:cc644054967e516db4689b5282ee98e4bc4b11ea2255c9630309f559ab96562e",
     "sha256:e84fb818852626e89a09f5143dbc31fe7f0e0a6a24cd8d2eb68062b904337af4"
   ]
   ```

   Notice that the first two layers are identical in both images. The second image adds two additional layers. Shared image layers are only stored once in `/var/lib/docker/` and are also shared when pushing and pulling an image to an image registry. Shared image layers can therefore reduce network bandwidth and storage.

   ​	注意，这两个镜像的前两层是相同的。第二个镜像增加了另外两层。共享镜像层在 `/var/lib/docker/` 中仅存储一次，并在推送和拉取镜像到镜像仓库时共享。共享镜像层因此可以减少网络带宽和存储需求。
   
   > **Tip**
   >
   > 
   >
   > Format output of Docker commands with the `--format` option.
   >
   > ​	使用 `--format` 选项格式化 Docker 命令的输出。
   >
   > The examples above use the `docker image inspect` command with the `--format` option to view the layer IDs, formatted as a JSON array. The `--format` option on Docker commands can be a powerful feature that allows you to extract and format specific information from the output, without requiring additional tools such as `awk` or `sed`. To learn more about formatting the output of docker commands using the `--format` flag, refer to the [format command and log output section]({{< ref "/manuals/DockerEngine/CLI/Formatcommandandlogoutput" >}}). We also pretty-printed the JSON output using the [`jq` utility](https://stedolan.github.io/jq/) for readability.
   >
   > ​	上述示例使用 `docker image inspect` 命令和 `--format` 选项，以 JSON 数组格式查看层 ID。Docker 命令中的 `--format` 选项是一个强大的功能，允许你从输出中提取并格式化特定信息，无需额外工具如 `awk` 或 `sed`。要了解更多关于使用 `--format` 标志格式化 Docker 命令输出的信息，请参考 [format command and log output section]({{< ref "/manuals/DockerEngine/CLI/Formatcommandandlogoutput" >}})。我们还使用 [`jq` 工具](https://stedolan.github.io/jq/) 以便于阅读的方式格式化 JSON 输出。

### 复制使容器更高效 Copying makes containers efficient

When you start a container, a thin writable container layer is added on top of the other layers. Any changes the container makes to the filesystem are stored here. Any files the container doesn't change don't get copied to this writable layer. This means that the writable layer is as small as possible.

​	当启动容器时，一个薄的可写容器层会被添加在其他层之上。容器对文件系统的任何更改都存储在这里。容器未更改的文件不会复制到该可写层中，这意味着可写层尽可能小。

When an existing file in a container is modified, the storage driver performs a copy-on-write operation. The specific steps involved depend on the specific storage driver. For the `overlay2` driver, the copy-on-write operation follows this rough sequence:

​	当修改容器中的现有文件时，存储驱动程序执行写时复制操作。具体步骤取决于存储驱动程序。例如，`overlay2` 驱动的写时复制操作大致遵循以下过程：

- Search through the image layers for the file to update. The process starts at the newest layer and works down to the base layer one layer at a time. When results are found, they're added to a cache to speed future operations.
  - 在镜像层中搜索要更新的文件。过程从最新层开始，一层层向下搜索到基础层。当找到结果时，将它们添加到缓存中以加速后续操作。

- Perform a `copy_up` operation on the first copy of the file that's found, to copy the file to the container's writable layer.
  - 对找到的文件执行 `copy_up` 操作，将文件复制到容器的可写层。

- Any modifications are made to this copy of the file, and the container can't see the read-only copy of the file that exists in the lower layer.
  - 对文件的任何修改都应用于这个副本，容器无法看到存在于下层中的只读文件副本。


Btrfs, ZFS, and other drivers handle the copy-on-write differently. You can read more about the methods of these drivers later in their detailed descriptions.

​	Btrfs、ZFS 和其他驱动程序的写时复制方法不同。可以在后续详细说明中了解这些驱动程序的方法。

Containers that write a lot of data consume more space than containers that don't. This is because most write operations consume new space in the container's thin writable top layer. Note that changing the metadata of files, for example, changing file permissions or ownership of a file, can also result in a `copy_up` operation, therefore duplicating the file to the writable layer.

​	写入大量数据的容器会消耗更多空间，因为大多数写操作会在容器的薄可写顶层中消耗新空间。请注意，修改文件的元数据（例如更改文件权限或文件所有权）也可能导致 `copy_up` 操作，从而将文件复制到可写层。

> **Tip**
>
> 
>
> Use volumes for write-heavy applications.
>
> ​	对于写密集型应用，请使用卷。
>
> Don't store the data in the container for write-heavy applications. Such applications, for example write-intensive databases, are known to be problematic particularly when pre-existing data exists in the read-only layer.
>
> ​	不要在写密集型应用中将数据存储在容器中。例如，写入密集的数据库在存在预先存在的数据的只读层时尤其容易出现问题。
>
> Instead, use Docker volumes, which are independent of the running container, and designed to be efficient for I/O. In addition, volumes can be shared among containers and don't increase the size of your container's writable layer. Refer to the [use volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) section to learn about volumes.
>
> ​	相反，使用 Docker 卷，这些卷独立于运行中的容器，并为 I/O 进行了高效设计。此外，卷可以在容器之间共享，不会增加容器可写层的大小。有关卷的更多信息，请参考 [使用卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})。

A `copy_up` operation can incur a noticeable performance overhead. This overhead is different depending on which storage driver is in use. Large files, lots of layers, and deep directory trees can make the impact more noticeable. This is mitigated by the fact that each `copy_up` operation only occurs the first time a given file is modified.

​	`copy_up` 操作可能会带来明显的性能开销。此开销因所用存储驱动程序而异。大文件、大量层和深目录结构可能会使影响更加明显。然而，每个 `copy_up` 操作仅在首次修改特定文件时发生。

To verify the way that copy-on-write works, the following procedure spins up 5 containers based on the `acme/my-final-image:1.0` image we built earlier and examines how much room they take up.

​	为了验证写时复制的工作方式，以下过程基于我们之前构建的 `acme/my-final-image:1.0` 镜像启动了 5 个容器，并检查了它们占用的空间。

1. From a terminal on your Docker host, run the following `docker run` commands. The strings at the end are the IDs of each container.

   在 Docker 主机的终端中运行以下 `docker run` 命令。命令末尾的字符串是每个容器的 ID。

   ```console
   $ docker run -dit --name my_container_1 acme/my-final-image:1.0 bash \
     && docker run -dit --name my_container_2 acme/my-final-image:1.0 bash \
     && docker run -dit --name my_container_3 acme/my-final-image:1.0 bash \
     && docker run -dit --name my_container_4 acme/my-final-image:1.0 bash \
     && docker run -dit --name my_container_5 acme/my-final-image:1.0 bash
   
   40ebdd7634162eb42bdb1ba76a395095527e9c0aa40348e6c325bd0aa289423c
   a5ff32e2b551168b9498870faf16c9cd0af820edf8a5c157f7b80da59d01a107
   3ed3c1a10430e09f253704116965b01ca920202d52f3bf381fbb833b8ae356bc
   939b3bf9e7ece24bcffec57d974c939da2bdcc6a5077b5459c897c1e2fa37a39
   cddae31c314fbab3f7eabeb9b26733838187abc9a2ed53f97bd5b04cd7984a5a
   ```

2. Run the `docker ps` command with the `--size` option to verify the 5 containers are running, and to see each container's size.

   使用 `--size` 选项运行 `docker ps` 命令以验证这 5 个容器正在运行，并查看每个容器的大小。

   ```console
   $ docker ps --size --format "table {{.ID}}\t{{.Image}}\t{{.Names}}\t{{.Size}}"
   
   CONTAINER ID   IMAGE                     NAMES            SIZE
   cddae31c314f   acme/my-final-image:1.0   my_container_5   0B (virtual 7.75MB)
   939b3bf9e7ec   acme/my-final-image:1.0   my_container_4   0B (virtual 7.75MB)
   3ed3c1a10430   acme/my-final-image:1.0   my_container_3   0B (virtual 7.75MB)
   a5ff32e2b551   acme/my-final-image:1.0   my_container_2   0B (virtual 7.75MB)
   40ebdd763416   acme/my-final-image:1.0   my_container_1   0B (virtual 7.75MB)
   ```

   The output above shows that all containers share the image's read-only layers (7.75MB), but no data was written to the container's filesystem, so no additional storage is used for the containers.

   ​	上述输出显示，所有容器共享镜像的只读层（7.75MB），但没有数据写入容器的文件系统，因此没有使用额外的存储空间。

3. Per-container storage 每个容器的存储

   To demonstrate this, run the following command to write the word 'hello' to a file on the container's writable layer in containers `my_container_1`, `my_container_2`, and `my_container_3`:

   要演示这一点，运行以下命令，在容器 `my_container_1`、`my_container_2` 和 `my_container_3` 的可写层中写入单词“hello”：

   ```console
   $ for i in {1..3}; do docker exec my_container_$i sh -c 'printf hello > /out.txt'; done
   ```

   Running the `docker ps` command again afterward shows that those containers now consume 5 bytes each. This data is unique to each container, and not shared. The read-only layers of the containers aren't affected, and are still shared by all containers.

   再次运行 `docker ps` 命令后，这些容器现在每个消耗 5 字节。这些数据对每个容器都是唯一的，不会共享。容器的只读层未受影响，仍由所有容器共享。

   ```console
   $ docker ps --size --format "table {{.ID}}\t{{.Image}}\t{{.Names}}\t{{.Size}}"
   
   CONTAINER ID   IMAGE                     NAMES            SIZE
   cddae31c314f   acme/my-final-image:1.0   my_container_5   0B (virtual 7.75MB)
   939b3bf9e7ec   acme/my-final-image:1.0   my_container_4   0B (virtual 7.75MB)
   3ed3c1a10430   acme/my-final-image:1.0   my_container_3   5B (virtual 7.75MB)
   a5ff32e2b551   acme/my-final-image:1.0   my_container_2   5B (virtual 7.75MB)
   40ebdd763416   acme/my-final-image:1.0   my_container_1   5B (virtual 7.75MB)
   ```

The previous examples illustrate how copy-on-write filesystems help make containers efficient. Not only does copy-on-write save space, but it also reduces container start-up time. When you create a container (or multiple containers from the same image), Docker only needs to create the thin writable container layer.

​	前面的示例说明了写时复制文件系统如何帮助容器提高效率。写时复制不仅节省了空间，还缩短了容器的启动时间。当创建一个容器（或从同一镜像创建多个容器）时，Docker 只需创建一个薄的可写容器层。

If Docker had to make an entire copy of the underlying image stack each time it created a new container, container creation times and disk space used would be significantly increased. This would be similar to the way that virtual machines work, with one or more virtual disks per virtual machine. The [`vfs` storage]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/VFSstoragedriver" >}}) doesn't provide a CoW filesystem or other optimizations. When using this storage driver, a full copy of the image's data is created for each container.

​	如果 Docker 每次创建新容器时都要完整复制底层镜像堆栈，那么容器的创建时间和磁盘空间消耗将显著增加。这类似于虚拟机的工作方式，即每个虚拟机都拥有一个或多个虚拟磁盘。[`vfs` 存储驱动]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/VFSstoragedriver" >}}) 不提供写时复制文件系统或其他优化。在使用此存储驱动时，每个容器都会创建镜像数据的完整副本。

## 相关信息 Related information

- [Volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})
  - [卷 (Volumes)]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})
- [Select a storage driver]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/Selectastoragedriver" >}})
  - [选择存储驱动]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/Selectastoragedriver" >}})
