+++
title = "注解"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/metadata/annotations/](https://docs.docker.com/build/metadata/annotations/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Annotations - 注解

Annotations provide descriptive metadata for images. Use annotations to record arbitrary information and attach it to your image, which helps consumers and tools understand the origin, contents, and how to use the image.

​	注解为镜像提供描述性元数据。使用注解可以记录任意信息并将其附加到镜像上，从而帮助用户和工具理解镜像的来源、内容和使用方法。

Annotations are similar to, and in some sense overlap with, [labels]({{< ref "/manuals/DockerEngine/Manageresources/Dockerobjectlabels" >}}). Both serve the same purpose: attach metadata to a resource. As a general principle, you can think of the difference between annotations and labels as follows:

​	注解与[标签]({{< ref "/manuals/DockerEngine/Manageresources/Dockerobjectlabels" >}})类似，在某种程度上具有相似的用途：将元数据附加到资源上。作为一般原则，可以将注解和标签的区别理解如下：

- Annotations describe OCI image components, such as [manifests](https://github.com/opencontainers/image-spec/blob/main/manifest.md), [indexes](https://github.com/opencontainers/image-spec/blob/main/image-index.md), and [descriptors](https://github.com/opencontainers/image-spec/blob/main/descriptor.md).
  - 注解描述 OCI 镜像组件，例如[清单](https://github.com/opencontainers/image-spec/blob/main/manifest.md)、[索引](https://github.com/opencontainers/image-spec/blob/main/image-index.md)和[描述符](https://github.com/opencontainers/image-spec/blob/main/descriptor.md)。

- Labels describe Docker resources, such as images, containers, networks, and volumes.
  - 标签描述 Docker 资源，例如镜像、容器、网络和卷。


The OCI image [specification](https://github.com/opencontainers/image-spec/blob/main/annotations.md) defines the format of annotations, as well as a set of pre-defined annotation keys. Adhering to the specified standards ensures that metadata about images can be surfaced automatically and consistently, by tools like Docker Scout.

​	OCI 镜像[规范](https://github.com/opencontainers/image-spec/blob/main/annotations.md)定义了注解的格式以及一组预定义的注解键。遵循该标准可确保工具（如 Docker Scout）能够自动、一致地展示镜像的元数据。

Annotations are not to be confused with [attestations]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations" >}}):

​	注解与[声明]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations" >}})不同：

- Attestations contain information about how an image was built and what it contains. An attestation is attached as a separate manifest on the image index. Attestations are not standardized by the Open Container Initiative.
  - 声明包含镜像的构建方式和内容信息，作为独立清单附加在镜像索引上。声明并未被开放容器项目（OCI）标准化。

- Annotations contain arbitrary metadata about an image. Annotations attach to the image [config](https://github.com/opencontainers/image-spec/blob/main/config.md) as labels, or on the image index or manifest as properties.
  - 注解包含镜像的任意元数据。注解作为标签附加在镜像[配置](https://github.com/opencontainers/image-spec/blob/main/config.md)上，或作为属性附加在镜像索引或清单上。


## 添加注解 Add annotations

You can add annotations to an image at build-time, or when creating the image manifest or index.

​	可以在构建时或创建镜像清单或索引时向镜像添加注解。

> **Note**
>
> 
>
> The Docker Engine image store doesn't support loading images with annotations. To build with annotations, make sure to push the image directly to a registry, using the `--push` CLI flag or the [registry exporter]({{< ref "/manuals/DockerBuild/Exporters/Imageandregistryexporters" >}}).
>
> ​	Docker 引擎的镜像存储不支持加载带有注解的镜像。要带注解进行构建，请确保直接将镜像推送到注册表，使用 `--push` CLI 标志或[注册表导出器]({{< ref "/manuals/DockerBuild/Exporters/Imageandregistryexporters" >}})。

To specify annotations on the command line, use the `--annotation` flag for the `docker build` command:

​	在命令行上指定注解，请使用 `docker build` 命令的 `--annotation` 标志：

```console
$ docker build --push --annotation "foo=bar" .
```

If you're using [Bake]({{< ref "/manuals/DockerBuild/Bake" >}}), you can use the `annotations` attribute to specify annotations for a given target:

​	如果使用 [Bake]({{< ref "/manuals/DockerBuild/Bake" >}})，可以使用 `annotations` 属性为目标指定注解：

```hcl
target "default" {
  output = ["type=registry"]
  annotations = ["foo=bar"]
}
```

For examples on how to add annotations to images built with GitHub Actions, see [Add image annotations with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Annotations" >}})

​	关于在 GitHub Actions 中向镜像添加注解的示例，请参见[使用 GitHub Actions 添加镜像注解]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Annotations" >}})。

You can also add annotations to an image created using `docker buildx imagetools create`. This command only supports adding annotations to an index or manifest descriptors, see [CLI reference](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#annotations).

​	也可以使用 `docker buildx imagetools create` 命令向镜像添加注解。此命令仅支持向索引或清单描述符添加注解，详情参见 [CLI 参考](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#annotations)。

## 检查注解 Inspect annotations

To view annotations on an **image index**, use the `docker buildx imagetools inspect` command. This shows you any annotations for the index and descriptors (references to manifests) that the index contains. The following example shows an `org.opencontainers.image.documentation` annotation on a descriptor, and an `org.opencontainers.image.authors` annotation on the index.

​	要查看**镜像索引**上的注解，使用 `docker buildx imagetools inspect` 命令。这将显示索引和包含的描述符（引用的清单）的任何注解。以下示例展示了描述符上的 `org.opencontainers.image.documentation` 注解，以及索引上的 `org.opencontainers.image.authors` 注解。



```console
$ docker buildx imagetools inspect IMAGE --raw
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.oci.image.index.v1+json",
  "manifests": [
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:d20246ef744b1d05a1dd69d0b3fa907db007c07f79fe3e68c17223439be9fefb",
      "size": 911,
      "annotations": {
        "org.opencontainers.image.documentation": "https://foo.example/docs",
      },
      "platform": {
        "architecture": "amd64",
        "os": "linux"
      }
    },
  ],
  "annotations": {
    "org.opencontainers.image.authors": "dvdksn"
  }
}
```

To inspect annotations on a manifest, use the `docker buildx imagetools inspect` command and specify `<IMAGE>@<DIGEST>`, where `<DIGEST>` is the digest of the manifest:

​	要检查清单上的注解，请使用 `docker buildx imagetools inspect` 命令，并指定 `<IMAGE>@<DIGEST>`，其中 `<DIGEST>` 是清单的摘要：



```console
$ docker buildx imagetools inspect IMAGE@sha256:d20246ef744b1d05a1dd69d0b3fa907db007c07f79fe3e68c17223439be9fefb --raw
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.oci.image.manifest.v1+json",
  "config": {
    "mediaType": "application/vnd.oci.image.config.v1+json",
    "digest": "sha256:4368b6959a78b412efa083c5506c4887e251f1484ccc9f0af5c406d8f76ece1d",
    "size": 850
  },
  "layers": [
    {
      "mediaType": "application/vnd.oci.image.layer.v1.tar+gzip",
      "digest": "sha256:2c03dbb20264f09924f9eab176da44e5421e74a78b09531d3c63448a7baa7c59",
      "size": 3333033
    },
    {
      "mediaType": "application/vnd.oci.image.layer.v1.tar+gzip",
      "digest": "sha256:4923ad480d60a548e9b334ca492fa547a3ce8879676685b6718b085de5aaf142",
      "size": 61887305
    }
  ],
  "annotations": {
    "index,manifest:org.opencontainers.image.vendor": "foocorp",
    "org.opencontainers.image.source": "https://git.example/foo.git",
  }
}
```

## 指定注解级别 Specify annotation level

By default, annotations are added to the image manifest. You can specify which level (OCI image component) to attach the annotation to by prefixing the annotation string with a special type declaration:

​	默认情况下，注解添加到镜像清单中。可以通过在注解字符串前添加特殊类型声明来指定注解的附加级别（OCI 镜像组件）：

```console
$ docker build --annotation "TYPE:KEY=VALUE" .
```

The following types are supported:

​	支持以下类型：

- `manifest`: annotates manifests. 注解清单。
- `index`: annotates the root index. 注解根索引。
- `manifest-descriptor`: annotates manifest descriptors in the index. 注解索引中的清单描述符。
- `index-descriptor`: annotates the index descriptor in the image layout. 注解镜像布局中的索引描述符。

For example, to build an image with the annotation `foo=bar` attached to the image index:

​	例如，构建一个带有附加到镜像索引的 `foo=bar` 注解的镜像：

```console
$ docker build --tag IMAGE --push --annotation "index:foo=bar" .
```

Note that the build must produce the component that you specify, or else the build will fail. For example, the following does not work, because the `docker` exporter does not produce an index:

​	注意，构建必须生成所指定的组件，否则构建将失败。例如，以下命令将无效，因为 `docker` 导出器不生成索引：

```console
$ docker build --output type=docker --annotation "index:foo=bar" .
```

Likewise, the following example also does not work, because buildx creates a `docker` output by default under some circumstances, such as when provenance attestations are explicitly disabled:

​	同样，以下示例也无效，因为在某些情况下，buildx 默认会创建 `docker` 输出，例如当明确禁用证明声明时：

```console
$ docker build --provenance=false --annotation "index:foo=bar" .
```

It is possible to specify types, separated by a comma, to add the annotation to more than one level. The following example creates an image with the annotation `foo=bar` on both the image index and the image manifest:

​	可以用逗号分隔的多种类型，将注解添加到多个级别。以下示例创建一个镜像，并在镜像索引和镜像清单上添加 `foo=bar` 注解：

```console
$ docker build --tag IMAGE --push --annotation "index,manifest:foo=bar" .
```

You can also specify a platform qualifier within square brackets in the type prefix, to annotate only components matching specific OS and architectures. The following example adds the `foo=bar` annotation only to the `linux/amd64` manifest:

​	还可以在类型前缀中使用方括号内的平台限定符，以仅注解符合特定操作系统和架构的组件。以下示例仅向 `linux/amd64` 清单添加 `foo=bar` 注解：

```console
$ docker build --tag IMAGE --push --annotation "manifest[linux/amd64]:foo=bar" .
```

## Related information

Related articles:

​	相关文章：

- [Add image annotations with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Annotations" >}})
  - [使用 GitHub Actions 添加镜像注解]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Annotations" >}})

- [Annotations OCI specification 注解的 OCI 规范](https://github.com/opencontainers/image-spec/blob/main/annotations.md)

Reference information:

​	参考信息：

- [`docker buildx build --annotation`](https://docs.docker.com/reference/cli/docker/buildx/build/#annotation)
- [Bake file reference: `annotations`](https://docs.docker.com/build/bake/reference/#targetannotations)
- [`docker buildx imagetools create --annotation`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#annotation)
