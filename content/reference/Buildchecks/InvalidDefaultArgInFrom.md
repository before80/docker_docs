+++
title = "InvalidDefaultArgInFrom"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/build-checks/invalid-default-arg-in-from/](https://docs.docker.com/reference/build-checks/invalid-default-arg-in-from/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# InvalidDefaultArgInFrom

## [Output](https://docs.docker.com/reference/build-checks/invalid-default-arg-in-from/#output)



```text
Using the global ARGs with default values should produce a valid build.
```

## [Description](https://docs.docker.com/reference/build-checks/invalid-default-arg-in-from/#description)

An `ARG` used in an image reference should be valid when no build arguments are used. An image build should not require `--build-arg` to be used to produce a valid build.

## [Examples](https://docs.docker.com/reference/build-checks/invalid-default-arg-in-from/#examples)

❌ Bad: don't rely on an ARG being set for an image reference to be valid



```dockerfile
ARG TAG
FROM busybox:${TAG}
```

✅ Good: include a default for the ARG



```dockerfile
ARG TAG=latest
FROM busybox:${TAG}
```

✅ Good: ARG can be empty if the image would be valid with it empty



```dockerfile
ARG VARIANT
FROM busybox:stable${VARIANT}
```

✅ Good: Use a default value if the build arg is not present



```dockerfile
ARG TAG
```
