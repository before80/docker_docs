+++
title = "Azure Blob Storage cache"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/build/cache/backends/azblob/](https://docs.docker.com/build/cache/backends/azblob/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Azure Blob Storage cache

**Experimental**

This is an experimental feature. The interface and behavior are unstable and may change in future releases.

The `azblob` cache store uploads your resulting build cache to [Azure's blob storage service](https://azure.microsoft.com/en-us/services/storage/blobs/).

This cache storage backend is not supported with the default `docker` driver. To use this feature, create a new builder using a different driver. See [Build drivers](https://docs.docker.com/build/builders/drivers/) for more information.

## [Synopsis](https://docs.docker.com/build/cache/backends/azblob/#synopsis)



```console
$ docker buildx build --push -t <registry>/<image> \
  --cache-to type=azblob,name=<cache-image>[,parameters...] \
  --cache-from type=azblob,name=<cache-image>[,parameters...] .
```

The following table describes the available CSV parameters that you can pass to `--cache-to` and `--cache-from`.

| Name                | Option                  | Type        | Default | Description                                                  |
| ------------------- | ----------------------- | ----------- | ------- | ------------------------------------------------------------ |
| `name`              | `cache-to`,`cache-from` | String      |         | Required. The name of the cache image.                       |
| `account_url`       | `cache-to`,`cache-from` | String      |         | Base URL of the storage account.                             |
| `secret_access_key` | `cache-to`,`cache-from` | String      |         | Blob storage account key, see [authentication](https://docs.docker.com/build/cache/backends/azblob/#authentication). |
| `mode`              | `cache-to`              | `min`,`max` | `min`   | Cache layers to export, see [cache mode](https://docs.docker.com/build/cache/backends/#cache-mode). |
| `ignore-error`      | `cache-to`              | Boolean     | `false` | Ignore errors caused by failed cache exports.                |

## [Authentication](https://docs.docker.com/build/cache/backends/azblob/#authentication)

The `secret_access_key`, if left unspecified, is read from environment variables on the BuildKit server following the scheme for the [Azure Go SDK](https://docs.microsoft.com/en-us/azure/developer/go/azure-sdk-authentication). The environment variables are read from the server, not the Buildx client.

## [Further reading](https://docs.docker.com/build/cache/backends/azblob/#further-reading)

For an introduction to caching see [Docker build cache](https://docs.docker.com/build/cache/).

For more information on the `azblob` cache backend, see the [BuildKit README](https://github.com/moby/buildkit#azure-blob-storage-cache-experimental).
