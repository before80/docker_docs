+++
title = "OCI and Docker exporters"
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

# OCI and Docker exporters

The `oci` exporter outputs the build result into an [OCI image layout](https://github.com/opencontainers/image-spec/blob/main/image-layout.md) tarball. The `docker` exporter behaves the same way, except it exports a Docker image layout instead.

The [`docker` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}}) doesn't support these exporters. You must use `docker-container` or some other driver if you want to generate these outputs.

## Synopsis

Build a container image using the `oci` and `docker` exporters:



```console
$ docker buildx build --output type=oci[,parameters] .
```



```console
$ docker buildx build --output type=docker[,parameters] .
```

The following table describes the available parameters:

| Parameter           | Type                                   | Default | Description                                                  |
| ------------------- | -------------------------------------- | ------- | ------------------------------------------------------------ |
| `name`              | String                                 |         | Specify image name(s)                                        |
| `dest`              | String                                 |         | Path                                                         |
| `tar`               | `true`,`false`                         | `true`  | Bundle the output into a tarball layout                      |
| `compression`       | `uncompressed`,`gzip`,`estargz`,`zstd` | `gzip`  | Compression type, see [compression](https://docs.docker.com/build/exporters/#compression) |
| `compression-level` | `0..22`                                |         | Compression level, see [compression](https://docs.docker.com/build/exporters/#compression) |
| `force-compression` | `true`,`false`                         | `false` | Forcefully apply compression, see [compression](https://docs.docker.com/build/exporters/#compression) |
| `oci-mediatypes`    | `true`,`false`                         |         | Use OCI media types in exporter manifests. Defaults to `true` for `type=oci`, and `false` for `type=docker`. See [OCI Media types](https://docs.docker.com/build/exporters/#oci-media-types) |
| `annotation.<key>`  | String                                 |         | Attach an annotation with the respective `key` and `value` to the built image,see [annotations](https://docs.docker.com/build/exporters/oci-docker/#annotations) |

## Annotations

These exporters support adding OCI annotation using `annotation` parameter, followed by the annotation name using dot notation. The following example sets the `org.opencontainers.image.title` annotation:



```console
$ docker buildx build \
    --output "type=<type>,name=<registry>/<image>,annotation.org.opencontainers.image.title=<title>" .
```

For more information about annotations, see [BuildKit documentation](https://github.com/moby/buildkit/blob/master/docs/annotations.md).

## Further reading

For more information on the `oci` or `docker` exporters, see the [BuildKit README](https://github.com/moby/buildkit/blob/master/README.md#docker-tarball).
