+++
title = "LegacyKeyValueFormat"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/build-checks/legacy-key-value-format/](https://docs.docker.com/reference/build-checks/legacy-key-value-format/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# LegacyKeyValueFormat

## Output



```text
"ENV key=value" should be used instead of legacy "ENV key value" format
```

## Description

The correct format for declaring environment variables and build arguments in a Dockerfile is `ENV key=value` and `ARG key=value`, where the variable name (`key`) and value (`value`) are separated by an equals sign (`=`). Historically, Dockerfiles have also supported a space separator between the key and the value (for example, `ARG key value`). This legacy format is deprecated, and you should only use the format with the equals sign.

## Examples

❌ Bad: using a space separator for variable key and value.



```dockerfile
FROM alpine
ARG foo bar
```

✅ Good: use an equals sign to separate key and value.



```dockerfile
FROM alpine
ARG foo=bar
```

❌ Bad: multi-line variable declaration with a space separator.



```dockerfile
ENV DEPS \
    curl \
    git \
    make
```

✅ Good: use an equals sign and wrap the value in quotes.



```dockerfile
ENV DEPS="\
    curl \
    git \
    make"
```
