+++
title = "缓存存储后端"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/backends/](https://docs.docker.com/build/cache/backends/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Cache storage backends - 缓存存储后端

To ensure fast builds, BuildKit automatically caches the build result in its own internal cache. Additionally, BuildKit also supports exporting build cache to an external location, making it possible to import in future builds.

​	为确保构建速度，BuildKit 会自动将构建结果缓存到其内部缓存中。此外，BuildKit 还支持将构建缓存导出到外部位置，从而可以在未来的构建中导入缓存。

An external cache becomes almost essential in CI/CD build environments. Such environments usually have little-to-no persistence between runs, but it's still important to keep the runtime of image builds as low as possible.

​		在 CI/CD 构建环境中，外部缓存几乎是必需的。这类环境通常在运行之间几乎没有持久性，但保持镜像构建的运行时间尽可能短仍然很重要。

The default `docker` driver supports the `inline`, `local`, `registry`, and `gha` cache backends, but only if you have enabled the [containerd image store]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}}). Other cache backends require you to select a different [driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}).

​	默认的 `docker` 驱动支持 `inline`、`local`、`registry` 和 `gha` 缓存后端，但仅在启用 containerd 图像存储 后才可使用。其他缓存后端需要选择不同的驱动。

> **Warning**
>
> 
>
> If you use secrets or credentials inside your build process, ensure you manipulate them using the dedicated [`--secret` option](https://docs.docker.com/reference/cli/docker/buildx/build/#secret). Manually managing secrets using `COPY` or `ARG` could result in leaked credentials.
>
> ​	如果在构建过程中使用了秘密或凭据，请确保使用专用的 [`--secret` 选项](https://docs.docker.com/reference/cli/docker/buildx/build/#secret) 来处理它们。手动使用 `COPY` 或 `ARG` 处理秘密可能会导致凭据泄露。

## Backends

Buildx supports the following cache storage backends:

​	Buildx 支持以下缓存存储后端：

- `inline`: embeds the build cache into the image. 将构建缓存嵌入到镜像中。

  The inline cache gets pushed to the same location as the main output result. This only works with the [`image` exporter]({{< ref "/manuals/DockerBuild/Exporters/Imageandregistryexporters" >}}).

  ​	内联缓存会与主要输出结果一起推送到同一位置。此方式仅适用于 `image` 导出器。

- `registry`: embeds the build cache into a separate image, and pushes to a dedicated location separate from the main output. 将构建缓存嵌入到单独的镜像中，并推送到与主要输出分开的专用位置。

- `local`: writes the build cache to a local directory on the filesystem. 将构建缓存写入文件系统上的本地目录。

- `gha`: uploads the build cache to [GitHub Actions cache](https://docs.github.com/en/rest/actions/cache) (beta). 将构建缓存上传到 [GitHub Actions 缓存](https://docs.github.com/en/rest/actions/cache)（测试版）。

- `s3`: uploads the build cache to an [AWS S3 bucket](https://aws.amazon.com/s3/) (unreleased). 将构建缓存上传到 [AWS S3 存储桶](https://aws.amazon.com/s3/)（未发布）。

- `azblob`: uploads the build cache to [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/) (unreleased). 将构建缓存上传到 [Azure Blob 存储](https://azure.microsoft.com/en-us/services/storage/blobs/)（未发布）。

## 命令语法 Command syntax

To use any of the cache backends, you first need to specify it on build with the [`--cache-to` option](https://docs.docker.com/reference/cli/docker/buildx/build/#cache-to) to export the cache to your storage backend of choice. Then, use the [`--cache-from` option](https://docs.docker.com/reference/cli/docker/buildx/build/#cache-from) to import the cache from the storage backend into the current build. Unlike the local BuildKit cache (which is always enabled), all of the cache storage backends must be explicitly exported to, and explicitly imported from.

​	要使用任何缓存后端，首先需要在构建时使用 [`--cache-to` 选项](https://docs.docker.com/reference/cli/docker/buildx/build/#cache-to)将缓存导出到所选的存储后端。然后，使用 [`--cache-from` 选项](https://docs.docker.com/reference/cli/docker/buildx/build/#cache-from)将缓存从存储后端导入到当前构建中。与始终启用的本地 BuildKit 缓存不同，所有缓存存储后端必须明确导出和导入。

Example `buildx` command using the `registry` backend, using import and export cache:

​	使用 `registry` 后端的 `buildx` 命令示例，包含导入和导出缓存：



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=registry,ref=<registry>/<cache-image>[,parameters...] \
  --cache-from type=registry,ref=<registry>/<cache-image>[,parameters...] .
```

> **Warning**
>
> 
>
> As a general rule, each cache writes to some location. No location can be written to twice, without overwriting the previously cached data. If you want to maintain multiple scoped caches (for example, a cache per Git branch), then ensure that you use different locations for exported cache.
>
> ​	通常，每个缓存写入一个位置。不能多次写入同一位置，否则会覆盖之前缓存的数据。如果希望维护多个范围的缓存（例如，每个 Git 分支一个缓存），请确保使用不同的导出缓存位置。

## 多重缓存 Multiple caches

BuildKit currently only supports [a single cache exporter](https://github.com/moby/buildkit/pull/3024). But you can import from as many remote caches as you like. For example, a common pattern is to use the cache of both the current branch and the main branch. The following example shows importing cache from multiple locations using the registry cache backend:

​	目前 BuildKit 仅支持[单一缓存导出器](https://github.com/moby/buildkit/pull/3024)。但您可以从多个远程缓存导入。例如，一个常见的模式是同时使用当前分支和主分支的缓存。以下示例展示了使用注册表缓存后端从多个位置导入缓存：

```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=registry,ref=<registry>/<cache-image>:<branch> \
  --cache-from type=registry,ref=<registry>/<cache-image>:<branch> \
  --cache-from type=registry,ref=<registry>/<cache-image>:main .
```

## 配置选项 Configuration options

This section describes some configuration options available when generating cache exports. The options described here are common for at least two or more backend types. Additionally, the different backend types support specific parameters as well. See the detailed page about each backend type for more information about which configuration parameters apply.

​	本节介绍生成缓存导出时的一些配置选项。这里描述的选项适用于至少两个或更多类型的后端。此外，不同的后端类型还支持特定的参数。请参阅有关每种后端类型的详细页面，了解适用的配置参数。

The common parameters described here are:

​	常用参数包括：

- [Cache mode](https://docs.docker.com/build/cache/backends/#cache-mode) 缓存模式
- [Cache compression](https://docs.docker.com/build/cache/backends/#cache-compression) 缓存压缩
- [OCI media type](https://docs.docker.com/build/cache/backends/#oci-media-types)  OCI 媒体类型

### 缓存模式 Cache mode

When generating a cache output, the `--cache-to` argument accepts a `mode` option for defining which layers to include in the exported cache. This is supported by all cache backends except for the `inline` cache.

​	生成缓存输出时，`--cache-to` 参数接受一个 `mode` 选项，用于定义要包含在导出缓存中的层。除 `inline` 缓存外，所有缓存后端均支持此选项。

Mode can be set to either of two options: `mode=min` or `mode=max`. For example, to build the cache with `mode=max` with the registry backend:

​	模式可设置为 `mode=min` 或 `mode=max`。例如，使用 `registry` 后端并将缓存构建为 `mode=max`：



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=registry,ref=<registry>/<cache-image>,mode=max \
  --cache-from type=registry,ref=<registry>/<cache-image> .
```

This option is only set when exporting a cache, using `--cache-to`. When importing a cache (`--cache-from`) the relevant parameters are automatically detected.

​	此选项仅在使用 `--cache-to` 导出缓存时设置。导入缓存（`--cache-from`）时，会自动检测相关参数。

In `min` cache mode (the default), only layers that are exported into the resulting image are cached, while in `max` cache mode, all layers are cached, even those of intermediate steps.

​	在 `min` 缓存模式（默认）下，仅导出结果镜像中包含的层进行缓存，而在 `max` 缓存模式下，所有层（包括中间步骤的层）都会缓存。

While `min` cache is typically smaller (which speeds up import/export times, and reduces storage costs), `max` cache is more likely to get more cache hits. Depending on the complexity and location of your build, you should experiment with both parameters to find the results that work best for you.

​	`min` 缓存通常更小（加快导入/导出时间并降低存储成本），而 `max` 缓存则更可能获得更多的缓存命中。根据构建的复杂性和位置，应尝试使用这两种参数以找到最适合的结果。

### 缓存压缩 Cache compression

The cache compression options are the same as the [exporter compression options](https://docs.docker.com/build/exporters/#compression). This is supported by the `local` and `registry` cache backends.

​	缓存压缩选项与[导出器压缩选项](https://docs.docker.com/build/exporters/#compression)相同。此功能适用于 `local` 和 `registry` 缓存后端。

For example, to compress the `registry` cache with `zstd` compression:

​	例如，使用 `zstd` 压缩 `registry` 缓存：

```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=registry,ref=<registry>/<cache-image>,compression=zstd \
  --cache-from type=registry,ref=<registry>/<cache-image> .
```

### OCI 媒体类型 OCI media types

The cache OCI options are the same as the [exporter OCI options](https://docs.docker.com/build/exporters/#oci-media-types). These are supported by the `local` and `registry` cache backends.

​	缓存 OCI 选项与[导出器 OCI 选项](https://docs.docker.com/build/exporters/#oci-media-types)相同，适用于 `local` 和 `registry` 缓存后端。

For example, to export OCI media type cache, use the `oci-mediatypes` property:

​	例如，要导出 OCI 媒体类型缓存，请使用 `oci-mediatypes` 属性：



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=registry,ref=<registry>/<cache-image>,oci-mediatypes=true \
  --cache-from type=registry,ref=<registry>/<cache-image> .
```

This property is only meaningful with the `--cache-to` flag. When fetching cache, BuildKit will auto-detect the correct media types to use.

​	此属性仅在 `--cache-to` 标志时有意义。获取缓存时，BuildKit 将自动检测应使用的正确媒体类型。

By default, the OCI media type generates an image index for the cache image. Some OCI registries, such as Amazon ECR, don't support the image index media type: `application/vnd.oci.image.index.v1+json`. If you export cache images to ECR, or any other registry that doesn't support image indices, set the `image-manifest` parameter to `true` to generate a single image manifest instead of an image index for the cache image:

​	默认情况下，OCI 媒体类型会为缓存镜像生成镜像索引。一些 OCI 注册表（如 Amazon ECR）不支持镜像索引媒体类型：`application/vnd.oci.image.index.v1+json`。如果要将缓存镜像导出到 ECR 或其他不支持镜像索引的注册表，请将 `image-manifest` 参数设置为 `true`，以生成单个镜像清单，而不是缓存镜像的镜像索引。



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=registry,ref=<registry>/<cache-image>,oci-mediatypes=true,image-manifest=true \
  --cache-from type=registry,ref=<registry>/<cache-image> .
```
