+++
title = "docker scout cache prune"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/scout/cache/prune/](https://docs.docker.com/reference/cli/docker/scout/cache/prune/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker scout cache prune

| Description | Remove temporary or cached data |
| :---------- | ------------------------------- |
| Usage       | `docker scout cache prune`      |

## Description

The `docker scout cache prune` command removes temporary data and SBOM cache.

By default, `docker scout cache prune` only deletes temporary data. To delete temporary data and clear the SBOM cache, use the `--sboms` flag.

## Options

| Option        | Default | Description                    |
| ------------- | ------- | ------------------------------ |
| `-f, --force` |         | Do not prompt for confirmation |
| `--sboms`     |         | Prune cached SBOMs             |

## Examples

### Delete temporary data



```console
$ docker scout cache prune
? Are you sure to delete all temporary data? Yes
    ✓ temporary data deleted
```

### Delete temporary *and* cache data



```console
$ docker scout cache prune --sboms
? Are you sure to delete all temporary data and all cached SBOMs? Yes
    ✓ temporary data deleted
    ✓ cached SBOMs deleted
```
