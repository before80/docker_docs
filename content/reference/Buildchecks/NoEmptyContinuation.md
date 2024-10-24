+++
title = "NoEmptyContinuation"
date = 2024-10-23T14:54:43+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/build-checks/no-empty-continuation/](https://docs.docker.com/reference/build-checks/no-empty-continuation/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# NoEmptyContinuation

## Output



```text
Empty continuation line found in: RUN apk add     gnupg     curl
```

## Description

Support for empty continuation (`/`) lines have been deprecated and will generate errors in future versions of the Dockerfile syntax.

Empty continuation lines are empty lines following a newline escape:



```dockerfile
FROM alpine
RUN apk add \

    gnupg \

    curl
```

Support for such empty lines is deprecated, and a future BuildKit release will remove support for this syntax entirely, causing builds to break. To avoid future errors, remove the empty lines, or add comments, since lines with comments aren't considered empty.

## Examples

❌ Bad: empty continuation line between `EXPOSE` and 80.



```dockerfile
FROM alpine
EXPOSE \

80
```

✅ Good: comments do not count as empty lines.



```dockerfile
FROM alpine
EXPOSE \
# Port
80
```
