+++
title = "构建声明"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/metadata/attestations/](https://docs.docker.com/build/metadata/attestations/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Build attestations - 构建声明

Build attestations describe how an image was built, and what it contains. The attestations are created at build-time by BuildKit, and become attached to the final image as metadata.

​	构建声明描述了镜像的构建方式及其内容。声明在构建时由 BuildKit 创建，并作为元数据附加到最终镜像上。

The purpose of attestations is to make it possible to inspect an image and see where it comes from, who created it and how, and what it contains. This enables you to make informed decisions about how an image impacts the supply chain security of your application. It also enables the use of policy engines for validating images based on policy rules you've defined.

​	声明的目的是能够检查镜像的来源、创建人及其包含的内容，从而帮助您对镜像如何影响应用的供应链安全做出明智决策。此外，声明还支持基于您定义的策略规则来使用策略引擎验证镜像。

Two types of build annotations are available:

​	提供了两种类型的构建声明：

- Software Bill of Material (SBOM): list of software artifacts that an image contains, or that were used to build the image.
  - 软件物料清单 (SBOM)：列出镜像中包含的或用于构建镜像的软件工件。

- Provenance: how an image was built.

  - 溯源：描述镜像的构建方式。

  

## 声明的目的 Purpose of attestations

The use of open source and third-party packages is more widespread than ever before. Developers share and reuse code because it helps increase productivity, allowing teams to create better products, faster.

​	开源和第三方包的使用比以往更加广泛。开发人员共享和重用代码，以帮助提高生产力，加快产品开发。

Importing and using code created elsewhere without vetting it introduces a severe security risk. Even if you do review the software that you consume, new zero-day vulnerabilities are frequently discovered, requiring development teams take action to remediate them.

​	引入和使用未经验证的代码会带来严重的安全风险。即使您审查了所使用的软件，也可能会有新的零日漏洞被发现，从而需要开发团队采取措施进行补救。

Build attestations make it easier to see the contents of an image, and where it comes from. Use attestations to analyze and decide whether to use an image, or to see if images you are already using are exposed to vulnerabilities.

​	构建声明使得查看镜像的内容及其来源变得更容易。通过声明分析是否使用某个镜像，或检查您已在使用的镜像是否存在漏洞。

## 创建声明 Creating attestations

When you build an image with `docker buildx build`, you can add attestation records to the resulting image using the `--provenance` and `--sbom` options. You can opt in to add either the SBOM or provenance attestation type, or both.

​	使用 `docker buildx build` 构建镜像时，可以通过 `--provenance` 和 `--sbom` 选项向生成的镜像添加声明记录。您可以选择添加 SBOM 或溯源声明类型，或同时添加两者。





```console
$ docker buildx build --sbom=true --provenance=true .
```

> **Note**
>
> 
>
> The default image store doesn't support attestations. If you're using the default image store and you build an image using the default `docker` driver, or using a different driver with the `--load` flag, the attestations are lost.
>
> ​	默认镜像存储不支持声明。如果使用默认镜像存储并通过默认的 `docker` 驱动或使用 `--load` 标志构建镜像，声明将会丢失。
>
> To make sure the attestations are preserved, you can:
>
> ​	要确保声明得以保留，可以：
>
> - Use a `docker-container` driver with the `--push` flag to push the image to a registry directly.
>   - 使用 `docker-container` 驱动并加上 `--push` 标志，将镜像直接推送到注册表。
> - Enable the [containerd image store]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}}).
>   - 启用 [containerd 镜像存储]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}})。

