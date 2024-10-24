+++
title = "Annotations"
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

# Add image annotations with GitHub Actions

Annotations let you specify arbitrary metadata for OCI image components, such as manifests, indexes, and descriptors.

To add annotations when building images with GitHub Actions, use the [metadata-action](https://github.com/docker/metadata-action#overwrite-labels-and-annotations) to automatically create OCI-compliant annotations. The metadata action creates an `annotations` output that you can reference, both with [build-push-action](https://github.com/docker/build-push-action/) and [bake-action](https://github.com/docker/bake-action/).



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

## Configure annotation level

By default, annotations are placed on image manifests. To configure the [annotation level](https://docs.docker.com/build/metadata/annotations/#specify-annotation-level), set the `DOCKER_METADATA_ANNOTATIONS_LEVELS` environment variable on the `metadata-action` step to a comma-separated list of all the levels that you want to annotate. For example, setting `DOCKER_METADATA_ANNOTATIONS_LEVELS` to `index` results in annotations on the image index instead of the manifests.

The following example creates annotations on both the image index and manifests.



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
