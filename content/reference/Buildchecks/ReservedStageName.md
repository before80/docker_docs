+++
title = "ReservedStageName"
date = 2024-10-23T14:54:43+08:00
weight = 120
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/build-checks/reserved-stage-name/](https://docs.docker.com/reference/build-checks/reserved-stage-name/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# ReservedStageName

## [Output](https://docs.docker.com/reference/build-checks/reserved-stage-name/#output)



```text
'scratch' is reserved and should not be used as a stage name
```

## [Description](https://docs.docker.com/reference/build-checks/reserved-stage-name/#description)

Reserved words should not be used as names for stages in multi-stage builds. The reserved words are:

- `context`
- `scratch`

## [Examples](https://docs.docker.com/reference/build-checks/reserved-stage-name/#examples)

❌ Bad: `scratch` and `context` are reserved names.



```dockerfile
FROM alpine AS scratch
FROM alpine AS context
```

✅ Good: the stage name `builder` is not reserved.



```dockerfile
FROM alpine AS builder
```
