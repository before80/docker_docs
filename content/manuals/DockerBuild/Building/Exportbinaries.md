+++
title = "导出二进制文件"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/building/export/](https://docs.docker.com/build/building/export/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Export binaries - 导出二进制文件

Did you know that you can use Docker to build your application to standalone binaries? Sometimes, you don’t want to package and distribute your application as a Docker image. Use Docker to build your application, and use exporters to save the output to disk.

​	您知道可以使用 Docker 将应用程序构建为独立的二进制文件吗？有时您不想将应用程序打包并作为 Docker 镜像分发，可以使用 Docker 构建应用程序，然后使用导出器将输出保存到磁盘。

The default output format for `docker build` is a container image. That image is automatically loaded to your local image store, where you can run a container from that image, or push it to a registry. Under the hood, this uses the default exporter, called the `docker` exporter.

​	`docker build` 的默认输出格式是容器镜像。该镜像会自动加载到本地镜像存储中，您可以从该镜像运行容器或将其推送到注册表。这背后使用的是默认导出器，称为 `docker` 导出器。

To export your build results as files instead, you can use the `--output` flag, or `-o` for short. the `--output` flag lets you change the output format of your build.

​	如果您想将构建结果导出为文件，可以使用 `--output` 标志（或简写为 `-o`），该标志可以更改构建的输出格式。

## 从构建中导出二进制文件 Export binaries from a build

If you specify a filepath to the `docker build --output` flag, Docker exports the contents of the build container at the end of the build to the specified location on your host's filesystem. This uses the `local` [exporter]({{< ref "/manuals/DockerBuild/Exporters/Localandtarexporters" >}}).

​	如果为 `docker build --output` 标志指定文件路径，Docker 会在构建结束时将构建容器的内容导出到主机文件系统中的指定位置。这使用的是 `local` [导出器]({{< ref "/manuals/DockerBuild/Exporters/Localandtarexporters" >}})。

The neat thing about this is that you can use Docker's powerful isolation and build features to create standalone binaries. This works well for Go, Rust, and other languages that can compile to a single binary.

​	这样做的妙处在于，您可以利用 Docker 强大的隔离和构建功能创建独立的二进制文件。这对 Go、Rust 和其他可以编译为单个二进制文件的语言非常有效。

The following example creates a simple Rust program that prints "Hello, World!", and exports the binary to the host filesystem.

​	以下示例创建一个简单的 Rust 程序，打印“Hello, World!” 并将二进制文件导出到主机文件系统中。

1. Create a new directory for this example, and navigate to it:

   为该示例创建一个新目录并进入该目录：

   ```console
   $ mkdir hello-world-bin
   $ cd hello-world-bin
   ```

2. Create a Dockerfile with the following contents:

   创建一个包含以下内容的 Dockerfile：

   ```Dockerfile
   # syntax=docker/dockerfile:1
   FROM rust:alpine AS build
   WORKDIR /src
   COPY <<EOT hello.rs
   fn main() {
       println!("Hello World!");
   }
   EOT
   RUN rustc -o /bin/hello hello.rs
   
   FROM scratch
   COPY --from=build /bin/hello /
   ENTRYPOINT ["/hello"]
   ```

   > **Tip**
   >
   > The `COPY <<EOT` syntax is a [here-document](https://docs.docker.com/reference/dockerfile/#here-documents). It lets you write multi-line strings in a Dockerfile. Here it's used to create a simple Rust program inline in the Dockerfile.
   >
   > ​	`COPY <<EOT` 语法是一种 [here-document](https://docs.docker.com/reference/dockerfile/#here-documents)。它允许您在 Dockerfile 中编写多行字符串。这里用于在 Dockerfile 中内联创建一个简单的 Rust 程序。

   This Dockerfile uses a multi-stage build to compile the program in the first stage, and then copies the binary to a scratch image in the second. The final image is a minimal image that only contains the binary. This use case for the `scratch` image is common for creating minimal build artifacts for programs that don't require a full operating system to run.

   ​	这个 Dockerfile 使用多阶段构建，在第一阶段编译程序，然后在第二阶段将二进制文件复制到一个 `scratch` 镜像中。最终的镜像是一个只包含二进制文件的最小镜像。这种 `scratch` 镜像的用法在创建无需完整操作系统运行的最小构建工件时非常常见。

3. Build the Dockerfile and export the binary to the current working directory:

   构建 Dockerfile 并将二进制文件导出到当前工作目录：

   ```console
   $ docker build --output=. .
   ```

   This command builds the Dockerfile and exports the binary to the current working directory. The binary is named `hello`, and it's created in the current working directory.
   
   ​	该命令构建 Dockerfile 并将二进制文件导出到当前工作目录中。二进制文件命名为 `hello`，并在当前工作目录中创建。

## 导出多平台构建 Exporting multi-platform builds

You use the `local` exporter to export binaries in combination with [multi-platform builds]({{< ref "/manuals/DockerBuild/Building/Multi-platform" >}}). This lets you compile multiple binaries at once, that can be run on any machine of any architecture, provided that the target platform is supported by the compiler you use.

​	您可以结合 [多平台构建]({{< ref "/manuals/DockerBuild/Building/Multi-platform" >}}) 使用 `local` 导出器导出二进制文件。这允许您一次编译多个二进制文件，只要目标平台由您使用的编译器支持，这些二进制文件就可以在任何架构的机器上运行。

Continuing on the example Dockerfile in the [Export binaries from a build](https://docs.docker.com/build/building/export/#export-binaries-from-a-build) section:

​	继续前面 [从构建中导出二进制文件](https://docs.docker.com/build/building/export/#export-binaries-from-a-build) 部分的示例 Dockerfile：

```dockerfile
# syntax=docker/dockerfile:1
FROM rust:alpine AS build
WORKDIR /src
COPY <<EOT hello.rs
fn main() {
    println!("Hello World!");
}
EOT
RUN rustc -o /bin/hello hello.rs

FROM scratch
COPY --from=build /bin/hello /
ENTRYPOINT ["/hello"]
```

You can build this Rust program for multiple platforms using the `--platform` flag with the `docker build` command. In combination with the `--output` flag, the build exports the binaries for each target to the specified directory.

​	您可以使用 `--platform` 标志和 `docker build` 命令为多个平台构建此 Rust 程序。结合 `--output` 标志，构建会将每个目标平台的二进制文件导出到指定目录。

For example, to build the program for both `linux/amd64` and `linux/arm64`:

​	例如，构建 `linux/amd64` 和 `linux/arm64` 的程序：

```console
$ docker build --platform=linux/amd64,linux/arm64 --output=out .
$ tree out/
out/
├── linux_amd64
│   └── hello
└── linux_arm64
    └── hello

3 directories, 2 files
```

## 其他信息 Additional information

In addition to the `local` exporter, there are other exporters available. To learn more about the available exporters and how to use them, see the [exporters]({{< ref "/manuals/DockerBuild/Exporters" >}}) documentation.

​	除了 `local` 导出器，还有其他可用的导出器。要了解有关可用导出器及其使用方法的更多信息，请参阅 [exporters]({{< ref "/manuals/DockerBuild/Exporters" >}}) 文档。
