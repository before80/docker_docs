+++
title = "OCI 和 Docker 导出器"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/exporters/oci-docker/](https://docs.docker.com/build/exporters/oci-docker/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# OCI and Docker exporters - OCI 和 Docker 导出器

The `oci` exporter outputs the build result into an [OCI image layout](https://github.com/opencontainers/image-spec/blob/main/image-layout.md) tarball. The `docker` exporter behaves the same way, except it exports a Docker image layout instead.

​	`oci` 导出器将构建结果导出为 [OCI 镜像布局](https://github.com/opencontainers/image-spec/blob/main/image-layout.md) tarball。`docker` 导出器的行为相同，但导出为 Docker 镜像布局。

The [`docker` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}}) doesn't support these exporters. You must use `docker-container` or some other driver if you want to generate these outputs.

​	[`docker` 驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}}) 不支持这些导出器。如果要生成这些输出，必须使用 `docker-container` 或其他驱动。

## 概要 Synopsis

Build a container image using the `oci` and `docker` exporters:

​	使用 `oci` 和 `docker` 导出器构建容器镜像：

```console
$ docker buildx build --output type=oci[,parameters] .
```



```console
$ docker buildx build --output type=docker[,parameters] .
```

The following table describes the available parameters:

​	下表描述了可用的参数：

| Parameter           | Type                                   | Default | Description                                                  |
| ------------------- | -------------------------------------- | ------- | ------------------------------------------------------------ |
| `name`              | String                                 |         | 指定镜像名称 Specify image name(s)                           |
| `dest`              | String                                 |         | 路径 Path                                                    |
| `tar`               | `true`,`false`                         | `true`  | 将输出打包到 tarball 布局中 Bundle the output into a tarball layout |
| `compression`       | `uncompressed`,`gzip`,`estargz`,`zstd` | `gzip`  | 压缩类型，见[压缩](https://docs.docker.com/build/exporters/#compression) Compression type, see [compression](https://docs.docker.com/build/exporters/#compression) |
| `compression-level` | `0..22`                                |         | 压缩级别，见[压缩](https://docs.docker.com/build/exporters/#compression) Compression level, see [compression](https://docs.docker.com/build/exporters/#compression) |
| `force-compression` | `true`,`false`                         | `false` | 强制应用压缩，见[压缩](https://docs.docker.com/build/exporters/#compression) Forcefully apply compression, see [compression](https://docs.docker.com/build/exporters/#compression) |
| `oci-mediatypes`    | `true`,`false`                         |         | 在导出清单中使用 OCI 媒体类型。对于 `type=oci` 默认为 `true`，对于 `type=docker` 默认为 `false`。见 [OCI 媒体类型](https://docs.docker.com/build/exporters/#oci-media-types) Use OCI media types in exporter manifests. Defaults to `true` for `type=oci`, and `false` for `type=docker`. See [OCI Media types](https://docs.docker.com/build/exporters/#oci-media-types) |
| `annotation.<key>`  | String                                 |         | 为构建的镜像附加具有相应 `key` 和 `value` 的注解，见 [注解](https://docs.docker.com/build/exporters/oci-docker/#annotations) Attach an annotation with the respective `key` and `value` to the built image,see [annotations](https://docs.docker.com/build/exporters/oci-docker/#annotations) |

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

For more information on the `oci` or `docker` exporters, see the [BuildKit README](https://github.com/moby/buildkit/blob/master/README.md#docker-tarball).

​	有关 `oci` 或 `docker` 导出器的更多信息，请参阅 [BuildKit README](https://github.com/moby/buildkit/blob/master/README.md#docker-tarball)。
