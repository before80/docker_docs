+++
title = "FromPlatformFlagConstDisallowed"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/build-checks/from-platform-flag-const-disallowed/](https://docs.docker.com/reference/build-checks/from-platform-flag-const-disallowed/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# FromPlatformFlagConstDisallowed

## [Output](https://docs.docker.com/reference/build-checks/from-platform-flag-const-disallowed/#output)



```text
FROM --platform flag should not use constant value "linux/amd64"
```

## [Description](https://docs.docker.com/reference/build-checks/from-platform-flag-const-disallowed/#description)

Specifying `--platform` in the Dockerfile `FROM` instruction forces the image to build on only one target platform. This prevents building a multi-platform image from this Dockerfile and you must build on the same platform as specified in `--platform`.

The recommended approach is to:

- Omit `FROM --platform` in the Dockerfile and use the `--platform` argument on the command line.
- Use `$BUILDPLATFORM` or some other combination of variables for the `--platform` argument.
- Stage name should include the platform, OS, or architecture name to indicate that it only contains platform-specific instructions.

## [Examples](https://docs.docker.com/reference/build-checks/from-platform-flag-const-disallowed/#examples)

❌ Bad: using a constant argument for `--platform`



```dockerfile
FROM --platform=linux/amd64 alpine AS base
RUN apk add --no-cache git
```

✅ Good: using the default platform



```dockerfile
FROM alpine AS base
RUN apk add --no-cache git
```

✅ Good: using a meta variable



```dockerfile
FROM --platform=${BUILDPLATFORM} alpine AS base
RUN apk add --no-cache git
```

✅ Good: used in a multi-stage build with a target architecture



```dockerfile
FROM --platform=linux/amd64 alpine AS build_amd64
...

FROM --platform=linux/arm64 alpine AS build_arm64
...

FROM build_${TARGETARCH} AS build
...
```
