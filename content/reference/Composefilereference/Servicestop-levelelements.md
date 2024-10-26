+++
title = "顶层元素 Secrets"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/reference/compose-file/services/](https://docs.docker.com/reference/compose-file/services/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Secrets top-level elements - 顶层元素 Secrets

Secrets are a flavor of [Configs]({{< ref "/reference/Composefilereference/Configstop-levelelements" >}}) focusing on sensitive data, with specific constraint for this usage.

​	Secrets 是 [Configs]({{< ref "/reference/Composefilereference/Configstop-levelelements" >}}) 的一种变体，专注于敏感数据，并为此用法设置了特定的约束。

Services can only access secrets when explicitly granted by a [`secrets` attribute](https://docs.docker.com/reference/compose-file/services/#secrets) within the `services` top-level element.

​	服务只能在 `services` 顶层元素中的 [`secrets` 属性](https://docs.docker.com/reference/compose-file/services/#secrets) 明确授予后才能访问 Secrets。

The top-level `secrets` declaration defines or references sensitive data that is granted to the services in your Compose application. The source of the secret is either `file` or `environment`.

​	顶层的 `secrets` 声明定义或引用授予 Compose 应用程序中服务的敏感数据。Secret 的来源可以是 `file` 或 `environment`。

- `file`: The secret is created with the contents of the file at the specified path.
  - `file`: 使用指定路径的文件内容创建 Secret。

- `environment`: The secret is created with the value of an environment variable.
  - `environment`: 使用环境变量的值创建 Secret。


## 示例 1 - Example 1

`server-certificate` secret is created as `<project_name>_server-certificate` when the application is deployed, by registering content of the `server.cert` as a platform secret.

​	`server-certificate` Secret 在应用部署时创建为 `<project_name>_server-certificate`，将 `server.cert` 的内容注册为平台 Secret。

```yml
secrets:
  server-certificate:
    file: ./server.cert
```

## Example 2

`token` secret is created as `<project_name>_token` when the application is deployed, by registering the content of the `OAUTH_TOKEN` environment variable as a platform secret.



```yml
secrets:
  token:
    environment: "OAUTH_TOKEN"
```

## Additional resources

For more information, see [How to use secrets in Compose]({{< ref "/manuals/DockerCompose/How-tos/SecretsinCompose" >}}).
