+++
title = "Azure Blob Storage 缓存"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/backends/azblob/](https://docs.docker.com/build/cache/backends/azblob/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Azure Blob Storage cache - Azure Blob Storage 缓存

**实验性 Experimental**

This is an experimental feature. The interface and behavior are unstable and may change in future releases.

​	此功能为实验性功能，接口和行为不稳定，可能会在未来版本中发生变化。

The `azblob` cache store uploads your resulting build cache to [Azure's blob storage service](https://azure.microsoft.com/en-us/services/storage/blobs/).

​	`azblob` 缓存存储将您的构建缓存上传到 [Azure 的 Blob 存储服务](https://azure.microsoft.com/en-us/services/storage/blobs/)。

This cache storage backend is not supported with the default `docker` driver. To use this feature, create a new builder using a different driver. See [Build drivers]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}) for more information.

​	默认的 `docker` 驱动不支持此缓存存储后端。要使用此功能，请使用不同的驱动创建一个新的构建器。更多信息参见 构建驱动。

## 概要 Synopsis



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=azblob,name=<cache-image>[,parameters...] \
  --cache-from type=azblob,name=<cache-image>[,parameters...] .
```

The following table describes the available CSV parameters that you can pass to `--cache-to` and `--cache-from`.

​	下表描述了可用于 `--cache-to` 和 `--cache-from` 的 CSV 参数：

| Name                | Option                  | Type        | Default | Description                                                  |
| ------------------- | ----------------------- | ----------- | ------- | ------------------------------------------------------------ |
| `name`              | `cache-to`,`cache-from` | String      |         | 必填。缓存镜像的名称。 Required. The name of the cache image. |
| `account_url`       | `cache-to`,`cache-from` | String      |         | 存储账户的基础 URL。 Base URL of the storage account.        |
| `secret_access_key` | `cache-to`,`cache-from` | String      |         | Blob 存储账户密钥，参见 [认证](https://docs.docker.com/build/cache/backends/azblob/#authentication)。 Blob storage account key, see [authentication](https://docs.docker.com/build/cache/backends/azblob/#authentication). |
| `mode`              | `cache-to`              | `min`,`max` | `min`   | 要导出的缓存层，参见 [缓存模式](https://docs.docker.com/build/cache/backends/#cache-mode)。 Cache layers to export, see [cache mode](https://docs.docker.com/build/cache/backends/#cache-mode). |
| `ignore-error`      | `cache-to`              | Boolean     | `false` | 忽略缓存导出失败导致的错误。 Ignore errors caused by failed cache exports. |

## 认证 Authentication

The `secret_access_key`, if left unspecified, is read from environment variables on the BuildKit server following the scheme for the [Azure Go SDK](https://docs.microsoft.com/en-us/azure/developer/go/azure-sdk-authentication). The environment variables are read from the server, not the Buildx client.

​	如果未指定 `secret_access_key`，则会从 BuildKit 服务器上的环境变量中读取，遵循 [Azure Go SDK](https://docs.microsoft.com/en-us/azure/developer/go/azure-sdk-authentication) 的方案。环境变量是从服务器而非 Buildx 客户端读取的。

## Further reading

For an introduction to caching see [Docker build cache]({{< ref "/manuals/DockerBuild/Cache" >}}).

​	有关缓存的介绍，请参见 Docker 构建缓存。

For more information on the `azblob` cache backend, see the [BuildKit README](https://github.com/moby/buildkit#azure-blob-storage-cache-experimental).

​	有关 `azblob` 缓存后端的更多信息，请参阅 [BuildKit README](https://github.com/moby/buildkit#azure-blob-storage-cache-experimental)。
