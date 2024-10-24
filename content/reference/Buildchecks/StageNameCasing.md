+++
title = "StageNameCasing"
date = 2024-10-23T14:54:43+08:00
weight = 140
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/build-checks/stage-name-casing/](https://docs.docker.com/reference/build-checks/stage-name-casing/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# StageNameCasing

## Output



```text
Stage name 'BuilderBase' should be lowercase
```

## Description

To help distinguish Dockerfile instruction keywords from identifiers, this rule forces names of stages in a multi-stage Dockerfile to be all lowercase.

## Examples

❌ Bad: mixing uppercase and lowercase characters in the stage name.



```dockerfile
FROM alpine AS BuilderBase
```

✅ Good: stage name is all in lowercase.



```dockerfile
FROM alpine AS builder-base
```
