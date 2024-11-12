+++
title = "注释"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/ci/github-actions/annotations/](https://docs.docker.com/build/ci/github-actions/annotations/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Add image annotations with GitHub Actions - 使用 GitHub Actions 添加镜像注释

Annotations let you specify arbitrary metadata for OCI image components, such as manifests, indexes, and descriptors.

​	注释允许为 OCI 镜像组件（如清单、索引和描述符）指定任意元数据。

To add annotations when building images with GitHub Actions, use the [metadata-action](https://github.com/docker/metadata-action#overwrite-labels-and-annotations) to automatically create OCI-compliant annotations. The metadata action creates an `annotations` output that you can reference, both with [build-push-action](https://github.com/docker/build-push-action/) and [bake-action](https://github.com/docker/bake-action/).

​	在使用 GitHub Actions 构建镜像时，可以使用 [metadata-action](https://github.com/docker/metadata-action#overwrite-labels-and-annotations) 自动创建符合 OCI 的注释。metadata action 创建一个 `annotations` 输出，您可以在 [build-push-action](https://github.com/docker/build-push-action/) 和 [bake-action](https://github.com/docker/bake-action/) 中引用该输出。



{{< tabpane text=true persist=disabled >}}

{{% tab header="build-push-action" %}}

```yaml
name: ci

on:
  push:

env:
  IMAGE_NAME: user/app

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.IMAGE_NAME }}

      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          tags: ${{ steps.meta.outputs.tags }}
          annotations: ${{ steps.meta.outputs.annotations }}
          push: true
```

{{% /tab  %}}

{{% tab header="bake-action" %}}

```yaml
name: ci

on:
  push:

env:
  IMAGE_NAME: user/app

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.IMAGE_NAME }}

      - name: Build
        uses: docker/bake-action@v5
        with:
          files: |
            ./docker-bake.hcl
            ${{ steps.meta.outputs.bake-file-tags }}
            ${{ steps.meta.outputs.bake-file-annotations }}
          push: true
```

{{% /tab  %}}

{{< /tabpane >}}

 

------

## 配置注释级别 Configure annotation level

By default, annotations are placed on image manifests. To configure the [annotation level](https://docs.docker.com/build/metadata/annotations/#specify-annotation-level), set the `DOCKER_METADATA_ANNOTATIONS_LEVELS` environment variable on the `metadata-action` step to a comma-separated list of all the levels that you want to annotate. For example, setting `DOCKER_METADATA_ANNOTATIONS_LEVELS` to `index` results in annotations on the image index instead of the manifests.

​	默认情况下，注释添加在镜像清单上。要配置 [注释级别](https://docs.docker.com/build/metadata/annotations/#specify-annotation-level)，在 `metadata-action` 步骤中将 `DOCKER_METADATA_ANNOTATIONS_LEVELS` 环境变量设置为包含所有希望添加注释的级别的逗号分隔列表。例如，将 `DOCKER_METADATA_ANNOTATIONS_LEVELS` 设置为 `index`，注释将添加在镜像索引而不是清单上。

The following example creates annotations on both the image index and manifests.

​	以下示例在镜像索引和清单上创建注释：

```yaml
name: ci

on:
  push:

env:
  IMAGE_NAME: user/app

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.IMAGE_NAME }}
        env:
          DOCKER_METADATA_ANNOTATIONS_LEVELS: manifest,index

      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          tags: ${{ steps.meta.outputs.tags }}
          annotations: ${{ steps.meta.outputs.annotations }}
          push: true
```

> **Note**
>
> 
>
> The build must produce the components that you want to annotate. For example, to annotate an image index, the build must produce an index. If the build produces only a manifest and you specify `index` or `index-descriptor`, the build fails.
>
> ​	构建必须生成要添加注释的组件。例如，要注释镜像索引，构建必须生成索引。如果构建只生成清单，而您指定了 `index` 或 `index-descriptor`，则构建将失败。
