+++
title = "Services top-level elements"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/compose-file/services/](https://docs.docker.com/reference/compose-file/services/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Secrets top-level elements

Secrets are a flavor of [Configs](https://docs.docker.com/reference/compose-file/configs/) focusing on sensitive data, with specific constraint for this usage.

Services can only access secrets when explicitly granted by a [`secrets` attribute](https://docs.docker.com/reference/compose-file/services/#secrets) within the `services` top-level element.

The top-level `secrets` declaration defines or references sensitive data that is granted to the services in your Compose application. The source of the secret is either `file` or `environment`.

- `file`: The secret is created with the contents of the file at the specified path.
- `environment`: The secret is created with the value of an environment variable.

## [Example 1](https://docs.docker.com/reference/compose-file/secrets/#example-1)

`server-certificate` secret is created as `<project_name>_server-certificate` when the application is deployed, by registering content of the `server.cert` as a platform secret.



```yml
secrets:
  server-certificate:
    file: ./server.cert
```

## [Example 2](https://docs.docker.com/reference/compose-file/secrets/#example-2)

`token` secret is created as `<project_name>_token` when the application is deployed, by registering the content of the `OAUTH_TOKEN` environment variable as a platform secret.



```yml
secrets:
  token:
    environment: "OAUTH_TOKEN"
```

## [Additional resources](https://docs.docker.com/reference/compose-file/secrets/#additional-resources)

For more information, see [How to use secrets in Compose](https://docs.docker.com/compose/how-tos/use-secrets/).
