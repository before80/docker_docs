+++
title = "多平台"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/building/multi-platform/](https://docs.docker.com/build/building/multi-platform/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Multi-platform builds - 多平台构建

A multi-platform build refers to a single build invocation that targets multiple different operating system or CPU architecture combinations. When building images, this lets you create a single image that can run on multiple platforms, such as `linux/amd64`, `linux/arm64`, and `windows/amd64`.

​	多平台构建指的是单次构建调用可以针对多个不同的操作系统或 CPU 架构组合进行构建。构建镜像时，这种方法允许您创建一个可以在多个平台上运行的单个镜像，例如 `linux/amd64`、`linux/arm64` 和 `windows/amd64`。

## 为什么需要多平台构建？ Why multi-platform builds?

Docker solves the "it works on my machine" problem by packaging applications and their dependencies into containers. This makes it easy to run the same application on different environments, such as development, testing, and production.

​	Docker 通过将应用程序及其依赖项打包到容器中，解决了“在我的机器上可以运行”的问题。这使得在不同环境（如开发、测试和生产）中运行相同应用程序变得容易。

But containerization by itself only solves part of the problem. Containers share the host kernel, which means that the code that's running inside the container must be compatible with the host's architecture. This is why you can't run a `linux/amd64` container on an arm64 host (without using emulation), or a Windows container on a Linux host.

​	但是，仅仅依靠容器化并不能完全解决这个问题。容器共享主机内核，这意味着在容器内运行的代码必须与主机的架构兼容。因此，您无法在 arm64 主机上运行 `linux/amd64` 容器（除非使用模拟），也无法在 Linux 主机上运行 Windows 容器。

Multi-platform builds solve this problem by packaging multiple variants of the same application into a single image. This enables you to run the same image on different types of hardware, such as development machines running x86-64 or ARM-based Amazon EC2 instances in the cloud, without the need for emulation.

​	多平台构建通过将同一应用程序的多个变体打包到一个镜像中来解决这个问题。这使您能够在不同类型的硬件上运行相同的镜像，例如在运行 x86-64 或 ARM 架构的开发机器上，或在云中的基于 ARM 的 Amazon EC2 实例上运行，而无需使用模拟。

### 单平台和多平台镜像的区别 Difference between single-platform and multi-platform images

Multi-platform images have a different structure than single-platform images. Single-platform images contain a single manifest that points to a single configuration and a single set of layers. Multi-platform images contain a manifest list, pointing to multiple manifests, each of which points to a different configuration and set of layers.

​	多平台镜像的结构不同于单平台镜像。单平台镜像包含一个指向单一配置和层集的清单；多平台镜像包含一个清单列表，指向多个清单，每个清单指向不同的配置和层集。

![Multi-platform image structure](Multi-platform_img/single-vs-multiplatform-image.svg)

When you push a multi-platform image to a registry, the registry stores the manifest list and all the individual manifests. When you pull the image, the registry returns the manifest list, and Docker automatically selects the correct variant based on the host's architecture. For example, if you run a multi-platform image on an ARM-based Raspberry Pi, Docker selects the `linux/arm64` variant. If you run the same image on an x86-64 laptop, Docker selects the `linux/amd64` variant (if you're using Linux containers).

​	当您将多平台镜像推送到注册表时，注册表会存储清单列表和所有单独的清单。当您拉取镜像时，注册表返回清单列表，Docker 根据主机的架构自动选择正确的变体。例如，如果您在基于 ARM 的树莓派上运行多平台镜像，Docker 会选择 `linux/arm64` 变体；如果在 x86-64 的笔记本上运行相同的镜像，Docker 会选择 `linux/amd64` 变体（如果使用的是 Linux 容器）。

## 前置条件 Prerequisites

To build multi-platform images, you first need to make sure that your Docker environment is set up to support it. There are two ways you can do that:

​	构建多平台镜像时，首先需要确保您的 Docker 环境已设置支持该功能。您可以通过以下两种方式实现：

- You can switch from the "classic" image store to the containerd image store.
  - 将“经典”镜像存储切换到 containerd 镜像存储。

