+++
title = "内联缓存"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/backends/inline/](https://docs.docker.com/build/cache/backends/inline/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Inline cache - 内联缓存

The `inline` cache storage backend is the simplest way to get an external cache and is easy to get started using if you're already building and pushing an image.

​	`inline` 缓存存储后端是一种获取外部缓存的最简单方法，如果您已经在构建并推送镜像，则非常适合开始使用。

The downside of inline cache is that it doesn't scale with multi-stage builds as well as the other drivers do. It also doesn't offer separation between your output artifacts and your cache output. This means that if you're using a particularly complex build flow, or not exporting your images directly to a registry, then you may want to consider the [registry]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends/Registrycache" >}}) cache.

​	内联缓存的缺点是它在多阶段构建中的扩展性不如其他驱动，也不提供输出工件和缓存输出的分离。这意味着，如果您使用特别复杂的构建流程，或未将镜像直接导出到注册表，则可能需要考虑使用 注册表缓存。

## 概要 Synopsis



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=inline \
  --cache-from type=registry,ref=<registry>/<image> .
```

No additional parameters are supported for the `inline` cache.

​	`inline` 缓存不支持其他参数。

To export cache using `inline` storage, pass `type=inline` to the `--cache-to` option:

​	要使用 `inline` 存储导出缓存，请将 `type=inline` 传递给 `--cache-to` 选项：



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=inline .
```

Alternatively, you can also export inline cache by setting the build argument `BUILDKIT_INLINE_CACHE=1`, instead of using the `--cache-to` flag:

​	或者，您也可以通过设置构建参数 `BUILDKIT_INLINE_CACHE=1` 来导出内联缓存，而无需使用 `--cache-to` 标志：



```console
$ docker buildx build --push -t <registry>/<image> \
  --build-arg BUILDKIT_INLINE_CACHE=1 .
```

To import the resulting cache on a future build, pass `type=registry` to `--cache-from` which lets you extract the cache from inside a Docker image in the specified registry:

​	要在未来构建中导入生成的缓存，请将 `type=registry` 传递给 `--cache-from`，允许您从指定注册表中的 Docker 镜像中提取缓存：



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-from type=registry,ref=<registry>/<image> .
```

## Further reading

For an introduction to caching see [Docker build cache]({{< ref "/manuals/DockerBuild/Cache" >}}).

​	有关缓存的介绍，请参见 Docker 构建缓存。

For more information on the `inline` cache backend, see the [BuildKit README](https://github.com/moby/buildkit#inline-push-image-and-cache-together).

​	有关 `inline` 缓存后端的更多信息，请参阅 [BuildKit README](https://github.com/moby/buildkit#inline-push-image-and-cache-together)。