> **Note**
>
> 
>
> Provenance attestations are enabled by default, with the `mode=min` option. You can disable provenance attestations using the `--provenance=false` flag, or by setting the [`BUILDX_NO_DEFAULT_ATTESTATIONS`](https://docs.docker.com/build/building/variables/#buildx_no_default_attestations) environment variable.
>
> ​	溯源声明默认启用，模式为 `mode=min`。可以使用 `--provenance=false` 标志或设置环境变量 [`BUILDX_NO_DEFAULT_ATTESTATIONS`](https://docs.docker.com/build/building/variables/#buildx_no_default_attestations) 来禁用溯源声明。
>
> Using the `--provenance=true` flag attaches provenance attestations with `mode=max` by default. See [Provenance attestation]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/Provenanceattestations" >}}) for more details.
>
> ​	使用 `--provenance=true` 标志默认会附加 `mode=max` 的溯源声明。详情参见[溯源声明]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/Provenanceattestations" >}})。

BuildKit generates the attestations when building the image. The attestation records are wrapped in the in-toto JSON format and attached to the image index in a manifest for the final image.

​	BuildKit 在构建镜像时生成声明。声明记录以 in-toto JSON 格式包装，并作为清单附加到镜像索引中。

## 存储 Storage

BuildKit produces attestations in the [in-toto format](https://github.com/in-toto/attestation), as defined by the [in-toto framework](https://in-toto.io/), a standard supported by the Linux Foundation.

​	BuildKit 生成的声明采用 [in-toto 格式](https://github.com/in-toto/attestation)，由 Linux 基金会支持的 [in-toto 框架](https://in-toto.io/)定义。

Attestations attach to images as a manifest in the image index. The data records of the attestations are stored as JSON blobs.

​	声明作为清单附加在镜像索引上，数据记录以 JSON blobs 的形式存储。

Because attestations attach to images as a manifest, it means that you can inspect the attestations for any image in a registry without having to pull the whole image.

​	由于声明附加在镜像的清单上，您可以在不拉取整个镜像的情况下检查注册表中任何镜像的声明。

All BuildKit exporters support attestations. The `local` and `tar` can't save the attestations to an image manifest, since it's outputting a directory of files or a tarball, not an image. Instead, these exporters write the attestations to one or more JSON files in the root directory of the export.

​	所有 BuildKit 导出器都支持声明。`local` 和 `tar` 无法将声明保存到镜像清单中，因为它们输出的是文件目录或 tar 包而非镜像。相反，这些导出器会将声明写入导出根目录中的一个或多个 JSON 文件中。

The following example shows a truncated in-toto JSON representation of an SBOM attestation.

​	以下示例展示了 SBOM 声明的截断版 in-toto JSON 表示。

```json
{
  "_type": "https://in-toto.io/Statement/v0.1",
  "predicateType": "https://spdx.dev/Document",
  "subject": [
    {
      "name": "pkg:docker/<registry>/<image>@<tag/digest>?platform=<platform>",
      "digest": {
        "sha256": "e8275b2b76280af67e26f068e5d585eb905f8dfd2f1918b3229db98133cb4862"
      }
    }
  ],
  "predicate": {
    "SPDXID": "SPDXRef-DOCUMENT",
    "creationInfo": {
      "created": "2022-12-15T11:47:54.546747383Z",
      "creators": ["Organization: Anchore, Inc", "Tool: syft-v0.60.3"],
      "licenseListVersion": "3.18"
    },
    "dataLicense": "CC0-1.0",
    "documentNamespace": "https://anchore.com/syft/dir/run/src/core-da0f600b-7f0a-4de0-8432-f83703e6bc4f",
    "name": "/run/src/core",
    // list of files that the image contains, e.g.:
    // 镜像包含的文件列表，例如：
    "files": [
      {
        "SPDXID": "SPDXRef-1ac501c94e2f9f81",
        "comment": "layerID: sha256:9b18e9b68314027565b90ff6189d65942c0f7986da80df008b8431276885218e",
        "fileName": "/bin/busybox",
        "licenseConcluded": "NOASSERTION"
      }
    ],
    // list of packages that were identified for this image:
    // 镜像中识别出的包列表：
    "packages": [
      {
        "name": "busybox",
        "originator": "Person: Sören Tempel <soeren+alpine@soeren-tempel.net>",
        "sourceInfo": "acquired package info from APK DB: lib/apk/db/installed",
        "versionInfo": "1.35.0-r17",
        "SPDXID": "SPDXRef-980737451f148c56",
        "description": "Size optimized toolbox of many common UNIX utilities",
        "downloadLocation": "https://busybox.net/",
        "licenseConcluded": "GPL-2.0-only",
        "licenseDeclared": "GPL-2.0-only"
        // ...
      }
    ],
    // files-packages relationship
    // 文件与包的关系
    "relationships": [
      {
        "relatedSpdxElement": "SPDXRef-1ac501c94e2f9f81",
        "relationshipType": "CONTAINS",
        "spdxElementId": "SPDXRef-980737451f148c56"
      },
      ...
    ],
    "spdxVersion": "SPDX-2.2"
  }
}
```

To deep-dive into the specifics about how attestations are stored, see [Image Attestation Storage (BuildKit)](https://docs.docker.com/build/metadata/attestations/attestation-storage/).

​	有关声明存储的具体细节，请参见[镜像声明存储（BuildKit）](https://docs.docker.com/build/metadata/attestations/attestation-storage/)。

## What's next

Learn more about the available attestation types and how to use them:

​	了解更多可用的声明类型及其使用方法：

- [Provenance 溯源]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/Provenanceattestations" >}})
- [SBOM]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/SBOMattestations" >}})
