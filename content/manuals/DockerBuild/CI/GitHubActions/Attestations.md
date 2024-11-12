+++
title = "认证"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/ci/github-actions/attestations/](https://docs.docker.com/build/ci/github-actions/attestations/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Add SBOM and provenance attestations with GitHub Actions - 使用 GitHub Actions 添加 SBOM 和来源认证

Software Bill of Material (SBOM) and provenance [attestations]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations" >}}) add metadata about the contents of your image, and how it was built.

​	软件物料清单（SBOM）和来源 [认证]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations" >}})为镜像的内容和构建方式添加元数据。

Attestations are supported with version 4 and later of the `docker/build-push-action`.

​	`docker/build-push-action` 版本 4 及以上支持认证。

## 默认来源认证 Default provenance

The `docker/build-push-action` GitHub Action automatically adds provenance attestations to your image, with the following conditions:

​	`docker/build-push-action` GitHub Action 自动向您的镜像添加来源认证，满足以下条件：

- If the GitHub repository is public, provenance attestations with `mode=max` are automatically added to the image.
  - 如果 GitHub 仓库是公开的，镜像会自动添加 `mode=max` 的来源认证。

- If the GitHub repository is private, provenance attestations with `mode=min` are automatically added to the image.

  - 如果 GitHub 仓库是私有的，镜像会自动添加 `mode=min` 的来源认证。

- If you're using the [`docker` exporter]({{< ref "/manuals/DockerBuild/Exporters/OCIandDockerexporters" >}}), or you're loading the build results to the runner with `load: true`, no attestations are added to the image. These output formats don't support attestations.

  - 如果您使用 [`docker` 输出格式]({{< ref "/manuals/DockerBuild/Exporters/OCIandDockerexporters" >}})，或通过 `load: true` 将构建结果加载到运行器中，则不会向镜像添加认证。这些输出格式不支持认证。

  

> **Warning**
>
> 
>
> If you're using `docker/build-push-action` to build images for code in a public GitHub repository, the provenance attestations attached to your image by default contains the values of build arguments. If you're misusing build arguments to pass secrets to your build, such as user credentials or authentication tokens, those secrets are exposed in the provenance attestation. Refactor your build to pass those secrets using [secret mounts](https://docs.docker.com/reference/cli/docker/buildx/build/#secret) instead. Also remember to rotate any secrets you may have exposed.
>
> ​	如果您在公开的 GitHub 仓库中使用 `docker/build-push-action` 构建镜像，默认情况下附加到镜像的来源认证中包含构建参数的值。如果误将敏感信息（如用户凭证或身份验证令牌）通过构建参数传递，它们会暴露在来源认证中。请重新设计构建流程，通过 [secret mounts](https://docs.docker.com/reference/cli/docker/buildx/build/#secret) 传递这些敏感信息。此外，务必旋转任何可能暴露的敏感信息。

## Max 级别的来源认证 Max-level provenance

It's recommended that you build your images with max-level provenance attestations. Private repositories only add min-level provenance by default, but you can manually override the provenance level by setting the `provenance` input on the `docker/build-push-action` GitHub Action to `mode=max`.

​	建议您使用 max 级别的来源认证构建镜像。默认情况下，私有仓库只添加 min 级别的认证，但您可以通过在 `docker/build-push-action` GitHub Action 中将 `provenance` 输入设置为 `mode=max` 来手动覆盖来源级别。

Note that adding attestations to an image means you must push the image to a registry directly, as opposed to loading the image to the local image store of the runner. This is because the local image store doesn't support loading images with attestations.

​	请注意，向镜像添加认证意味着必须将镜像直接推送到注册表，而不是将镜像加载到运行器的本地镜像存储中。这是因为本地镜像存储不支持加载带有认证的镜像。

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

      - name: Build and push image
        uses: docker/build-push-action@v6
        with:
          push: true
          provenance: mode=max
          tags: ${{ steps.meta.outputs.tags }}
```

## SBOM

SBOM attestations aren't automatically added to the image. To add SBOM attestations, set the `sbom` input of the `docker/build-push-action` to true.

​	SBOM 认证不会自动添加到镜像。要添加 SBOM 认证，请将 `docker/build-push-action` 的 `sbom` 输入设置为 true。

Note that adding attestations to an image means you must push the image to a registry directly, as opposed to loading the image to the local image store of the runner. This is because the local image store doesn't support loading images with attestations.

​	请注意，向镜像添加认证意味着必须将镜像直接推送到注册表，而不是将镜像加载到运行器的本地镜像存储中。这是因为本地镜像存储不支持加载带有认证的镜像。



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

      - name: Build and push image
        uses: docker/build-push-action@v6
        with:
          sbom: true
          push: true
          tags: ${{ steps.meta.outputs.tags }}
```
