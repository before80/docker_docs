+++
title = "docker pull"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/pull/](https://docs.docker.com/reference/cli/docker/image/pull/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image pull

| Description | Download an image from a registry                |
| :---------- | ------------------------------------------------ |
| Usage       | `docker image pull [OPTIONS] NAME[:TAG|@DIGEST]` |
| Aliases     | `docker pull`                                    |

## Description

Most of your images will be created on top of a base image from the [Docker Hub](https://hub.docker.com/) registry.

​	大多数镜像会基于来自 [Docker Hub](https://hub.docker.com/) 注册表的基础镜像创建。

[Docker Hub](https://hub.docker.com/) contains many pre-built images that you can `pull` and try without needing to define and configure your own.

​	[Docker Hub](https://hub.docker.com/) 包含许多预构建的镜像，你可以 `pull` 并尝试使用它们，而无需自行定义和配置。

To download a particular image, or set of images (i.e., a repository), use `docker pull`.

​	要下载特定镜像或一组镜像（即一个仓库），可以使用 `docker pull`。

### 代理配置 Proxy configuration

If you are behind an HTTP proxy server, for example in corporate settings, before open a connect to registry, you may need to configure the Docker daemon's proxy settings, refer to the [dockerd command-line reference](https://docs.docker.com/reference/cli/dockerd/#proxy-configuration) for details.

​	如果你位于 HTTP 代理服务器后，例如在企业环境中，在连接到注册表之前，可能需要配置 Docker 守护进程的代理设置，详情请参阅 [dockerd 命令行参考](https://docs.docker.com/reference/cli/dockerd/#proxy-configuration)。

### 并行下载 Concurrent downloads

By default the Docker daemon will pull three layers of an image at a time. If you are on a low bandwidth connection this may cause timeout issues and you may want to lower this via the `--max-concurrent-downloads` daemon option. See the [daemon documentation]({{< ref "/reference/CLIreference/dockerd" >}}) for more details.

​	默认情况下，Docker 守护进程一次会拉取镜像的三个层。如果你的网络带宽较低，这可能会导致超时问题，你可以通过 `--max-concurrent-downloads` 守护进程选项来降低该值。详情请参阅 [守护进程文档]({{< ref "/reference/CLIreference/dockerd" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-a, --all-tags`](https://docs.docker.com/reference/cli/docker/image/pull/#all-tags) |         | 下载仓库中的所有标记镜像 Download all tagged images in the repository |
| `--disable-content-trust`                                    | `true`  | 跳过镜像验证 Skip image verification                         |
| `--platform`                                                 |         | API 1.32+ 如果服务器支持多平台，设置平台 API 1.32+ Set platform if server is multi-platform capable |
| `-q, --quiet`                                                |         | 禁用详细输出 Suppress verbose output                         |

## Examples

### 从 Docker Hub 拉取镜像 Pull an image from Docker Hub

To download a particular image, or set of images (i.e., a repository), use `docker image pull` (or the `docker pull` shorthand). If no tag is provided, Docker Engine uses the `:latest` tag as a default. This example pulls the `debian:latest` image:

​	要下载特定镜像或一组镜像（即一个仓库），可以使用 `docker image pull`（或简写为 `docker pull`）。如果没有提供标签，Docker 引擎会默认使用 `:latest` 标签。以下示例拉取了 `debian:latest` 镜像：



```console
$ docker image pull debian

Using default tag: latest
latest: Pulling from library/debian
e756f3fdd6a3: Pull complete
Digest: sha256:3f1d6c17773a45c97bd8f158d665c9709d7b29ed7917ac934086ad96f92e4510
Status: Downloaded newer image for debian:latest
docker.io/library/debian:latest
```

Docker images can consist of multiple layers. In the example above, the image consists of a single layer; `e756f3fdd6a3`.

​	Docker 镜像可以由多个层组成。上述示例中的镜像由单个层 `e756f3fdd6a3` 组成。

Layers can be reused by images. For example, the `debian:bookworm` image shares its layer with the `debian:latest`. Pulling the `debian:bookworm` image therefore only pulls its metadata, but not its layers, because the layer is already present locally:

​	镜像层可以被不同镜像重用。例如，`debian:bookworm` 镜像共享了其层与 `debian:latest` 镜像，因此拉取 `debian:bookworm` 镜像只需拉取其元数据，而无需再拉取层，因为该层已经存在于本地：



```console
$ docker image pull debian:bookworm

bookworm: Pulling from library/debian
Digest: sha256:3f1d6c17773a45c97bd8f158d665c9709d7b29ed7917ac934086ad96f92e4510
Status: Downloaded newer image for debian:bookworm
docker.io/library/debian:bookworm
```

To see which images are present locally, use the [`docker images`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimages" >}}) command:

​	要查看本地存在哪些镜像，可以使用 [`docker images`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimages" >}}) 命令：



```console
$ docker images

REPOSITORY   TAG        IMAGE ID       CREATED        SIZE
debian       bookworm   4eacea30377a   8 days ago     124MB
debian       latest     4eacea30377a   8 days ago     124MB
```

Docker uses a content-addressable image store, and the image ID is a SHA256 digest covering the image's configuration and layers. In the example above, `debian:bookworm` and `debian:latest` have the same image ID because they are the same image tagged with different names. Because they are the same image, their layers are stored only once and do not consume extra disk space.

​	Docker 使用内容可寻址镜像存储，镜像 ID 是覆盖镜像配置和层的 SHA256 摘要。上述示例中，`debian:bookworm` 和 `debian:latest` 具有相同的镜像 ID，因为它们是同一个镜像，只是使用了不同的标签。由于它们是相同的镜像，其层仅存储一次，不会占用额外的磁盘空间。

For more information about images, layers, and the content-addressable store, refer to [understand images, containers, and storage drivers]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}}).

​	有关镜像、层和内容可寻址存储的更多信息，请参考 [理解镜像、容器和存储驱动程序]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}})。

### 按摘要（不可变标识符）拉取镜像 Pull an image by digest (immutable identifier)

So far, you've pulled images by their name (and "tag"). Using names and tags is a convenient way to work with images. When using tags, you can `docker pull` an image again to make sure you have the most up-to-date version of that image. For example, `docker pull ubuntu:24.04` pulls the latest version of the Ubuntu 24.04 image.

​	目前为止，你是通过名称（和“标签”）拉取镜像的。使用名称和标签是处理镜像的一种方便方法。当使用标签时，你可以再次 `docker pull` 镜像以确保拥有该镜像的最新版本。例如，`docker pull ubuntu:24.04` 会拉取最新的 Ubuntu 24.04 镜像版本。

In some cases you don't want images to be updated to newer versions, but prefer to use a fixed version of an image. Docker enables you to pull an image by its digest. When pulling an image by digest, you specify exactly which version of an image to pull. Doing so, allows you to "pin" an image to that version, and guarantee that the image you're using is always the same.

​	在某些情况下，你不希望镜像更新到新版本，而是希望使用镜像的固定版本。Docker 支持按摘要拉取镜像。当按摘要拉取镜像时，你可以精确指定要拉取的镜像版本，从而“锁定”到该版本，确保使用的镜像始终相同。

To know the digest of an image, pull the image first. Let's pull the latest `ubuntu:24.04` image from Docker Hub:

​	要了解镜像的摘要，可以先拉取镜像。以下示例从 Docker Hub 拉取最新的 `ubuntu:24.04` 镜像：

```console
$ docker pull ubuntu:24.04

24.04: Pulling from library/ubuntu
125a6e411906: Pull complete
Digest: sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30
Status: Downloaded newer image for ubuntu:24.04
docker.io/library/ubuntu:24.04
```

Docker prints the digest of the image after the pull has finished. In the example above, the digest of the image is:

​	在拉取完成后，Docker 会打印出镜像的摘要。在上述示例中，镜像的摘要是：

```console
sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30
```

Docker also prints the digest of an image when pushing to a registry. This may be useful if you want to pin to a version of the image you just pushed.

​	Docker 在推送镜像至注册表时也会打印镜像的摘要。如果你希望锁定到刚推送的镜像版本，这可能会很有用。

A digest takes the place of the tag when pulling an image, for example, to pull the above image by digest, run the following command:

​	摘要可用于取代标签来拉取镜像，例如，通过以下命令按摘要拉取上述镜像：

```console
$ docker pull ubuntu@sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30

docker.io/library/ubuntu@sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30: Pulling from library/ubuntu
Digest: sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30
Status: Image is up to date for ubuntu@sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30
docker.io/library/ubuntu@sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30
```

Digest can also be used in the `FROM` of a Dockerfile, for example:

​	摘要还可以用于 Dockerfile 中的 `FROM` 指令，例如：

```dockerfile
FROM ubuntu@sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30
LABEL org.opencontainers.image.authors="some maintainer <maintainer@example.com>"
```

> **Note**
>
> Using this feature "pins" an image to a specific version in time. Docker does therefore not pull updated versions of an image, which may include security updates. If you want to pull an updated image, you need to change the digest accordingly.
>
> ​	使用此功能会将镜像“锁定”到特定的时间点版本。因此 Docker 不会拉取该镜像的更新版本，这些更新可能包含安全更新。如果需要拉取更新的镜像，需相应更改摘要。

### 从其他注册表拉取 Pull from a different registry

By default, `docker pull` pulls images from [Docker Hub](https://hub.docker.com/). It is also possible to manually specify the path of a registry to pull from. For example, if you have set up a local registry, you can specify its path to pull from it. A registry path is similar to a URL, but does not contain a protocol specifier (`https://`).

​	默认情况下，`docker pull` 从 [Docker Hub](https://hub.docker.com/) 拉取镜像。你也可以手动指定注册表路径来从其他注册表拉取镜像。例如，如果你已设置本地注册表，则可以指定其路径来从中拉取镜像。注册表路径类似于 URL，但不包含协议说明符（`https://`）。

The following command pulls the `testing/test-image` image from a local registry listening on port 5000 (`myregistry.local:5000`):

​	以下命令从在端口 5000 监听的本地注册表 `myregistry.local:5000` 中拉取 `testing/test-image` 镜像：



```console
$ docker image pull myregistry.local:5000/testing/test-image
```

Registry credentials are managed by [docker login]({{< ref "/reference/CLIreference/docker/dockerlogin" >}}).

​	注册表凭证由 [docker login]({{< ref "/reference/CLIreference/docker/dockerlogin" >}}) 管理。

Docker uses the `https://` protocol to communicate with a registry, unless the registry is allowed to be accessed over an insecure connection. Refer to the [insecure registries](https://docs.docker.com/reference/cli/dockerd/#insecure-registries) section for more information.

​	Docker 使用 `https://` 协议与注册表通信，除非该注册表允许通过不安全连接访问。详情请参阅 [不安全注册表](https://docs.docker.com/reference/cli/dockerd/#insecure-registries) 部分。

### 拉取包含多个镜像的仓库 (`-a`, `--all-tags`) Pull a repository with multiple images (-a, --all-tags)

By default, `docker pull` pulls a single image from the registry. A repository can contain multiple images. To pull all images from a repository, provide the `-a` (or `--all-tags`) option when using `docker pull`.

​	默认情况下，`docker pull` 从注册表拉取单个镜像。一个仓库可以包含多个镜像。要从仓库拉取所有镜像，请在 `docker pull` 中提供 `-a`（或 `--all-tags`）选项。

This command pulls all images from the `ubuntu` repository:

​	此命令从 `ubuntu` 仓库拉取所有镜像：



```console
$ docker image pull --all-tags ubuntu

Pulling repository ubuntu
ad57ef8d78d7: Download complete
105182bb5e8b: Download complete
511136ea3c5a: Download complete
73bd853d2ea5: Download complete
....

Status: Downloaded newer image for ubuntu
```

After the pull has completed use the `docker image ls` command (or the `docker images` shorthand) to see the images that were pulled. The example below shows all the `ubuntu` images that are present locally:

​	拉取完成后，使用 `docker image ls` 命令（或 `docker images` 简写）查看已拉取的镜像。以下示例显示了本地存在的所有 `ubuntu` 镜像：



```console
$ docker image ls --filter reference=ubuntu
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
ubuntu       22.04     8a3cdc4d1ad3   3 weeks ago    77.9MB
ubuntu       jammy     8a3cdc4d1ad3   3 weeks ago    77.9MB
ubuntu       24.04     35a88802559d   6 weeks ago    78.1MB
ubuntu       latest    35a88802559d   6 weeks ago    78.1MB
ubuntu       noble     35a88802559d   6 weeks ago    78.1MB
```

### 取消拉取操作 Cancel a pull

Killing the `docker pull` process, for example by pressing `CTRL-c` while it is running in a terminal, will terminate the pull operation.

​	在拉取过程中按 `CTRL-c` 终止 `docker pull` 进程，操作将停止。

```console
$ docker pull ubuntu

Using default tag: latest
latest: Pulling from library/ubuntu
a3ed95caeb02: Pulling fs layer
236608c7b546: Pulling fs layer
^C
```

The Engine terminates a pull operation when the connection between the daemon and the client (initiating the pull) is cut or lost for any reason or the command is manually terminated.

​	当守护进程与发起拉取的客户端之间的连接被切断或丢失，或命令被手动终止时，守护进程会终止拉取操作。
