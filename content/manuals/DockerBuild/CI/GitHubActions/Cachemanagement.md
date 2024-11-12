+++
title = "Cache management"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/ci/github-actions/cache/](https://docs.docker.com/build/ci/github-actions/cache/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Cache management with GitHub Actions - 使用 GitHub Actions 管理缓存

This page contains examples on using the cache storage backends with GitHub Actions.

​	本文包含使用缓存存储后端与 GitHub Actions 配合的示例。

> **Note**
>
> 
>
> See [Cache storage backends]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}}) for more details about cache storage backends.
>
> ​	有关缓存存储后端的更多详情，请参见 [缓存存储后端]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}})。

## 内联缓存 Inline cache

In most cases you want to use the [inline cache exporter]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends/Inlinecache" >}}). However, note that the `inline` cache exporter only supports `min` cache mode. To use `max` cache mode, push the image and the cache separately using the registry cache exporter with the `cache-to` option, as shown in the [registry cache example](https://docs.docker.com/build/ci/github-actions/cache/#registry-cache).

​	在大多数情况下，建议使用 [内联缓存导出器]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends/Inlinecache" >}})。但需注意的是，`inline` 缓存导出器仅支持 `min` 缓存模式。要使用 `max` 缓存模式，请分别推送镜像和缓存，使用注册表缓存导出器并通过 `cache-to` 选项来实现，如在[注册表缓存示例](https://docs.docker.com/build/ci/github-actions/cache/#registry-cache)中所示。



```yaml
name: ci

on:
  push:

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
      
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: user/app:latest
          cache-from: type=registry,ref=user/app:latest
          cache-to: type=inline
```

## 注册表缓存 Registry cache

You can import/export cache from a cache manifest or (special) image configuration on the registry with the [registry cache exporter]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends/Registrycache" >}}).

​	通过 [注册表缓存导出器]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends/Registrycache" >}})，可以从缓存清单或注册表上的特殊镜像配置导入/导出缓存。



```yaml
name: ci

on:
  push:

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
      
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: user/app:latest
          cache-from: type=registry,ref=user/app:buildcache
          cache-to: type=registry,ref=user/app:buildcache,mode=max
```

## GitHub 缓存 GitHub cache

### 缓存后端 API - Cache backend API

**实验性 Experimental**

