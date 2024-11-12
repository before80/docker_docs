+++
title = "镜像和注册表导出器"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/exporters/image-registry/](https://docs.docker.com/build/exporters/image-registry/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Image and registry exporters - 镜像和注册表导出器

The `image` exporter outputs the build result into a container image format. The `registry` exporter is identical, but it automatically pushes the result by setting `push=true`.

​	`image` 导出器将构建结果输出为容器镜像格式。`registry` 导出器与其相同，但自动设置 `push=true` 将结果推送。

## 概要 Synopsis

Build a container image using the `image` and `registry` exporters:

​	使用 `image` 和 `registry` 导出器构建容器镜像：

```console
$ docker buildx build --output type=image[,parameters] .
$ docker buildx build --output type=registry[,parameters] .
```

The following table describes the available parameters that you can pass to `--output` for `type=image`:

​	下表描述了可以传递给 `type=image` 的 `--output` 参数：

| Parameter              | Type                                   | Default | Description                                                  |
| ---------------------- | -------------------------------------- | ------- | ------------------------------------------------------------ |
| `name`                 | String                                 |         | 指定镜像名称Specify image name(s)                            |
| `push`                 | `true`,`false`                         | `false` | 创建镜像后推送。Push after creating the image.               |
| `push-by-digest`       | `true`,`false`                         | `false` | 推送镜像不带名称。Push image without name.                   |
| `registry.insecure`    | `true`,`false`                         | `false` | 允许推送到不安全的注册表。 Allow pushing to insecure registry. |
| `dangling-name-prefix` | `<value>`                              |         | 使用 `prefix@<digest>` 为匿名镜像命名 Name image with `prefix@<digest>`, used for anonymous images |
| `name-canonical`       | `true`,`false`                         |         | 添加附加规范名称 `name@<digest>` Add additional canonical name `name@<digest>` |
| `compression`          | `uncompressed`,`gzip`,`estargz`,`zstd` | `gzip`  | 压缩类型，见[压缩](https://docs.docker.com/build/exporters/#compression) Compression type, see [compression](https://docs.docker.com/build/exporters/#compression) |
| `compression-level`    | `0..22`                                |         | 压缩级别，见[压缩](https://docs.docker.com/build/exporters/#compression) Compression level, see [compression](https://docs.docker.com/build/exporters/#compression) |
| `force-compression`    | `true`,`false`                         | `false` | 强制应用压缩，见[压缩](https://docs.docker.com/build/exporters/#compression) Forcefully apply compression, see [compression](https://docs.docker.com/build/exporters/#compression) |
| `rewrite-timestamp`    | `true`,`false`                         | `false` | 将文件时间戳重写为 `SOURCE_DATE_EPOCH` 值。查看[构建可重现性](https://github.com/moby/buildkit/blob/master/docs/build-repro.md)了解如何指定 `SOURCE_DATE_EPOCH` 值。 Rewrite the file timestamps to the `SOURCE_DATE_EPOCH` value. See [build reproducibility](https://github.com/moby/buildkit/blob/master/docs/build-repro.md) for how to specify the `SOURCE_DATE_EPOCH` value. |
| `oci-mediatypes`       | `true`,`false`                         | `false` | 在导出清单中使用 OCI 媒体类型，见 [OCI 媒体类型](https://docs.docker.com/build/exporters/#oci-media-types) Use OCI media types in exporter manifests, see [OCI Media types](https://docs.docker.com/build/exporters/#oci-media-types) |
| `unpack`               | `true`,`false`                         | `false` | 创建镜像后解包（用于 containerd） Unpack image after creation (for use with containerd) |
| `store`                | `true`,`false`                         | `true`  | 将结果镜像存储到 worker 的镜像存储中，确保镜像的所有 blob 存在于内容存储中。对于没有镜像存储的 worker 会被忽略（如使用 OCI workers 时）。 Store the result images to the worker's (for example, containerd) image store, and ensures that the image has all blobs in the content store. Ignored if the worker doesn't have image store (when using OCI workers, for example). |
| `annotation.<key>`     | String                                 |         | 为构建的镜像附加具有相应 `key` 和 `value` 的注解，见 [注解](https://docs.docker.com/build/exporters/image-registry/#annotations) Attach an annotation with the respective `key` and `value` to the built image,see [annotations](https://docs.docker.com/build/exporters/image-registry/#annotations) |

## 注解 Annotations

These exporters support adding OCI annotation using `annotation` parameter, followed by the annotation name using dot notation. The following example sets the `org.opencontainers.image.title` annotation:

​	这些导出器支持使用 `annotation` 参数添加 OCI 注解，注解名称使用点符号表示。以下示例设置 `org.opencontainers.image.title` 注解：

```console
$ docker buildx build \
    --output "type=<type>,name=<registry>/<image>,annotation.org.opencontainers.image.title=<title>" .
```

For more information about annotations, see [BuildKit documentation](https://github.com/moby/buildkit/blob/master/docs/annotations.md).

​	有关注解的更多信息，请参阅 [BuildKit 文档](https://github.com/moby/buildkit/blob/master/docs/annotations.md)。

## Further reading

For more information on the `image` or `registry` exporters, see the [BuildKit README](https://github.com/moby/buildkit/blob/master/README.md#imageregistry).

​	有关 `image` 或 `registry` 导出器的更多信息，请参阅 [BuildKit README](https://github.com/moby/buildkit/blob/master/README.md#imageregistry)。
