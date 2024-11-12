+++
title = "导出器"
date = 2024-10-23T14:54:40+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/exporters/](https://docs.docker.com/build/exporters/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Exporters overview- 导出器概述

Exporters save your build results to a specified output type. You specify the exporter to use with the [`--output` CLI option](https://docs.docker.com/reference/cli/docker/buildx/build/#output). Buildx supports the following exporters:

​	导出器会将构建结果保存为指定的输出类型。您可以使用 [`--output` CLI 选项](https://docs.docker.com/reference/cli/docker/buildx/build/#output) 指定要使用的导出器。Buildx 支持以下导出器：

- `image`: exports the build result to a container image.
  - `image`：将构建结果导出为容器镜像。

- `registry`: exports the build result into a container image, and pushes it to the specified registry.

  - `registry`：将构建结果导出为容器镜像，并推送到指定的注册表。

- `local`: exports the build root filesystem into a local directory.

  - `local`：将构建的根文件系统导出到本地目录。

- `tar`: packs the build root filesystem into a local tarball.

  - `tar`：将构建的根文件系统打包到本地 tarball 中。

- `oci`: exports the build result to the local filesystem in the [OCI image layout](https://github.com/opencontainers/image-spec/blob/v1.0.1/image-layout.md) format.

  - `oci`：将构建结果导出到本地文件系统中的 [OCI 镜像布局](https://github.com/opencontainers/image-spec/blob/v1.0.1/image-layout.md) 格式。

- `docker`: exports the build result to the local filesystem in the [Docker Image Specification v1.2.0](https://github.com/moby/moby/blob/v25.0.0/image/spec/v1.2.md) format.

  - `docker`：将构建结果导出到本地文件系统中的 [Docker 镜像规范 v1.2.0](https://github.com/moby/moby/blob/v25.0.0/image/spec/v1.2.md) 格式。

- `cacheonly`: doesn't export a build output, but runs the build and creates a cache.

  - `cacheonly`：不导出构建输出，但运行构建并创建缓存。

  

## 使用导出器 Using exporters

To specify an exporter, use the following command syntax:

​	要指定导出器，请使用以下命令语法：

```console
$ docker buildx build --tag <registry>/<image> \
  --output type=TYPE .
```

Most common use cases don't require that you specify which exporter to use explicitly. You only need to specify the exporter if you intend to customize the output, or if you want to save it to disk. The `--load` and `--push` options allow Buildx to infer the exporter settings to use.

​	大多数常见用例不需要显式指定导出器。您只需要在想自定义输出或保存到磁盘时指定导出器。`--load` 和 `--push` 选项允许 Buildx 推断要使用的导出器设置。

For example, if you use the `--push` option in combination with `--tag`, Buildx automatically uses the `image` exporter, and configures the exporter to push the results to the specified registry.

​	例如，如果您在 `--tag` 选项中使用了 `--push` 选项，Buildx 会自动使用 `image` 导出器，并将结果推送到指定的注册表。

To get the full flexibility out of the various exporters BuildKit has to offer, you use the `--output` flag that lets you configure exporter options.

​	要充分利用 BuildKit 提供的各种导出器，您可以使用 `--output` 标志来配置导出器选项。

## 使用场景 Use cases

Each exporter type is designed for different use cases. The following sections describe some common scenarios, and how you can use exporters to generate the output that you need.

​	每种导出器类型都适用于不同的场景。以下部分描述了一些常见的场景，以及如何使用导出器生成所需的输出。

### 加载到镜像存储 Load to image store

Buildx is often used to build container images that can be loaded to an image store. That's where the `docker` exporter comes in. The following example shows how to build an image using the `docker` exporter, and have that image loaded to the local image store, using the `--output` option:

​	Buildx 经常用于构建可以加载到镜像存储中的容器镜像，这时可以使用 `docker` 导出器。以下示例展示了如何使用 `docker` 导出器构建镜像，并使用 `--output` 选项将该镜像加载到本地镜像存储中：



```console
$ docker buildx build \
  --output type=docker,name=<registry>/<image> .
```

Buildx CLI will automatically use the `docker` exporter and load it to the image store if you supply the `--tag` and `--load` options:

​	如果提供了 `--tag` 和 `--load` 选项，Buildx CLI 将自动使用 `docker` 导出器并将其加载到镜像存储中：

```console
$ docker buildx build --tag <registry>/<image> --load .
```

Building images using the `docker` driver are automatically loaded to the local image store.

​	使用 `docker` 驱动构建的镜像会自动加载到本地镜像存储中。

Images loaded to the image store are available to `docker run` immediately after the build finishes, and you'll see them in the list of images when you run the `docker images` command.

​	加载到镜像存储的镜像在构建完成后即可用于 `docker run`，并会出现在 `docker images` 命令的镜像列表中。

### 推送到注册表 Push to registry

To push a built image to a container registry, you can use the `registry` or `image` exporters.

​	要将构建的镜像推送到容器注册表，可以使用 `registry` 或 `image` 导出器。

When you pass the `--push` option to the Buildx CLI, you instruct BuildKit to push the built image to the specified registry:

​	当您将 `--push` 选项传递给 Buildx CLI 时，指示 BuildKit 将构建的镜像推送到指定的注册表：

```console
$ docker buildx build --tag <registry>/<image> --push .
```

Under the hood, this uses the `image` exporter, and sets the `push` parameter. It's the same as using the following long-form command using the `--output` option:

​	在底层，这使用的是 `image` 导出器，并设置了 `push` 参数。这等同于使用 `--output` 选项的以下长格式命令：



```console
$ docker buildx build \
  --output type=image,name=<registry>/<image>,push=true .
```

You can also use the `registry` exporter, which does the same thing:

​	您也可以使用 `registry` 导出器，它执行相同的操作：



```console
$ docker buildx build \
  --output type=registry,name=<registry>/<image> .
```

### 导出镜像布局到文件 Export image layout to file

You can use either the `oci` or `docker` exporters to save the build results to image layout on your local filesystem. Both of these exporters generate a tar archive file containing the corresponding image layout. The `dest` parameter defines the target output path for the tarball.

​	您可以使用 `oci` 或 `docker` 导出器将构建结果保存到本地文件系统中的镜像布局。两个导出器都生成包含相应镜像布局的 tar 存档文件。`dest` 参数定义了 tarball 的目标输出路径。



```console
$ docker buildx build --output type=oci,dest=./image.tar .
[+] Building 0.8s (7/7) FINISHED
 ...
 => exporting to oci image format                                                                     0.0s
 => exporting layers                                                                                  0.0s
 => exporting manifest sha256:c1ef01a0a0ef94a7064d5cbce408075730410060e253ff8525d1e5f7e27bc900        0.0s
 => exporting config sha256:eadab326c1866dd247efb52cb715ba742bd0f05b6a205439f107cf91b3abc853          0.0s
 => sending tarball                                                                                   0.0s
$ mkdir -p out && tar -C out -xf ./image.tar
$ tree out
out
├── blobs
│   └── sha256
│       ├── 9b18e9b68314027565b90ff6189d65942c0f7986da80df008b8431276885218e
│       ├── c78795f3c329dbbbfb14d0d32288dea25c3cd12f31bd0213be694332a70c7f13
│       ├── d1cf38078fa218d15715e2afcf71588ee482352d697532cf316626164699a0e2
│       ├── e84fa1df52d2abdfac52165755d5d1c7621d74eda8e12881f6b0d38a36e01775
│       └── fe9e23793a27fe30374308988283d40047628c73f91f577432a0d05ab0160de7
├── index.json
├── manifest.json
└── oci-layout
```

### 导出文件系统 Export filesystem

If you don't want to build an image from your build results, but instead export the filesystem that was built, you can use the `local` and `tar` exporters.

​	如果不想从构建结果中生成镜像，而是导出构建的文件系统，可以使用 `local` 和 `tar` 导出器。

The `local` exporter unpacks the filesystem into a directory structure in the specified location. The `tar` exporter creates a tarball archive file.

​	`local` 导出器会将文件系统解压缩到指定位置的目录结构中。`tar` 导出器则会创建一个 tarball 存档文件。

```

```



```console
$ docker buildx build --output type=tar,dest=<path/to/output> .
```

The `local` exporter is useful in [multi-stage builds]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}}) since it allows you to export only a minimal number of build artifacts, such as self-contained binaries.

​	`local` 导出器在 [多阶段构建]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}}) 中非常有用，因为它允许仅导出少量构建工件，如自包含的二进制文件。

### 仅缓存导出 Cache-only export

The `cacheonly` exporter can be used if you just want to run a build, without exporting any output. This can be useful if, for example, you want to run a test build. Or, if you want to run the build first, and create exports using subsequent commands. The `cacheonly` exporter creates a build cache, so any successive builds are instant.

​	如果您只想运行构建而不导出任何输出，可以使用 `cacheonly` 导出器。这在您想要运行测试构建时非常有用，或者在构建之前运行构建并使用后续命令创建导出时使用。`cacheonly` 导出器会创建一个构建缓存，因此任何后续构建都是即时完成的。



```console
$ docker buildx build --output type=cacheonly
```

If you don't specify an exporter, and you don't provide short-hand options like `--load` that automatically selects the appropriate exporter, Buildx defaults to using the `cacheonly` exporter. Except if you build using the `docker` driver, in which case you use the `docker` exporter.

​	如果您未指定导出器，并且未提供像 `--load` 这样的自动选择适当导出器的简写选项，Buildx 将默认使用 `cacheonly` 导出器。除非您使用 `docker` 驱动构建，在这种情况下将使用 `docker` 导出器。

Buildx logs a warning message when using `cacheonly` as a default:

​	当默认使用 `cacheonly` 时，Buildx 会记录警告信息：



```console
$ docker buildx build .
WARNING: No output specified with docker-container driver.
         Build result will only remain in the build cache.
         To push result image into registry use --push or
         to load image into docker use --load
```

## 多重导出器 Multiple exporters

Introduced in Buildx version 0.13.0

​	引入于 Buildx 版本 0.13.0

You can use multiple exporters for any given build by specifying the `--output` flag multiple times. This requires **both Buildx and BuildKit** version 0.13.0 or later.

​	通过多次指定 `--output` 标志，您可以在任何给定的构建中使用多个导出器。这需要 **Buildx 和 BuildKit** 版本为 0.13.0 或更高版本。

The following example runs a single build, using three different exporters:

​	以下示例运行单个构建，使用三种不同的导出器：

- The `registry` exporter to push the image to a registry
  - 使用 `registry` 导出器将镜像推送到注册表

- The `local` exporter to extract the build results to the local filesystem
  - 使用 `local` 导出器将构建结果提取到本地文件系统

- The `--load` flag (a shorthand for the `image` exporter) to load the results to the local image store.
  - 使用 `--load` 标志（`image` 导出器的简写）将结果加载到本地镜像存储。




```console
$ docker buildx build \
  --output type=registry,tag=<registry>/<image> \
  --output type=local,dest=<path/to/output> \
  --load .
```

## 配置选项 Configuration options

This section describes some configuration options available for exporters.

​	此部分描述了导出器的一些配置选项。

The options described here are common for at least two or more exporter types. Additionally, the different exporters types support specific parameters as well. See the detailed page about each exporter for more information about which configuration parameters apply.

​	这里描述的选项适用于至少两种或多种导出器类型。此外，不同的导出器类型还支持特定的参数。有关适用的配置参数的详细信息，请参阅每个导出器的详细页面。

The common parameters described here are:

​	这里描述的通用参数包括：

- [Compression](https://docs.docker.com/build/exporters/#compression)
- [OCI media type](https://docs.docker.com/build/exporters/#oci-media-types)

### 压缩 Compression

When you export a compressed output, you can configure the exact compression algorithm and level to use. While the default values provide a good out-of-the-box experience, you may wish to tweak the parameters to optimize for storage vs compute costs. Changing the compression parameters can reduce storage space required, and improve image download times, but will increase build times.

​	导出压缩输出时，可以配置使用的确切压缩算法和级别。虽然默认值提供了良好的开箱体验，但您可能希望调整参数以优化存储与计算成本的权衡。更改压缩参数可以减少所需的存储空间，并提高镜像下载时间，但会增加构建时间。

To select the compression algorithm, you can use the `compression` option. For example, to build an `image` with `compression=zstd`:

​	要选择压缩算法，可以使用 `compression` 选项。例如，要构建具有 `compression=zstd` 的 `image`：

```console
$ docker buildx build \
  --output type=image,name=<registry>/<image>,push=true,compression=zstd .
```

Use the `compression-level=<value>` option alongside the `compression` parameter to choose a compression level for the algorithms which support it:

​	使用 `compression-level=<value>` 选项与 `compression` 参数一起选择支持算法的压缩级别：

- 0-9 for `gzip` and `estargz`
- 0-22 for `zstd`

As a general rule, the higher the number, the smaller the resulting file will be, and the longer the compression will take to run.

​	通常，数字越高，生成的文件越小，压缩运行时间越长。

Use the `force-compression=true` option to force re-compressing layers imported from a previous image, if the requested compression algorithm is different from the previous compression algorithm.

​	使用 `force-compression=true` 选项在请求的压缩算法与之前的压缩算法不同时强制重新压缩从先前镜像导入的层。

> **Note**
>
> 
>
> The `gzip` and `estargz` compression methods use the [`compress/gzip` package](https://pkg.go.dev/compress/gzip), while `zstd` uses the [`github.com/klauspost/compress/zstd` package](https://github.com/klauspost/compress/tree/master/zstd).
>
> ​	`gzip` 和 `estargz` 压缩方法使用 [`compress/gzip` 包](https://pkg.go.dev/compress/gzip)，`zstd` 使用 [`github.com/klauspost/compress/zstd` 包](https://github.com/klauspost/compress/tree/master/zstd)。

### OCI 媒体类型 OCI media types

The `image`, `registry`, `oci` and `docker` exporters create container images. These exporters support both Docker media types (default) and OCI media types

​	`image`、`registry`、`oci` 和 `docker` 导出器创建容器镜像。这些导出器支持 Docker 媒体类型（默认）和 OCI 媒体类型。

To export images with OCI media types set, use the `oci-mediatypes` property.

​	要设置导出带有 OCI 媒体类型的镜像，请使用 `oci-mediatypes` 属性。



```console
$ docker buildx build \
  --output type=image,name=<registry>/<image>,push=true,oci-mediatypes=true .
```

## What's next

Read about each of the exporters to learn about how they work and how to use them:

​	阅读有关每个导出器的信息，以了解它们的工作原理及其使用方法：

- [Image and registry exporters]({{< ref "/manuals/DockerBuild/Exporters/Imageandregistryexporters" >}})
  - [镜像和注册表导出器]({{< ref "/manuals/DockerBuild/Exporters/Imageandregistryexporters" >}})

- [OCI and Docker exporters]({{< ref "/manuals/DockerBuild/Exporters/OCIandDockerexporters" >}}).
  - [OCI 和 Docker 导出器]({{< ref "/manuals/DockerBuild/Exporters/OCIandDockerexporters" >}})。
- [Local and tar exporters]({{< ref "/manuals/DockerBuild/Exporters/Localandtarexporters" >}})
  - [本地和 tar 导出器]({{< ref "/manuals/DockerBuild/Exporters/Localandtarexporters" >}})
