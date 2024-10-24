+++
title = "MaintainerDeprecated"
date = 2024-10-23T14:54:43+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/build-checks/maintainer-deprecated/](https://docs.docker.com/reference/build-checks/maintainer-deprecated/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# MaintainerDeprecated

## [Output](https://docs.docker.com/reference/build-checks/maintainer-deprecated/#output)



```text
MAINTAINER instruction is deprecated in favor of using label
```

## [Description](https://docs.docker.com/reference/build-checks/maintainer-deprecated/#description)

The `MAINTAINER` instruction, used historically for specifying the author of the Dockerfile, is deprecated. To set author metadata for an image, use the `org.opencontainers.image.authors` [OCI label](https://github.com/opencontainers/image-spec/blob/main/annotations.md#pre-defined-annotation-keys).

## [Examples](https://docs.docker.com/reference/build-checks/maintainer-deprecated/#examples)

❌ Bad: don't use the `MAINTAINER` instruction



```dockerfile
MAINTAINER moby@example.com
```

✅ Good: specify the author using the `org.opencontainers.image.authors` label



```dockerfile
LABEL org.opencontainers.image.authors="moby@example.com"
```
