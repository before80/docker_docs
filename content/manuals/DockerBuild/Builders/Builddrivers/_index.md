+++
title = "构建驱动"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/builders/drivers/](https://docs.docker.com/build/builders/drivers/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Build drivers - 构建驱动

Build drivers are configurations for how and where the BuildKit backend runs. Driver settings are customizable and allows fine-grained control of the builder. Buildx supports the following drivers:

​	构建驱动是关于 BuildKit 后端如何及在哪里运行的配置。驱动设置可定制，允许对构建器进行精细控制。Buildx 支持以下驱动：

- `docker`: uses the BuildKit library bundled into the Docker daemon.
  - `docker`：使用捆绑在 Docker 守护进程中的 BuildKit 库。

- `docker-container`: creates a dedicated BuildKit container using Docker.
  - `docker-container`：使用 Docker 创建一个专用的 BuildKit 容器。

- `kubernetes`: creates BuildKit pods in a Kubernetes cluster.
  - `kubernetes`：在 Kubernetes 集群中创建 BuildKit pods。

- `remote`: connects directly to a manually managed BuildKit daemon.
  - `remote`：直接连接到手动管理的 BuildKit 守护进程。


Different drivers support different use cases. The default `docker` driver prioritizes simplicity and ease of use. It has limited support for advanced features like caching and output formats, and isn't configurable. Other drivers provide more flexibility and are better at handling advanced scenarios.

​	不同的驱动支持不同的用例。默认的 `docker` 驱动优先考虑简便性和易用性。它对高级功能（如缓存和输出格式）的支持有限，并且不可配置。其他驱动提供了更大的灵活性，更适合处理复杂的场景。

The following table outlines some differences between drivers.

​	下表概述了各驱动之间的一些差异：

| Feature                                             | `docker` | `docker-container` | `kubernetes` |      `remote`      |
| :-------------------------------------------------- | :------: | :----------------: | :----------: | :----------------: |
| **Automatically load image **自动加载到本地镜像存储 |    ✅     |                    |              |                    |
| **Cache export** 缓存导出                           |    ✓*    |         ✅          |      ✅       |         ✅          |
| **Tarball output **Tarball 输出                     |          |         ✅          |      ✅       |         ✅          |
| **Multi-arch images **多架构镜像                    |          |         ✅          |      ✅       |         ✅          |
| **BuildKit configuration** BuildKit 配置            |          |         ✅          |      ✅       | Managed externally |

\* *The `docker` driver doesn't support all cache export options. See [Cache storage backends]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}}) for more information.*

​	`docker` 驱动不支持所有缓存导出选项。有关更多信息，请参阅[缓存存储后端]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}})。

## 加载到本地镜像存储 Loading to local image store

Unlike when using the default `docker` driver, images built using other drivers aren't automatically loaded into the local image store. If you don't specify an output, the build result is exported to the build cache only.

​	与默认的 `docker` 驱动不同，使用其他驱动构建的镜像不会自动加载到本地镜像存储中。如果未指定输出，构建结果将仅导出到构建缓存。

To build an image using a non-default driver and load it to the image store, use the `--load` flag with the build command:

​	要使用非默认驱动构建镜像并将其加载到镜像存储中，请在构建命令中使用 `--load` 标志：





```console
$ docker buildx build --load -t <image> --builder=container .
...
=> exporting to oci image format                                                                                                      7.7s
=> => exporting layers                                                                                                                4.9s
=> => exporting manifest sha256:4e4ca161fa338be2c303445411900ebbc5fc086153a0b846ac12996960b479d3                                      0.0s
=> => exporting config sha256:adf3eec768a14b6e183a1010cb96d91155a82fd722a1091440c88f3747f1f53f                                        0.0s
=> => sending tarball                                                                                                                 2.8s
=> importing to docker
```

With this option, the image is available in the image store after the build finishes:

​	使用此选项后，构建完成后镜像会在镜像存储中可用：





```console
$ docker image ls
REPOSITORY                       TAG               IMAGE ID       CREATED             SIZE
<image>                          latest            adf3eec768a1   2 minutes ago       197MB
```

### 默认加载 Load by default

Introduced in Buildx version 0.14.0

​	Buildx 0.14.0 引入

You can configure the custom build drivers to behave in a similar way to the default `docker` driver, and load images to the local image store by default. To do so, set the `default-load` driver option when creating the builder:

​	您可以配置自定义构建驱动，使其行为类似于默认的 `docker` 驱动，默认将镜像加载到本地镜像存储中。为此，请在创建构建器时设置 `default-load` 驱动选项：

```console
$ docker buildx create --driver-opt default-load=true
```

Note that, just like with the `docker` driver, if you specify a different output format with `--output`, the result will not be loaded to the image store unless you also explicitly specify `--output type=docker` or use the `--load` flag.

​	请注意，与 `docker` 驱动一样，如果您使用 `--output` 指定了不同的输出格式，结果将不会加载到镜像存储中，除非您也显式指定 `--output type=docker` 或使用 `--load` 标志。

## What's next



Read about each driver:

​	了解每种驱动：

- [Docker driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}})
  - [Docker 驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}})
- [Docker container driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}})
  - [Docker 容器驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}})
- [Kubernetes driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Kubernetesdriver" >}})
  - [Kubernetes 驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Kubernetesdriver" >}})
- [Remote driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Remotedriver" >}})
  - [远程驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Remotedriver" >}})
