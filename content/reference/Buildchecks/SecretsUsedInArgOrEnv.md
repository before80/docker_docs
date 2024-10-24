+++
title = "SecretsUsedInArgOrEnv"
date = 2024-10-23T14:54:43+08:00
weight = 130
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/build-checks/secrets-used-in-arg-or-env/](https://docs.docker.com/reference/build-checks/secrets-used-in-arg-or-env/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# SecretsUsedInArgOrEnv

## Output



```text
Potentially sensitive data should not be used in the ARG or ENV commands
```

## Description

While it is common to pass secrets to running processes through environment variables during local development, setting secrets in a Dockerfile using `ENV` or `ARG` is insecure because they persist in the final image. This rule reports violations where `ENV` and `ARG` keys indicate that they contain sensitive data.

Instead of `ARG` or `ENV`, you should use secret mounts, which expose secrets to your builds in a secure manner, and do not persist in the final image or its metadata. See [Build secrets]({{< ref "/manuals/DockerBuild/Building/Secrets" >}}).

## Examples

❌ Bad: `AWS_SECRET_ACCESS_KEY` is a secret value.



```dockerfile
FROM scratch
ARG AWS_SECRET_ACCESS_KEY
```
