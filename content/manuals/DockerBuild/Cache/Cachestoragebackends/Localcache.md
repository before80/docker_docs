+++
title = "本地缓存"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/backends/local/](https://docs.docker.com/build/cache/backends/local/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Local cache - 本地缓存

The `local` cache store is a simple cache option that stores your cache as files in a directory on your filesystem, using an [OCI image layout](https://github.com/opencontainers/image-spec/blob/main/image-layout.md) for the underlying directory structure. Local cache is a good choice if you're just testing, or if you want the flexibility to self-manage a shared storage solution.

​	`local` 缓存存储是一种简单的缓存选项，将缓存存储为文件系统目录中的文件，使用 [OCI 镜像布局](https://github.com/opencontainers/image-spec/blob/main/image-layout.md) 作为底层目录结构。本地缓存是进行测试或需要灵活管理共享存储解决方案的不错选择。

## 概要 Synopsis



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=local,dest=path/to/local/dir[,parameters...] \
  --cache-from type=local,src=path/to/local/dir .
```

The following table describes the available CSV parameters that you can pass to `--cache-to` and `--cache-from`.

​	下表描述了可用于 `--cache-to` 和 `--cache-from` 的 CSV 参数：

| Name                | Option       | Type                    | Default | Description                                                  |
| ------------------- | ------------ | ----------------------- | ------- | ------------------------------------------------------------ |
| `src`               | `cache-from` | String                  |         | 从中导入缓存的本地目录路径。 Path of the local directory where cache gets imported from. |
| `digest`            | `cache-from` | String                  |         | 要导入的清单摘要，参见 [缓存版本控制](https://docs.docker.com/build/cache/backends/local/#cache-versioning)。 Digest of manifest to import, see [cache versioning](https://docs.docker.com/build/cache/backends/local/#cache-versioning). |
| `dest`              | `cache-to`   | String                  |         | 导出缓存的本地目录路径。 Path of the local directory where cache gets exported to. |
| `mode`              | `cache-to`   | `min`,`max`             | `min`   | 要导出的缓存层，参见 [缓存模式](https://docs.docker.com/build/cache/backends/#cache-mode)。 Cache layers to export, see [cache mode](https://docs.docker.com/build/cache/backends/#cache-mode). |
| `oci-mediatypes`    | `cache-to`   | `true`,`false`          | `true`  | 在导出清单中使用 OCI 媒体类型，参见 [OCI 媒体类型](https://docs.docker.com/build/cache/backends/#oci-media-types)。 Use OCI media types in exported manifests, see [OCI media types](https://docs.docker.com/build/cache/backends/#oci-media-types). |
| `image-manifest`    | `cache-to`   | `true`,`false`          | `false` | 使用 OCI 媒体类型时，为缓存生成镜像清单而不是镜像索引，参见 [OCI 媒体类型](https://docs.docker.com/build/cache/backends/#oci-media-types)。 When using OCI media types, generate an image manifest instead of an image index for the cache image, see [OCI media types](https://docs.docker.com/build/cache/backends/#oci-media-types). |
| `compression`       | `cache-to`   | `gzip`,`estargz`,`zstd` | `gzip`  | 压缩类型，参见 [缓存压缩](https://docs.docker.com/build/cache/backends/#cache-compression)。Compression type, see [cache compression](https://docs.docker.com/build/cache/backends/#cache-compression). |
| `compression-level` | `cache-to`   | `0..22`                 |         | 压缩级别，参见 [缓存压缩](https://docs.docker.com/build/cache/backends/#cache-compression)。Compression level, see [cache compression](https://docs.docker.com/build/cache/backends/#cache-compression). |
| `force-compression` | `cache-to`   | `true`,`false`          | `false` | 强制压缩，参见 [缓存压缩](https://docs.docker.com/build/cache/backends/#cache-compression)。 Forcibly apply compression, see [cache compression](https://docs.docker.com/build/cache/backends/#cache-compression). |
| `ignore-error`      | `cache-to`   | Boolean                 | `false` | 忽略缓存导出失败导致的错误。 Ignore errors caused by failed cache exports. |

If the `src` cache doesn't exist, then the cache import step will fail, but the build continues.

​	如果 `src` 缓存不存在，则缓存导入步骤将失败，但构建会继续进行。

## 缓存版本控制 Cache versioning

This section describes how versioning works for caches on a local filesystem, and how you can use the `digest` parameter to use older versions of cache.

​	此部分描述了本地文件系统缓存的版本控制方式，以及如何使用 `digest` 参数来使用旧版本缓存。

If you inspect the cache directory manually, you can see the resulting OCI image layout:

​	如果手动检查缓存目录，可以看到生成的 OCI 镜像布局：



```console
$ ls cache
blobs  index.json  ingest
$ cat cache/index.json | jq
{
  "schemaVersion": 2,
  "manifests": [
    {
      "mediaType": "application/vnd.oci.image.index.v1+json",
      "digest": "sha256:6982c70595cb91769f61cd1e064cf5f41d5357387bab6b18c0164c5f98c1f707",
      "size": 1560,
      "annotations": {
        "org.opencontainers.image.ref.name": "latest"
      }
    }
  ]
}
```

Like other cache types, local cache gets replaced on export, by replacing the contents of the `index.json` file. However, previous caches will still be available in the `blobs` directory. These old caches are addressable by digest, and kept indefinitely. Therefore, the size of the local cache will continue to grow (see [`moby/buildkit#1896`](https://github.com/moby/buildkit/issues/1896) for more information).

​	与其他缓存类型类似，本地缓存在导出时会替换 `index.json` 文件的内容。然而，以前的缓存仍保留在 `blobs` 目录中。这些旧缓存可通过摘要进行访问，并且会永久保留。因此，本地缓存的大小会持续增长（更多信息参见 [`moby/buildkit#1896`](https://github.com/moby/buildkit/issues/1896)）。

When importing cache using `--cache-to`, you can specify the `digest` parameter to force loading an older version of the cache, for example:

​	使用 `--cache-from` 导入缓存时，可以指定 `digest` 参数以强制加载旧版本缓存，例如：



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=local,dest=path/to/local/dir \
  --cache-from type=local,ref=path/to/local/dir,digest=sha256:6982c70595cb91769f61cd1e064cf5f41d5357387bab6b18c0164c5f98c1f707 .
```

## Further reading

For an introduction to caching see [Docker build cache]({{< ref "/manuals/DockerBuild/Cache" >}}).

​	有关缓存的介绍，请参见 Docker 构建缓存。

For more information on the `local` cache backend, see the [BuildKit README](https://github.com/moby/buildkit#local-directory-1).

​	有关 `local` 缓存后端的更多信息，请参阅 [BuildKit README](https://github.com/moby/buildkit#local-directory-1)。

