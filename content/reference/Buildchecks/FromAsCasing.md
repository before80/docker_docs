+++
title = "FromAsCasing"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/build-checks/from-as-casing/](https://docs.docker.com/reference/build-checks/from-as-casing/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# FromAsCasing

## [Output](https://docs.docker.com/reference/build-checks/from-as-casing/#output)



```text
'as' and 'FROM' keywords' casing do not match
```

## [Description](https://docs.docker.com/reference/build-checks/from-as-casing/#description)

While Dockerfile keywords can be either uppercase or lowercase, mixing case styles is not recommended for readability. This rule reports violations where mixed case style occurs for a `FROM` instruction with an `AS` keyword declaring a stage name.

## [Examples](https://docs.docker.com/reference/build-checks/from-as-casing/#examples)

❌ Bad: `FROM` is uppercase, `AS` is lowercase.



```dockerfile
FROM debian:latest as builder
```

✅ Good: `FROM` and `AS` are both uppercase



```dockerfile
FROM debian:latest AS deb-builder
```

✅ Good: `FROM` and `AS` are both lowercase.



```dockerfile
from debian:latest as deb-builder
```

## [Related errors](https://docs.docker.com/reference/build-checks/from-as-casing/#related-errors)

- [`FileConsistentCommandCasing`](https://docs.docker.com/reference/build-checks/consistent-instruction-casing/)
