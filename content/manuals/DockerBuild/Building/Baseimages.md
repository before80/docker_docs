+++
title = "基础镜像"
date = 2024-10-23T14:54:40+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/building/base-images/](https://docs.docker.com/build/building/base-images/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Base images - 基础镜像

All Dockerfiles start from a base image. A base is the image that your image extends. It refers to the contents of the `FROM` instruction in the Dockerfile.

​	所有 Dockerfile 都从基础镜像开始。基础镜像是您的镜像所扩展的镜像，它指的是 Dockerfile 中的 `FROM` 指令内容。





```dockerfile
FROM debian
```

For most cases, you don't need to create your own base image. Docker Hub contains a vast library of Docker images that are suitable for use as a base image in your build. [Docker Official Images]({{< ref "/manuals/Trustedcontent/DockerOfficialImages" >}}) are specifically designed as a set of hardened, battle-tested images that support a wide variety of platforms, languages, and frameworks. There are also [Docker Verified Publisher](https://hub.docker.com/search?q=&image_filter=store) images, created by trusted publishing partners, verified by Docker.

​	在大多数情况下，您不需要创建自己的基础镜像。Docker Hub 上有丰富的 Docker 镜像库，可以作为构建的基础镜像。[Docker 官方镜像]({{< ref "/manuals/Trustedcontent/DockerOfficialImages" >}})是一组经过强化和验证的镜像，支持多种平台、语言和框架。还有 [Docker 验证的发布者镜像](https://hub.docker.com/search?q=&image_filter=store)，由可信的合作伙伴发布并经过 Docker 验证。

## 创建基础镜像 Create a base image

If you need to completely control the contents of your image, you can create your own base image from a Linux distribution of your choosing, or use the special `FROM scratch` base:

​	如果您需要完全控制镜像的内容，可以从您选择的 Linux 发行版创建自己的基础镜像，或使用特殊的 `FROM scratch` 基础镜像：

```dockerfile
FROM scratch
```

The `scratch` image is typically used to create minimal images containing only just what an application needs. See [Create a minimal base image using scratch](https://docs.docker.com/build/building/base-images/#create-a-minimal-base-image-using-scratch).

​	`scratch` 镜像通常用于创建仅包含应用程序所需内容的最小镜像。请参阅[使用 scratch 创建最小基础镜像](https://docs.docker.com/build/building/base-images/#create-a-minimal-base-image-using-scratch)。

To create a distribution base image, you can use a root filesystem, packaged as a `tar` file, and import it to Docker with `docker import`. The process for creating your own base image depends on the Linux distribution you want to package. See [Create a full image using tar](https://docs.docker.com/build/building/base-images/#create-a-full-image-using-tar).

​	要创建一个发行版基础镜像，可以使用打包成 `tar` 文件的根文件系统，并使用 `docker import` 导入到 Docker 中。创建自己基础镜像的过程取决于您想要打包的 Linux 发行版。请参阅[使用 tar 创建完整镜像](https://docs.docker.com/build/building/base-images/#create-a-full-image-using-tar)。

## 使用 scratch 创建最小基础镜像 Create a minimal base image using scratch

The reserved, minimal `scratch` image serves as a starting point for building containers. Using the `scratch` image signals to the build process that you want the next command in the `Dockerfile` to be the first filesystem layer in your image.

​	保留的最小 `scratch` 镜像可作为构建容器的起点。使用 `scratch` 镜像表示您希望 `Dockerfile` 中的下一条命令作为镜像中的第一个文件系统层。

While `scratch` appears in Docker's [repository on Docker Hub](https://hub.docker.com/_/scratch), you can't pull it, run it, or tag any image with the name `scratch`. Instead, you can refer to it in your `Dockerfile`. For example, to create a minimal container using `scratch`:

​	虽然 `scratch` 出现在 Docker Hub 的[仓库](https://hub.docker.com/_/scratch)中，但您无法拉取、运行或标记任何名为 `scratch` 的镜像。相反，您可以在 `Dockerfile` 中引用它。例如，使用 `scratch` 创建一个最小容器：



```dockerfile
# syntax=docker/dockerfile:1
FROM scratch
ADD hello /
CMD ["/hello"]
```

Assuming an executable binary named `hello` exists at the root of the [build context]({{< ref "/manuals/DockerBuild/Coreconcepts/Buildcontext" >}}). You can build this Docker image using the following `docker build` command:

​	假设在[构建上下文]({{< ref "/manuals/DockerBuild/Coreconcepts/Buildcontext" >}})的根目录中有一个名为 `hello` 的可执行二进制文件。您可以使用以下 `docker build` 命令构建此 Docker 镜像：





```console
$ docker build --tag hello .
```

To run your new image, use the `docker run` command:

​	使用 `docker run` 命令运行您的新镜像：





```console
$ docker run --rm hello
```

This example image can only successfully execute as long as the `hello` binary doesn't have any runtime dependencies. Computer programs tend to depend on certain other programs or resources to exist in the runtime environment. For example:

​	这个示例镜像可以成功执行，但前提是 `hello` 二进制文件没有任何运行时依赖项。大多数程序依赖于特定的运行环境，例如：

- Programming language runtimes
  - 编程语言运行时

- Dynamically linked C libraries
  - 动态链接的 C 库

- CA certificates
  - CA 证书


When building a base image, or any image, this is an important aspect to consider. And this is why creating a base image using `FROM scratch` can be difficult, for anything other than small, simple programs. On the other hand, it's also important to include only the things you need in your image, to reduce the image size and attack surface.

​	在构建基础镜像或任何镜像时，这一点很重要。这也是为什么使用 `FROM scratch` 创建基础镜像除了小型简单程序外比较困难的原因。另一方面，仅包含所需内容有助于减小镜像大小并减少攻击面。

## 使用 tar 创建完整镜像 Create a full image using tar

In general, start with a working machine that is running the distribution you'd like to package as a base image, though that is not required for some tools like Debian's [Debootstrap](https://wiki.debian.org/Debootstrap), which you can also use to build Ubuntu images.

​	通常，您可以从正在运行目标发行版的机器开始，打包该发行版以创建基础镜像。不过，对于某些工具，例如 Debian 的 [Debootstrap](https://wiki.debian.org/Debootstrap)，可以直接在无操作系统的情况下构建 Ubuntu 镜像。

For example, to create an Ubuntu base image:

​	例如，创建一个 Ubuntu 基础镜像：





```dockerfile
$ sudo debootstrap focal focal > /dev/null
$ sudo tar -C focal -c . | docker import - focal

sha256:81ec9a55a92a5618161f68ae691d092bf14d700129093158297b3d01593f4ee3

$ docker run focal cat /etc/lsb-release

DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04 LTS"
```

There are more example scripts for creating base images in [the Moby GitHub repository](https://github.com/moby/moby/blob/master/contrib).

​	在 [Moby GitHub 仓库](https://github.com/moby/moby/blob/master/contrib)中有更多创建基础镜像的示例脚本。

## 更多资源 More resources

For more information about building images and writing Dockerfiles, see:

​	有关构建镜像和编写 Dockerfile 的更多信息，请参阅：

- [Dockerfile reference]({{< ref "/reference/Dockerfilereference" >}})
  - [Dockerfile 参考]({{< ref "/reference/Dockerfilereference" >}})

- [Dockerfile best practices]({{< ref "/manuals/DockerBuild/Building/Bestpractices" >}})

  - [Dockerfile 最佳实践]({{< ref "/manuals/DockerBuild/Building/Bestpractices" >}})
- [Docker Official Images]({{< ref "/manuals/Trustedcontent/DockerOfficialImages" >}})

  - [Docker 官方镜像]({{< ref "/manuals/Trustedcontent/DockerOfficialImages" >}})

  
