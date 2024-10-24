+++
title = "Inline cache"
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

# Inline cache

The `inline` cache storage backend is the simplest way to get an external cache and is easy to get started using if you're already building and pushing an image.

The downside of inline cache is that it doesn't scale with multi-stage builds as well as the other drivers do. It also doesn't offer separation between your output artifacts and your cache output. This means that if you're using a particularly complex build flow, or not exporting your images directly to a registry, then you may want to consider the [registry]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends/Registrycache" >}}) cache.

## Synopsis



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=inline \
  --cache-from type=registry,ref=<registry>/<image> .
```

No additional parameters are supported for the `inline` cache.

To export cache using `inline` storage, pass `type=inline` to the `--cache-to` option:



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=inline .
```

Alternatively, you can also export inline cache by setting the build argument `BUILDKIT_INLINE_CACHE=1`, instead of using the `--cache-to` flag:



```console
$ docker buildx build --push -t <registry>/<image> \
  --build-arg BUILDKIT_INLINE_CACHE=1 .
```

To import the resulting cache on a future build, pass `type=registry` to `--cache-from` which lets you extract the cache from inside a Docker image in the specified registry:



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-from type=registry,ref=<registry>/<image> .
```

## Further reading

For an introduction to caching see [Docker build cache]({{< ref "/manuals/DockerBuild/Cache" >}}).

For more information on the `inline` cache backend, see the [BuildKit README](https://github.com/moby/buildkit#inline-push-image-and-cache-together).
