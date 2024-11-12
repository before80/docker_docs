+++
title = "GitHub Actions 缓存"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/backends/gha/](https://docs.docker.com/build/cache/backends/gha/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# GitHub Actions cache - GitHub Actions 缓存

**受限 Restricted**

This is an experimental feature. The interface and behavior are unstable and may change in future releases.

​	此功能为实验性功能，接口和行为不稳定，可能会在未来版本中发生变化。

The GitHub Actions cache utilizes the [GitHub-provided Action's cache](https://github.com/actions/cache) or other cache services supporting the GitHub Actions cache protocol. This is the recommended cache to use inside your GitHub Actions workflows, as long as your use case falls within the [size and usage limits set by GitHub](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy).

​	GitHub Actions 缓存利用了 [GitHub 提供的 Action 缓存](https://github.com/actions/cache) 或其他支持 GitHub Actions 缓存协议的缓存服务。只要您的使用情况在 [GitHub 设置的大小和使用限制](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy) 范围内，这是推荐在 GitHub Actions 工作流中使用的缓存。

This cache storage backend is not supported with the default `docker` driver. To use this feature, create a new builder using a different driver. See [Build drivers]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}) for more information.

​	默认的 `docker` 驱动不支持此缓存存储后端。要使用此功能，请使用不同的驱动创建一个新的构建器。更多信息参见 构建驱动。

## 概要 Synopsis



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=gha[,parameters...] \
  --cache-from type=gha[,parameters...] .
```

The following table describes the available CSV parameters that you can pass to `--cache-to` and `--cache-from`.

​	下表描述了可用于 `--cache-to` 和 `--cache-from` 的 CSV 参数：

| Name           | Option                  | Type        | Default                  | Description                                                  |
| -------------- | ----------------------- | ----------- | ------------------------ | ------------------------------------------------------------ |
| `url`          | `cache-to`,`cache-from` | String      | `$ACTIONS_CACHE_URL`     | 缓存服务器 URL，参见 [认证](https://docs.docker.com/build/cache/backends/gha/#authentication)。 Cache server URL, see [authentication](https://docs.docker.com/build/cache/backends/gha/#authentication). |
| `token`        | `cache-to`,`cache-from` | String      | `$ACTIONS_RUNTIME_TOKEN` | 访问令牌，参见 [认证](https://docs.docker.com/build/cache/backends/gha/#authentication)。 Access token, see [authentication](https://docs.docker.com/build/cache/backends/gha/#authentication). |
| `scope`        | `cache-to`,`cache-from` | String      | `buildkit`               | 缓存对象所属的作用域，参见 [作用域](https://docs.docker.com/build/cache/backends/gha/#scope) Which scope cache object belongs to, see [scope](https://docs.docker.com/build/cache/backends/gha/#scope) |
| `mode`         | `cache-to`              | `min`,`max` | `min`                    | 要导出的缓存层，参见 [缓存模式](https://docs.docker.com/build/cache/backends/#cache-mode)。 Cache layers to export, see [cache mode](https://docs.docker.com/build/cache/backends/#cache-mode). |
| `ignore-error` | `cache-to`              | Boolean     | `false`                  | 忽略缓存导出失败导致的错误。 Ignore errors caused by failed cache exports. |
| `timeout`      | `cache-to`,`cache-from` | String      | `10m`                    | 导入或导出缓存的最大时长，超时将停止操作。 Max duration for importing or exporting cache before it's timed out. |
| `repository`   | `cache-to`              | String      |                          | 用于缓存存储的 GitHub 仓库。 GitHub repository used for cache storage. |
| `ghtoken`      | `cache-to`              | String      |                          | 访问 GitHub API 所需的 GitHub 令牌。 GitHub token required for accessing the GitHub API. |

## 认证 Authentication

If the `url` or `token` parameters are left unspecified, the `gha` cache backend will fall back to using environment variables. If you invoke the `docker buildx` command manually from an inline step, then the variables must be manually exposed. Consider using the [`crazy-max/ghaction-github-runtime`](https://github.com/crazy-max/ghaction-github-runtime), GitHub Action as a helper for exposing the variables.

​	如果未指定 `url` 或 `token` 参数，`gha` 缓存后端将使用环境变量。若手动从内联步骤中调用 `docker buildx` 命令，则需要手动公开这些变量。可考虑使用 [`crazy-max/ghaction-github-runtime`](https://github.com/crazy-max/ghaction-github-runtime) GitHub Action 作为辅助来公开变量。

## 作用域 Scope

Scope is a key used to identify the cache object. By default, it is set to `buildkit`. If you build multiple images, each build will overwrite the cache of the previous, leaving only the final cache.

​	作用域是用于标识缓存对象的键。默认设置为 `buildkit`。若构建多个镜像，每次构建将覆盖之前的缓存，仅保留最终的缓存。

To preserve the cache for multiple builds, you can specify this scope attribute with a specific name. In the following example, the cache is set to the image name, to ensure each image gets its own cache:

​	若要保留多个构建的缓存，可为该作用域属性指定特定名称。在以下示例中，缓存设置为镜像名称，以确保每个镜像都有其自己的缓存：



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=gha,url=...,token=...,scope=image \
  --cache-from type=gha,url=...,token=...,scope=image .
$ docker buildx build --push -t <registry>/<image2> \
  --cache-to type=gha,url=...,token=...,scope=image2 \
  --cache-from type=gha,url=...,token=...,scope=image2 .
```

GitHub's [cache access restrictions](https://docs.github.com/en/actions/advanced-guides/caching-dependencies-to-speed-up-workflows#restrictions-for-accessing-a-cache), still apply. Only the cache for the current branch, the base branch and the default branch is accessible by a workflow.

​	GitHub 的 [缓存访问限制](https://docs.github.com/en/actions/advanced-guides/caching-dependencies-to-speed-up-workflows#restrictions-for-accessing-a-cache) 仍然适用。仅当前分支、基准分支和默认分支的缓存可供工作流访问。

### Using `docker/build-push-action`

When using the [`docker/build-push-action`](https://github.com/docker/build-push-action), the `url` and `token` parameters are automatically populated. No need to manually specify them, or include any additional workarounds.

​	使用 [`docker/build-push-action`](https://github.com/docker/build-push-action) 时，`url` 和 `token` 参数会自动填充，无需手动指定或额外处理。

For example:



```yaml
- name: Build and push
  uses: docker/build-push-action@v6
  with:
    context: .
    push: true
    tags: "<registry>/<image>:latest"
    cache-from: type=gha
    cache-to: type=gha,mode=max
```

## 避免 GitHub Actions 缓存 API 限制 Avoid GitHub Actions cache API throttling

GitHub's [usage limits and eviction policy](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy) causes stale cache entries to be removed after a certain period of time. By default, the `gha` cache backend uses the GitHub Actions cache API to check the status of cache entries.

​	GitHub 的 [使用限制和缓存清除策略](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy) 导致旧缓存条目在一定时间后被移除。默认情况下，`gha` 缓存后端使用 GitHub Actions 缓存 API 检查缓存条目状态。

The GitHub Actions cache API is subject to rate limiting if you make too many requests in a short period of time, which may happen as a result of cache lookups during a build using the `gha` cache backend.

​	GitHub Actions 缓存 API 在短时间内的请求过多时会受限，这在使用 `gha` 缓存后端的构建过程中可能会因频繁缓存查找而发生。

```text
#31 exporting to GitHub Actions Cache
#31 preparing build cache for export
#31 preparing build cache for export 600.3s done
#31 ERROR: maximum timeout reached
------
 > exporting to GitHub Actions Cache:
------
ERROR: failed to solve: maximum timeout reached
make: *** [Makefile:35: release] Error 1
Error: Process completed with exit code 2.
```

To mitigate this issue, you can supply a GitHub token to BuildKit. This lets BuildKit utilize the standard GitHub API for checking cache keys, thereby reducing the number of requests made to the cache API.

​	为解决此问题，可以向 BuildKit 提供 GitHub 令牌，使其使用标准 GitHub API 检查缓存键，从而减少对缓存 API 的请求数量。

To provide a GitHub token, you can use the `ghtoken` parameter, and a `repository` parameter to specify the repository to use for cache storage. The `ghtoken` parameter is a GitHub token with the `repo` scope, which is required to access the GitHub Actions cache API.

​	提供 GitHub 令牌的方法是使用 `ghtoken` 参数，并指定 `repository` 参数来指定用于缓存存储的仓库。`ghtoken` 参数是具有 `repo` 作用域的 GitHub 令牌，用于访问 GitHub Actions 缓存 API。

The `ghtoken` parameter is automatically set to the value of `secrets.GITHUB_TOKEN` when you build with the `docker/build-push-action` action. You can also set the `ghtoken` parameter manually using the `github-token` input, as shown in the following example:

​	在使用 `docker/build-push-action` 构建时，`ghtoken` 参数会自动设置为 `secrets.GITHUB_TOKEN` 的值。您也可以使用 `github-token` 输入参数手动设置 `ghtoken` 参数，如以下示例所示：

```yaml
- name: Build and push
  uses: docker/build-push-action@v6
  with:
    context: .
    push: true
    tags: "<registry>/<image>:latest"
    cache-from: type=gha
    cache-to: type=gha,mode=max
    github-token: ${{ secrets.MY_CUSTOM_TOKEN }}
```

## Further reading

For an introduction to caching see [Docker build cache]({{< ref "/manuals/DockerBuild/Cache" >}}).

​	有关缓存的介绍，请参见 Docker 构建缓存。

For more information on the `gha` cache backend, see the [BuildKit README](https://github.com/moby/buildkit#github-actions-cache-experimental).

​	有关 `gha` 缓存后端的更多信息，请参阅 [BuildKit README](https://github.com/moby/buildkit#github-actions-cache-experimental)。

For more information about using GitHub Actions with Docker, see [Introduction to GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions" >}})

​	有关使用 GitHub Actions 与 Docker 的更多信息，请参见 GitHub Actions 介绍。

