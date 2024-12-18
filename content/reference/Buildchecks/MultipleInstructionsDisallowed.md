+++
title = "MultipleInstructionsDisallowed"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/build-checks/multiple-instructions-disallowed/](https://docs.docker.com/reference/build-checks/multiple-instructions-disallowed/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# MultipleInstructionsDisallowed

## Output



```text
Multiple CMD instructions should not be used in the same stage because only the last one will be used
```

## Description

If you have multiple `CMD`, `HEALTHCHECK`, or `ENTRYPOINT` instructions in your Dockerfile, only the last occurrence is used. An image can only ever have one `CMD`, `HEALTHCHECK`, and `ENTRYPOINT`.

## Examples

❌ Bad: Duplicate instructions.



```dockerfile
FROM alpine
ENTRYPOINT ["echo", "Hello, Norway!"]
ENTRYPOINT ["echo", "Hello, Sweden!"]
# Only "Hello, Sweden!" will be printed
```

✅ Good: only one `ENTRYPOINT` instruction.



```dockerfile
FROM alpine
ENTRYPOINT ["echo", "Hello, Norway!\nHello, Sweden!"]
```

You can have both a regular, top-level `CMD` and a separate `CMD` for a `HEALTHCHECK` instruction.

✅ Good: only one top-level `CMD` instruction.



```dockerfile
FROM python:alpine
RUN apk add curl
HEALTHCHECK --interval=1s --timeout=3s \
  CMD ["curl", "-f", "http://localhost:8080"]
CMD ["python", "-m", "http.server", "8080"]
```