- You can create and use a custom builder.
  - 创建并使用自定义构建器。


The "classic" image store of the Docker Engine does not support multi-platform images. Switching to the containerd image store ensures that your Docker Engine can push, pull, and build multi-platform images.

​	Docker Engine 的“经典”镜像存储不支持多平台镜像。切换到 containerd 镜像存储可以确保您的 Docker Engine 可以推送、拉取并构建多平台镜像。

Creating a custom builder that uses a driver with multi-platform support, such as the `docker-container` driver, will let you build multi-platform images without switching to a different image store. However, you still won't be able to load the multi-platform images you build into your Docker Engine image store. But you can push them to a container registry directly with `docker build --push`.

​	创建一个使用支持多平台的驱动程序（例如 `docker-container` 驱动程序）的自定义构建器，让您无需切换到不同的镜像存储即可构建多平台镜像。不过，您构建的多平台镜像无法加载到 Docker Engine 镜像存储中，但可以使用 `docker build --push` 直接将它们推送到容器注册表中。



{{< tabpane text=true persist=disabled >}}

{{% tab header="containerd image store" %}}

The steps for enabling the containerd image store depends on whether you're using Docker Desktop or Docker Engine standalone:

​	启用 containerd 镜像存储的步骤取决于您使用的是 Docker Desktop 还是 Docker Engine 独立版：

- If you're using Docker Desktop, enable the containerd image store in the [Docker Desktop settings]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}}).
  - 如果您使用的是 Docker Desktop，请在 [Docker Desktop 设置]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}})中启用 containerd 镜像存储。

- If you're using Docker Engine standalone, enable the containerd image store using the [daemon configuration file]({{< ref "/manuals/DockerEngine/Storage/containerdimagestore" >}}).
  - 如果您使用的是 Docker Engine 独立版，请通过[守护进程配置文件]({{< ref "/manuals/DockerEngine/Storage/containerdimagestore" >}})启用 containerd 镜像存储。




{{% /tab  %}}

{{% tab header="Custom builder" %}}

To create a custom builder, use the `docker buildx create` command to create a builder that uses the `docker-container` driver.

​	使用 `docker buildx create` 命令创建一个使用 `docker-container` 驱动程序的构建器。

```console
$ docker buildx create \
  --name container-builder \
  --driver docker-container \
  --bootstrap --use
```

> **Note**
>
> Builds with the `docker-container` driver aren't automatically loaded to your Docker Engine image store. For more information, see [Build drivers]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}).
>
> ​	使用 `docker-container` 驱动程序的构建不会自动加载到 Docker Engine 镜像存储中。更多信息请参阅 [构建驱动程序]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}})。

{{% /tab  %}}

{{< /tabpane >}}

If you're using Docker Engine standalone and you need to build multi-platform images using emulation, you also need to install QEMU, see [Install QEMU manually](https://docs.docker.com/build/building/multi-platform/#install-qemu-manually).

