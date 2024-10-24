+++
title = "DuplicateStageName"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/build-checks/duplicate-stage-name/](https://docs.docker.com/reference/build-checks/duplicate-stage-name/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# DuplicateStageName

## [Output](https://docs.docker.com/reference/build-checks/duplicate-stage-name/#output)



```text
Duplicate stage name 'foo-base', stage names should be unique
```

## [Description](https://docs.docker.com/reference/build-checks/duplicate-stage-name/#description)

Defining multiple stages with the same name results in an error because the builder is unable to uniquely resolve the stage name reference.

## [Examples](https://docs.docker.com/reference/build-checks/duplicate-stage-name/#examples)

❌ Bad: `builder` is declared as a stage name twice.



```dockerfile
FROM debian:latest AS builder
RUN apt-get update; apt-get install -y curl

FROM golang:latest AS builder
```

✅ Good: stages have unique names.



```dockerfile
FROM debian:latest AS deb-builder
RUN apt-get update; apt-get install -y curl

FROM golang:latest AS go-builder
```
