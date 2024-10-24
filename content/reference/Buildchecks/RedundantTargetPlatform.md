+++
title = "RedundantTargetPlatform"
date = 2024-10-23T14:54:43+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/build-checks/redundant-target-platform/](https://docs.docker.com/reference/build-checks/redundant-target-platform/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# RedundantTargetPlatform

## [Output](https://docs.docker.com/reference/build-checks/redundant-target-platform/#output)



```text
Setting platform to predefined $TARGETPLATFORM in FROM is redundant as this is the default behavior
```

## [Description](https://docs.docker.com/reference/build-checks/redundant-target-platform/#description)

A custom platform can be used for a base image. The default platform is the same platform as the target output so setting the platform to `$TARGETPLATFORM` is redundant and unnecessary.

## [Examples](https://docs.docker.com/reference/build-checks/redundant-target-platform/#examples)

❌ Bad: this usage of `--platform` is redundant since `$TARGETPLATFORM` is the default.



```dockerfile
FROM --platform=$TARGETPLATFORM alpine AS builder
RUN apk add --no-cache git
```

✅ Good: omit the `--platform` argument.



```dockerfile
FROM alpine AS builder
RUN apk add --no-cache git
```