​	如果您在 Docker Engine 独立版上使用仿真进行多平台构建，还需要安装 QEMU，请参阅[手动安装 QEMU](https://docs.docker.com/build/building/multi-platform/#install-qemu-manually)。

## 构建多平台镜像 Build multi-platform images

When triggering a build, use the `--platform` flag to define the target platforms for the build output, such as `linux/amd64` and `linux/arm64`:

​	触发构建时，使用 `--platform` 标志定义构建输出的目标平台，例如 `linux/amd64` 和 `linux/arm64`：

```console
$ docker buildx build --platform linux/amd64,linux/arm64 .
```

## 策略 Strategies

You can build multi-platform images using three different strategies, depending on your use case:

​	根据不同的用例，您可以使用以下三种策略来构建多平台镜像：

1. Using emulation, via [QEMU](https://docs.docker.com/build/building/multi-platform/#qemu) 通过 [QEMU](https://docs.docker.com/build/building/multi-platform/#qemu) 进行仿真
2. Use a builder with [multiple native nodes](https://docs.docker.com/build/building/multi-platform/#multiple-native-nodes) 使用具有[多个本地节点](https://docs.docker.com/build/building/multi-platform/#multiple-native-nodes)的构建器
3. Use [cross-compilation](https://docs.docker.com/build/building/multi-platform/#cross-compilation) with multi-stage builds 使用多阶段构建进行[交叉编译](https://docs.docker.com/build/building/multi-platform/#cross-compilation)

### QEMU

Building multi-platform images under emulation with QEMU is the easiest way to get started if your builder already supports it. Using emulation requires no changes to your Dockerfile, and BuildKit automatically detects the architectures that are available for emulation.

​	如果您的构建器已支持，使用 QEMU 仿真进行多平台构建是最简单的入门方式。使用仿真无需对 Dockerfile 做出更改，BuildKit 会自动检测可用于仿真的架构。

> **Note**
>
> 
>
> Emulation with QEMU can be much slower than native builds, especially for compute-heavy tasks like compilation and compression or decompression.
>
> ​	使用 QEMU 进行仿真比本地构建慢得多，特别是对于编译和压缩/解压等计算密集型任务。
>
> Use [multiple native nodes](https://docs.docker.com/build/building/multi-platform/#multiple-native-nodes) or [cross-compilation](https://docs.docker.com/build/building/multi-platform/#cross-compilation) instead, if possible.
>
> ​	如果可能，请使用[多个本地节点](https://docs.docker.com/build/building/multi-platform/#multiple-native-nodes)或[交叉编译](https://docs.docker.com/build/building/multi-platform/#cross-compilation)。

Docker Desktop supports running and building multi-platform images under emulation by default. No configuration is necessary as the builder uses the QEMU that's bundled within the Docker Desktop VM.

​	Docker Desktop 默认支持在仿真下运行和构建多平台镜像，无需配置，构建器会使用 Docker Desktop 虚拟机内捆绑的 QEMU。

#### Install QEMU manually

If you're using a builder outside of Docker Desktop, such as if you're using Docker Engine on Linux, or a custom remote builder, you need to install QEMU and register the executable types on the host OS. The prerequisites for installing QEMU are:

​	如果您使用 Docker Desktop 之外的构建器（例如在 Linux 上的 Docker Engine 或自定义远程构建器），需要在主机操作系统上安装 QEMU 并注册可执行文件类型。安装 QEMU 的先决条件包括：

- Linux kernel version 4.8 or later
  - Linux 内核版本 4.8 或更高

- `binfmt-support` version 2.1.7 or later
  - `binfmt-support` 版本 2.1.7 或更高

- The QEMU binaries must be statically compiled and registered with the `fix_binary` flag
  - QEMU 二进制文件必须静态编译并使用 `fix_binary` 标志注册


Use the [`tonistiigi/binfmt`](https://github.com/tonistiigi/binfmt) image to install QEMU and register the executable types on the host with a single command:

​	使用 [`tonistiigi/binfmt`](https://github.com/tonistiigi/binfmt) 镜像可以一键安装 QEMU 并在主机上注册可执行文件类型：

```console
$ docker run --privileged --rm tonistiigi/binfmt --install all
```

This installs the QEMU binaries and registers them with [`binfmt_misc`](https://en.wikipedia.org/wiki/Binfmt_misc), enabling QEMU to execute non-native file formats for emulation.

​	这会安装 QEMU 二进制文件，并将它们注册到 [`binfmt_misc`](https://en.wikipedia.org/wiki/Binfmt_misc)，使 QEMU 能够执行非本机文件格式进行仿真。

Once QEMU is installed and the executable types are registered on the host OS, they work transparently inside containers. You can verify your registration by checking if `F` is among the flags in `/proc/sys/fs/binfmt_misc/qemu-*`.

​	一旦在主机操作系统上安装了 QEMU 并注册了可执行文件类型，它们会在容器内透明地工作。您可以通过检查 `/proc/sys/fs/binfmt_misc/qemu-*` 中的标志是否包含 `F` 来验证注册是否成功。

### 多个本地节点 Multiple native nodes

Using multiple native nodes provide better support for more complicated cases that QEMU can't handle, and also provides better performance.

​	使用多个本地节点可以更好地支持 QEMU 无法处理的复杂情况，并提供更好的性能。

You can add additional nodes to a builder using the `--append` flag.

​	您可以使用 `--append` 标志向构建器添加额外的节点。

The following command creates a multi-node builder from Docker contexts named `node-amd64` and `node-arm64`. This example assumes that you've already added those contexts.

​	以下命令从名为 `node-amd64` 和 `node-arm64` 的 Docker 上下文创建一个多节点构建器。此示例假设您已经添加了这些上下文。

```console
$ docker buildx create --use --name mybuild node-amd64
mybuild
$ docker buildx create --append --name mybuild node-arm64
$ docker buildx build --platform linux/amd64,linux/arm64 .
```

While this approach has advantages over emulation, managing multi-node builders introduces some overhead of setting up and managing builder clusters. Alternatively, you can use Docker Build Cloud, a service that provides managed multi-node builders on Docker's infrastructure. With Docker Build Cloud, you get native multi-platform ARM and X86 builders without the burden of maintaining them. Using cloud builders also provides additional benefits, such as a shared build cache.

​	尽管这种方法比仿真有优势，但管理多节点构建器会带来设置和管理集群的开销。或者，您可以使用 Docker Build Cloud，这是一个在 Docker 基础设施上提供托管多节点构建器的服务。使用 Docker Build Cloud，您可以获得原生多平台 ARM 和 X86 构建器，而无需维护它们。此外，云构建器还提供共享构建缓存等额外好处。

After signing up for Docker Build Cloud, add the builder to your local environment and start building.

​	注册 Docker Build Cloud 后，将构建器添加到本地环境并开始构建。

```console
$ docker buildx create --driver cloud ORG/BUILDER_NAME
cloud-ORG-BUILDER_NAME
$ docker build \
  --builder cloud-ORG-BUILDER_NAME \
  --platform linux/amd64,linux/arm64,linux/arm/v7 \
  --tag IMAGE_NAME \
  --push .
```

For more information, see [Docker Build Cloud]({{< ref "/manuals/DockerBuildCloud" >}}).

​	更多信息请参阅 [Docker Build Cloud]({{< ref "/manuals/DockerBuildCloud" >}})。

### 交叉编译 Cross-compilation

Depending on your project, if the programming language you use has good support for cross-compilation, you can leverage multi-stage builds to build binaries for target platforms from the native architecture of the builder. Special build arguments, such as `BUILDPLATFORM` and `TARGETPLATFORM`, are automatically available for use in your Dockerfile.

​	根据您的项目，如果您使用的编程语言支持良好的交叉编译，可以利用多阶段构建为目标平台构建二进制文件。特殊的构建参数，如 `BUILDPLATFORM` 和 `TARGETPLATFORM`，会自动用于 Dockerfile 中。

In the following example, the `FROM` instruction is pinned to the native platform of the builder (using the `--platform=$BUILDPLATFORM` option) to prevent emulation from kicking in. Then the pre-defined `$BUILDPLATFORM` and `$TARGETPLATFORM` build arguments are interpolated in a `RUN` instruction. In this case, the values are just printed to stdout with `echo`, but this illustrates how you would pass them to the compiler for cross-compilation.

​	在以下示例中，`FROM` 指令固定为构建器的本地平台（使用 `--platform=$BUILDPLATFORM` 选项）以防止仿真启动。然后在 `RUN` 指令中插入预定义的 `$BUILDPLATFORM` 和 `$TARGETPLATFORM` 构建参数。在此示例中，值被打印到 stdout 上，但它说明了如何将这些值传递给编译器以实现交叉编译。



```dockerfile
# syntax=docker/dockerfile:1
FROM --platform=$BUILDPLATFORM golang:alpine AS build
ARG TARGETPLATFORM
ARG BUILDPLATFORM
RUN echo "I am running on $BUILDPLATFORM, building for $TARGETPLATFORM" > /log
FROM alpine
COPY --from=build /log /log
```

## 示例 Examples

Here are some examples of multi-platform builds:

​	以下是一些多平台构建示例：

- [Simple multi-platform build using emulation](https://docs.docker.com/build/building/multi-platform/#simple-multi-platform-build-using-emulation)
  - 使用仿真的简单多平台构建

- [Multi-platform Neovim build using Docker Build Cloud](https://docs.docker.com/build/building/multi-platform/#multi-platform-neovim-build-using-docker-build-cloud)
  - 使用 Docker Build Cloud 的多平台 Neovim 构建

- [Cross-compiling a Go application](https://docs.docker.com/build/building/multi-platform/#cross-compiling-a-go-application)
  - 跨平台编译 Go 应用程序


### 使用仿真的简单多平台构建 Simple multi-platform build using emulation

This example demonstrates how to build a simple multi-platform image using emulation with QEMU. The image contains a single file that prints the architecture of the container.

​	此示例演示如何使用 QEMU 仿真构建一个简单的多平台镜像。该镜像包含一个单文件，用于打印容器的架构。

Prerequisites:

- Docker Desktop, or Docker Engine with [QEMU installed](https://docs.docker.com/build/building/multi-platform/#install-qemu-manually)
  - Docker Desktop，或带有 [QEMU 已安装](https://docs.docker.com/build/building/multi-platform/#install-qemu-manually)的 Docker Engine

- containerd image store enabled
  - 启用 containerd 镜像存储


Steps:

1. Create an empty directory and navigate to it:

   创建一个空目录并进入：

   ```console
   $ mkdir multi-platform
   $ cd multi-platform
   ```

2. Create a simple Dockerfile that prints the architecture of the container:

   创建一个简单的 Dockerfile，用于打印容器的架构：

   ```dockerfile
   # syntax=docker/dockerfile:1
   FROM alpine
   RUN uname -m > /arch
   ```

3. Build the image for `linux/amd64` and `linux/arm64`:

   构建适用于 `linux/amd64` 和 `linux/arm64` 的镜像：

   ```console
   $ docker build --platform linux/amd64,linux/arm64 -t multi-platform .
   ```

4. Run the image and print the architecture:

   运行镜像并打印架构：

   ```console
   $ docker run --rm multi-platform cat /arch
   ```

   - If you're running on an x86-64 machine, you should see `x86_64`.
     - 如果您运行在 x86-64 机器上，应该看到 `x86_64`。
   - If you're running on an ARM machine, you should see `aarch64`.
     - 如果您运行在 ARM 机器上，应该看到 `aarch64`。

### 使用 Docker Build Cloud 的多平台 Neovim 构建 Multi-platform Neovim build using Docker Build Cloud

This example demonstrates how run a multi-platform build using Docker Build Cloud to compile and export [Neovim](https://github.com/neovim/neovim) binaries for the `linux/amd64` and `linux/arm64` platforms.

​	此示例展示了如何使用 Docker Build Cloud 运行多平台构建，编译并导出 [Neovim](https://github.com/neovim/neovim) 的 `linux/amd64` 和 `linux/arm64` 平台二进制文件。

Docker Build Cloud provides managed multi-node builders that support native multi-platform builds without the need for emulation, making it much faster to do CPU-intensive tasks like compilation.

​	Docker Build Cloud 提供托管的多节点构建器，无需仿真即可支持原生多平台构建，使编译等 CPU 密集型任务速度更快。

Prerequisites:

- You've [signed up for Docker Build Cloud and created a builder]({{< ref "/manuals/DockerBuildCloud/Setup" >}})
  - 您已经[注册了 Docker Build Cloud 并创建了构建器]({{< ref "/manuals/DockerBuildCloud/Setup" >}})


Steps:

1. Create an empty directory and navigate to it:

   创建一个空目录并进入：

   ```console
   $ mkdir docker-build-neovim
   $ cd docker-build-neovim
   ```

2. Create a Dockerfile that builds Neovim.

   创建一个构建 Neovim 的 Dockerfile。

   ```dockerfile
   # syntax=docker/dockerfile:1
   FROM debian:bookworm AS build
   WORKDIR /work
   RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
       --mount=type=cache,target=/var/lib/apt,sharing=locked \
       apt-get update && apt-get install -y \
       build-essential \
       cmake \
       curl \
       gettext \
       ninja-build \
       unzip
   ADD https://github.com/neovim/neovim.git#stable .
   RUN make CMAKE_BUILD_TYPE=RelWithDebInfo
   
   FROM scratch
   COPY --from=build /work/build/bin/nvim /
   ```

3. Build the image for `linux/amd64` and `linux/arm64` using Docker Build Cloud:

   使用 Docker Build Cloud 构建 `linux/amd64` 和 `linux/arm64` 的镜像：

   ```console
   $ docker build \
      --builder <cloud-builder> \
      --platform linux/amd64,linux/arm64 \
      --output ./bin
   ```

   This command builds the image using the cloud builder and exports the binaries to the `bin` directory.

   ​	该命令使用云构建器构建镜像并将二进制文件导出到 `bin` 目录。

4. Verify that the binaries are built for both platforms. You should see the `nvim` binary for both `linux/amd64` and `linux/arm64`.

   验证二进制文件是否为两个平台构建。您应该看到 `linux/amd64` 和 `linux/arm64` 的 `nvim` 二进制文件。

   ```console
   $ tree ./bin
   ./bin
   ├── linux_amd64
   │   └── nvim
   └── linux_arm64
       └── nvim
   
   3 directories, 2 files
   ```

### 跨平台编译 Go 应用程序 Cross-compiling a Go application

This example demonstrates how to cross-compile a Go application for multiple platforms using multi-stage builds. The application is a simple HTTP server that listens on port 8080 and returns the architecture of the container. This example uses Go, but the same principles apply to other programming languages that support cross-compilation.

​	此示例展示了如何使用多阶段构建跨平台编译 Go 应用程序。该应用程序是一个简单的 HTTP 服务器，监听端口 8080 并返回容器的架构。此示例使用 Go，但同样适用于支持交叉编译的其他编程语言。

Cross-compilation with Docker builds works by leveraging a series of pre-defined (in BuildKit) build arguments that give you information about platforms of the builder and the build targets. You can use these pre-defined arguments to pass the platform information to the compiler.

​	Docker 构建的交叉编译通过一系列预定义的构建参数（在 BuildKit 中）实现，这些参数提供了构建器和构建目标的平台信息。您可以使用这些预定义的参数将平台信息传递给编译器。

In Go, you can use the `GOOS` and `GOARCH` environment variables to specify the target platform to build for.

​	在 Go 中，您可以使用 `GOOS` 和 `GOARCH` 环境变量指定要构建的目标平台。

Prerequisites:

- Docker Desktop or Docker Engine

Steps:

1. Create an empty directory and navigate to it:

   创建一个空目录并进入：

   ```console
   $ mkdir go-server
   $ cd go-server
   ```

2. Create a base Dockerfile that builds the Go application:

   创建一个用于构建 Go 应用程序的基础 Dockerfile：

   ```dockerfile
   # syntax=docker/dockerfile:1
   FROM golang:alpine AS build
   WORKDIR /app
   ADD https://github.com/dvdksn/buildme.git#eb6279e0ad8a10003718656c6867539bd9426ad8 .
   RUN go build -o server .
   
   FROM alpine
   COPY --from=build /app/server /server
   ENTRYPOINT ["/server"]
   ```

   This Dockerfile can't build multi-platform with cross-compilation yet. If you were to try to build this Dockerfile with `docker build`, the builder would attempt to use emulation to build the image for the specified platforms.

   ​	该 Dockerfile 尚未支持跨平台构建。如果您尝试使用 `docker build` 构建此 Dockerfile，构建器将尝试使用仿真来构建指定平台的镜像。

3. To add cross-compilation support, update the Dockerfile to use the pre-defined `BUILDPLATFORM` and `TARGETPLATFORM` build arguments. These arguments are automatically available in the Dockerfile when you use the `--platform` flag with `docker build`. 为了添加交叉编译支持，更新 Dockerfile，使其使用预定义的 `BUILDPLATFORM` 和 `TARGETPLATFORM` 构建参数。这些参数在使用 `--platform` 标志时会自动可用。

   - Pin the `golang` image to the platform of the builder using the `--platform=$BUILDPLATFORM` option.
     - 使用 `--platform=$BUILDPLATFORM` 选项将 `golang` 镜像固定到构建器的平台。

   - Add `ARG` instructions for the Go compilation stages to make the `TARGETOS` and `TARGETARCH` build arguments available to the commands in this stage.
     - 为 Go 编译阶段添加 `ARG` 指令，使 `TARGETOS` 和 `TARGETARCH` 构建参数可用于该阶段的命令。

   - Set the `GOOS` and `GOARCH` environment variables to the values of `TARGETOS` and `TARGETARCH`. The Go compiler uses these variables to do cross-compilation.
     - 将 `GOOS` 和 `GOARCH` 环境变量设置为 `TARGETOS` 和 `TARGETARCH` 的值。Go 编译器使用这些变量来进行交叉编译。


   

   {{< tabpane text=true persist=disabled >}}

   {{% tab header="Updated Dockerfile" %}}

   ```dockerfile
   # syntax=docker/dockerfile:1
   FROM --platform=$BUILDPLATFORM golang:alpine AS build
   ARG TARGETOS
   ARG TARGETARCH
   WORKDIR /app
   ADD https://github.com/dvdksn/buildme.git#eb6279e0ad8a10003718656c6867539bd9426ad8 .
   RUN GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -o server .
   
   FROM alpine
   COPY --from=build /app/server /server
   ENTRYPOINT ["/server"]
   ```

   {{% /tab  %}}

   {{% tab header="Old Dockerfile" %}}

   ```dockerfile
   # syntax=docker/dockerfile:1
   FROM golang:alpine AS build
   WORKDIR /app
   ADD https://github.com/dvdksn/buildme.git#eb6279e0ad8a10003718656c6867539bd9426ad8 .
   RUN go build -o server .
   
   FROM alpine
   COPY --from=build /app/server /server
   ENTRYPOINT ["/server"]
   ```

   {{% /tab  %}}

   {{% tab header="Diff" %}}

   ```dockerfile
   # syntax=docker/dockerfile:1
   -FROM golang:alpine AS build
   +FROM --platform=$BUILDPLATFORM golang:alpine AS build
   +ARG TARGETOS
   +ARG TARGETARCH
   WORKDIR /app
   ADD https://github.com/dvdksn/buildme.git#eb6279e0ad8a10003718656c6867539bd9426ad8 .
   -RUN go build -o server .
   RUN GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -o server .
   
   FROM alpine
   COPY --from=build /app/server /server
   ENTRYPOINT ["/server"]
   ```

   {{% /tab  %}}

   {{< /tabpane >}}

   

   ------

4. Build the image for `linux/amd64` and `linux/arm64`:

   为 `linux/amd64` 和 `linux/arm64` 构建镜像：

   ```console
   $ docker build --platform linux/amd64,linux/arm64 -t go-server .
   ```

This example has shown how to cross-compile a Go application for multiple platforms with Docker builds. The specific steps on how to do cross-compilation may vary depending on the programming language you're using. Consult the documentation for your programming language to learn more about cross-compiling for different platforms.

​	此示例展示了如何使用 Docker 构建多平台编译 Go 应用程序。具体的交叉编译步骤可能因使用的编程语言不同而异。请查阅您编程语言的文档以了解更多关于不同平台交叉编译的信息。

> **Tip**
>
> You may also want to consider checking out [xx - Dockerfile cross-compilation helpers](https://github.com/tonistiigi/xx). `xx` is a Docker image containing utility scripts that make cross-compiling with Docker builds easier.
>
> ​	您还可以考虑查看 [xx - Dockerfile 交叉编译助手](https://github.com/tonistiigi/xx)。`xx` 是一个 Docker 镜像，包含了一些实用脚本，使 Docker 构建的交叉编译更为简单。
