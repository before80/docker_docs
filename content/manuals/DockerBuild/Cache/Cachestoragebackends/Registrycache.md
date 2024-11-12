+++
title = "注册表缓存"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/backends/registry/](https://docs.docker.com/build/cache/backends/registry/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Registry cache - 注册表缓存

The `registry` cache storage can be thought of as an extension to the `inline` cache. Unlike the `inline` cache, the `registry` cache is entirely separate from the image, which allows for more flexible usage - `registry`-backed cache can do everything that the inline cache can do, and more:

​	`registry` 缓存存储可以视为 `inline` 缓存的扩展。与 `inline` 缓存不同，`registry` 缓存完全独立于镜像，从而提供了更多的灵活性——基于 `registry` 的缓存不仅可以完成 `inline` 缓存的所有功能，还能实现更多：

- Allows for separating the cache and resulting image artifacts so that you can distribute your final image without the cache inside.
  - 允许分离缓存和生成的镜像工件，因此可以分发不带缓存的最终镜像。

- It can efficiently cache multi-stage builds in `max` mode, instead of only the final stage.
  - 可以在 `max` 模式下高效缓存多阶段构建，而不仅仅是最终阶段。

- It works with other exporters for more flexibility, instead of only the `image` exporter.
  - 可以与其他导出器一起使用，提供更多的灵活性，而不仅仅是 `image` 导出器。


This cache storage backend is not supported with the default `docker` driver. To use this feature, create a new builder using a different driver. See [Build drivers]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}) for more information.

​	默认的 `docker` 驱动不支持此缓存存储后端。要使用此功能，请使用不同的驱动创建一个新的构建器。更多信息参见 构建驱动。

## 概要 Synopsis

Unlike the simpler `inline` cache, the `registry` cache supports several configuration parameters:

​	与较为简单的 `inline` 缓存不同，`registry` 缓存支持多个配置参数：



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=registry,ref=<registry>/<cache-image>[,parameters...] \
  --cache-from type=registry,ref=<registry>/<cache-image> .
```

The following table describes the available CSV parameters that you can pass to `--cache-to` and `--cache-from`.

​	下表描述了可用于 `--cache-to` 和 `--cache-from` 的 CSV 参数：

| Name                | Option                  | Type                    | Default | Description                                                  |
| ------------------- | ----------------------- | ----------------------- | ------- | ------------------------------------------------------------ |
| `ref`               | `cache-to`,`cache-from` | String                  |         | 要导入的缓存镜像的全名。 Full name of the cache image to import. |
| `mode`              | `cache-to`              | `min`,`max`             | `min`   | 要导出的缓存层，参见 [缓存模式](https://docs.docker.com/build/cache/backends/#cache-mode)。 Cache layers to export, see [cache mode](https://docs.docker.com/build/cache/backends/#cache-mode). |
| `oci-mediatypes`    | `cache-to`              | `true`,`false`          | `true`  | 在导出清单中使用 OCI 媒体类型，参见 [OCI 媒体类型](https://docs.docker.com/build/cache/backends/#oci-media-types)。Use OCI media types in exported manifests, see [OCI media types](https://docs.docker.com/build/cache/backends/#oci-media-types). |
| `image-manifest`    | `cache-to`              | `true`,`false`          | `false` | 使用 OCI 媒体类型时，为缓存生成镜像清单而不是镜像索引，参见 [OCI 媒体类型](https://docs.docker.com/build/cache/backends/#oci-media-types)。 When using OCI media types, generate an image manifest instead of an image index for the cache image, see [OCI media types](https://docs.docker.com/build/cache/backends/#oci-media-types). |
| `compression`       | `cache-to`              | `gzip`,`estargz`,`zstd` | `gzip`  | 压缩类型，参见 [缓存压缩](https://docs.docker.com/build/cache/backends/#cache-compression)。 Compression type, see [cache compression](https://docs.docker.com/build/cache/backends/#cache-compression). |
| `compression-level` | `cache-to`              | `0..22`                 |         | 压缩级别，参见 [缓存压缩](https://docs.docker.com/build/cache/backends/#cache-compression)。 Compression level, see [cache compression](https://docs.docker.com/build/cache/backends/#cache-compression). |
| `force-compression` | `cache-to`              | `true`,`false`          | `false` | 强制压缩，参见 [缓存压缩](https://docs.docker.com/build/cache/backends/#cache-compression)。Forcibly apply compression, see [cache compression](https://docs.docker.com/build/cache/backends/#cache-compression). |
| `ignore-error`      | `cache-to`              | Boolean                 | `false` | 忽略缓存导出失败导致的错误。Ignore errors caused by failed cache exports. |

You can choose any valid value for `ref`, as long as it's not the same as the target location that you push your image to. You might choose different tags (e.g. `foo/bar:latest` and `foo/bar:build-cache`), separate image names (e.g. `foo/bar` and `foo/bar-cache`), or even different repositories (e.g. `docker.io/foo/bar` and `ghcr.io/foo/bar`). It's up to you to decide the strategy that you want to use for separating your image from your cache images.

​	您可以为 `ref` 选择任意有效值，只要它与推送镜像的目标位置不同即可。您可以选择不同的标签（例如 `foo/bar:latest` 和 `foo/bar:build-cache`）、单独的镜像名称（例如 `foo/bar` 和 `foo/bar-cache`），甚至不同的仓库（例如 `docker.io/foo/bar` 和 `ghcr.io/foo/bar`）。您可以自行决定用于分离镜像与缓存镜像的策略。

If the `--cache-from` target doesn't exist, then the cache import step will fail, but the build continues.

​	如果 `--cache-from` 目标不存在，则缓存导入步骤将失败，但构建会继续进行。

## Further reading

For an introduction to caching see [Docker build cache]({{< ref "/manuals/DockerBuild/Cache" >}}).

​	有关缓存的介绍，请参见 Docker 构建缓存。

For more information on the `registry` cache backend, see the [BuildKit README](https://github.com/moby/buildkit#registry-push-image-and-cache-separately).

​	有关 `registry` 缓存后端的更多信息，请参阅 [BuildKit README](https://github.com/moby/buildkit#registry-push-image-and-cache-separately)。