This cache exporter is experimental. Please provide feedback on the [BuildKit repository](https://github.com/moby/buildkit) if you experience any issues.

​	此缓存导出器是实验性的。如有任何问题，请在 [BuildKit 仓库](https://github.com/moby/buildkit)提供反馈。

The [GitHub Actions cache exporter]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends/GitHubActionscache" >}}) backend uses the [GitHub Cache API](https://github.com/tonistiigi/go-actions-cache/blob/master/api.md) to fetch and upload cache blobs. That's why you should only use this cache backend in a GitHub Action workflow, as the `url` (`$ACTIONS_CACHE_URL`) and `token` (`$ACTIONS_RUNTIME_TOKEN`) attributes only get populated in a workflow context.

​	[GitHub Actions 缓存导出器]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends/GitHubActionscache" >}})后端使用 [GitHub 缓存 API](https://github.com/tonistiigi/go-actions-cache/blob/master/api.md)来获取和上传缓存块。因此，应仅在 GitHub Action 工作流中使用此缓存后端，因为 `url`（`$ACTIONS_CACHE_URL`）和 `token`（`$ACTIONS_RUNTIME_TOKEN`）属性仅在工作流上下文中被填充。



```yaml
name: ci

on:
  push:

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
      
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: user/app:latest
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

### 缓存挂载 Cache mounts

BuildKit doesn't preserve cache mounts in the GitHub Actions cache by default. If you wish to put your cache mounts into GitHub Actions cache and reuse it between builds, you can use a workaround provided by [`reproducible-containers/buildkit-cache-dance`](https://github.com/reproducible-containers/buildkit-cache-dance).

​	默认情况下，BuildKit 不会在 GitHub Actions 缓存中保留缓存挂载。若希望将缓存挂载放入 GitHub Actions 缓存并在构建之间重用，可使用 [`reproducible-containers/buildkit-cache-dance`](https://github.com/reproducible-containers/buildkit-cache-dance) 提供的解决方案。

This GitHub Action creates temporary containers to extract and inject the cache mount data with your Docker build steps.

​	此 GitHub Action 创建临时容器，以便在 Docker 构建步骤中提取和注入缓存挂载数据。

The following example shows how to use this workaround with a Go project.

​	以下示例展示了如何使用此解决方案构建 Go 项目。

Example Dockerfile in `build/package/Dockerfile`

​	在 `build/package/Dockerfile` 中的示例 Dockerfile：

```Dockerfile
FROM golang:1.21.1-alpine as base-build

WORKDIR /build
RUN go env -w GOMODCACHE=/root/.cache/go-build

COPY go.mod go.sum ./
RUN --mount=type=cache,target=/root/.cache/go-build go mod download

COPY ./src ./
RUN --mount=type=cache,target=/root/.cache/go-build go build -o /bin/app /build/src
...
```

Example CI action

​	示例 CI 操作：

```yaml
name: ci

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Docker meta
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: YOUR_IMAGE
          tags: |
            type=ref,event=branch
            type=ref,event=pr
            type=semver,pattern={{version}}
            type=semver,pattern={{major}}.{{minor}}            

      - name: Go Build Cache for Docker
        uses: actions/cache@v4
        with:
          path: go-build-cache
          key: ${{ runner.os }}-go-build-cache-${{ hashFiles('**/go.sum') }}

      - name: inject go-build-cache into docker
        uses: reproducible-containers/buildkit-cache-dance@4b2444fec0c0fb9dbf175a96c094720a692ef810 # v2.1.4
        with:
          cache-source: go-build-cache

      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          cache-from: type=gha
          cache-to: type=gha,mode=max
          file: build/package/Dockerfile
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          platforms: linux/amd64,linux/arm64
```

For more information about this workaround, refer to the [GitHub repository](https://github.com/reproducible-containers/buildkit-cache-dance).

​	有关此解决方案的更多信息，请参考 [GitHub 仓库](https://github.com/reproducible-containers/buildkit-cache-dance)。

### 本地缓存 Local cache

> **Warning**
>
> 
>
> At the moment, old cache entries aren't deleted, so the cache size [keeps growing](https://github.com/docker/build-push-action/issues/252). The following example uses the `Move cache` step as a workaround (see [`moby/buildkit#1896`](https://github.com/moby/buildkit/issues/1896) for more info).
>
> ​	当前旧的缓存条目不会被删除，因此缓存大小[会不断增长](https://github.com/docker/build-push-action/issues/252)。以下示例使用 `Move cache` 步骤作为一种解决方案（有关更多信息，请参阅 [`moby/buildkit#1896`](https://github.com/moby/buildkit/issues/1896)）。

You can also leverage [GitHub cache](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows) using the [actions/cache](https://github.com/actions/cache) and [local cache exporter]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends/Localcache" >}}) with this action:

​	你还可以结合使用 [GitHub 缓存](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows)、[actions/cache](https://github.com/actions/cache) 和 [本地缓存导出器]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends/Localcache" >}})。

```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Cache Docker layers
        uses: actions/cache@v4
        with:
          path: /tmp/.buildx-cache
          key: ${{ runner.os }}-buildx-${{ github.sha }}
          restore-keys: |
            ${{ runner.os }}-buildx-            
      
      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: user/app:latest
          cache-from: type=local,src=/tmp/.buildx-cache
          cache-to: type=local,dest=/tmp/.buildx-cache-new,mode=max
      
      - # Temp fix
        # https://github.com/docker/build-push-action/issues/252
        # https://github.com/moby/buildkit/issues/1896
        name: Move cache
        run: |
          rm -rf /tmp/.buildx-cache
          mv /tmp/.buildx-cache-new /tmp/.buildx-cache     
```
