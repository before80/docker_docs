+++
title = "Amazon S3 缓存"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/backends/s3/](https://docs.docker.com/build/cache/backends/s3/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Amazon S3 cache - Amazon S3 缓存

**受限功能 Restricted**

This is an experimental feature. The interface and behavior are unstable and may change in future releases.

​	此功能为实验性功能，接口和行为不稳定，可能会在未来版本中发生变化。

The `s3` cache storage uploads your resulting build cache to [Amazon S3 file storage service](https://aws.amazon.com/s3/) or other S3-compatible services, such as [MinIO](https://min.io/).

​	`s3` 缓存存储将您的构建缓存上传到 [Amazon S3 文件存储服务](https://aws.amazon.com/s3/)或其他兼容 S3 的服务，例如 [MinIO](https://min.io/)。

This cache storage backend is not supported with the default `docker` driver. To use this feature, create a new builder using a different driver. See [Build drivers]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}) for more information.

​	默认的 `docker` 驱动不支持此缓存存储后端。要使用此功能，请使用不同的驱动创建一个新的构建器。更多信息参见 构建驱动。

## 概要 Synopsis



```console
$ docker buildx build --push -t <user>/<image> \
  --cache-to type=s3,region=<region>,bucket=<bucket>,name=<cache-image>[,parameters...] \
  --cache-from type=s3,region=<region>,bucket=<bucket>,name=<cache-image> .
```

The following table describes the available CSV parameters that you can pass to `--cache-to` and `--cache-from`.

​	下表描述了可用于 `--cache-to` 和 `--cache-from` 的 CSV 参数：

| Name                 | Option                  | Type        | Default | Description                                                  |
| -------------------- | ----------------------- | ----------- | ------- | ------------------------------------------------------------ |
| `region`             | `cache-to`,`cache-from` | String      |         | 必填。地理位置。 Required. Geographic location.              |
| `bucket`             | `cache-to`,`cache-from` | String      |         | 必填。S3 存储桶名称。 Required. Name of the S3 bucket.       |
| `name`               | `cache-to`,`cache-from` | String      |         | 缓存镜像名称。 Name of the cache image.                      |
| `endpoint_url`       | `cache-to`,`cache-from` | String      |         | S3 存储桶的端点。 Endpoint of the S3 bucket.                 |
| `blobs_prefix`       | `cache-to`,`cache-from` | String      |         | 添加到 blob 文件名前的前缀。 Prefix to prepend to blob filenames. |
| `upload_parallelism` | `cache-to`              | Integer     | `4`     | 并行层上传的数量。 Number of parallel layer uploads.         |
| `touch_refresh`      | `cache-to`              | Time        | `24h`   | 更新未更改缓存层的时间戳的间隔。 Interval for updating the timestamp of unchanged cache layers. |
| `manifests_prefix`   | `cache-to`,`cache-from` | String      |         | 添加到清单文件名前的前缀。 Prefix to prepend on manifest filenames. |
| `use_path_style`     | `cache-to`,`cache-from` | Boolean     | `false` | 当为 `true` 时，在 URL 中使用 `bucket` 而不是主机名。 When `true`, uses `bucket` in the URL instead of hostname. |
| `access_key_id`      | `cache-to`,`cache-from` | String      |         | 参见 [认证](https://docs.docker.com/build/cache/backends/s3/#authentication)。 See [authentication](https://docs.docker.com/build/cache/backends/s3/#authentication). |
| `secret_access_key`  | `cache-to`,`cache-from` | String      |         | 参见 [认证](https://docs.docker.com/build/cache/backends/s3/#authentication)。 See [authentication](https://docs.docker.com/build/cache/backends/s3/#authentication). |
| `session_token`      | `cache-to`,`cache-from` | String      |         | 参见 [认证](https://docs.docker.com/build/cache/backends/s3/#authentication)。 See [authentication](https://docs.docker.com/build/cache/backends/s3/#authentication). |
| `mode`               | `cache-to`              | `min`,`max` | `min`   | 要导出的缓存层，参见 [缓存模式](https://docs.docker.com/build/cache/backends/#cache-mode)。Cache layers to export, see [cache mode](https://docs.docker.com/build/cache/backends/#cache-mode). |
| `ignore-error`       | `cache-to`              | Boolean     | `false` | 忽略缓存导出失败导致的错误。 Ignore errors caused by failed cache exports. |

## 认证 Authentication

Buildx can reuse existing AWS credentials, configured either using a credentials file or environment variables, for pushing and pulling cache to S3. Alternatively, you can use the `access_key_id`, `secret_access_key`, and `session_token` attributes to specify credentials directly on the CLI.

​	Buildx 可以重复使用现有的 AWS 凭据，这些凭据可以通过凭据文件或环境变量进行配置，以便向 S3 推送和拉取缓存。或者，您可以直接在 CLI 上使用 `access_key_id`、`secret_access_key` 和 `session_token` 属性来指定凭据。

Refer to [AWS Go SDK, Specifying Credentials](https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/configuring-sdk.html#specifying-credentials) for details about authentication using environment variables and credentials file.

​	有关使用环境变量和凭据文件进行身份验证的详细信息，请参考 [AWS Go SDK，指定凭据](https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/configuring-sdk.html#specifying-credentials)。

## Further reading

For an introduction to caching see [Docker build cache]({{< ref "/manuals/DockerBuild/Cache" >}}).

​	有关缓存的介绍，请参见 Docker 构建缓存。

For more information on the `s3` cache backend, see the [BuildKit README](https://github.com/moby/buildkit#s3-cache-experimental).

​	有关 `s3` 缓存后端的更多信息，请参阅 [BuildKit README](https://github.com/moby/buildkit#s3-cache-experimental)。
