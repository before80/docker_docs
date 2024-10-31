+++
title = "最佳实践"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/environment-variables/best-practices/](https://docs.docker.com/compose/how-tos/environment-variables/best-practices/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Best practices for working with environment variables in Docker Compose - Docker Compose 中使用环境变量的最佳实践

#### 安全地处理敏感信息 Handle sensitive information securely

Be cautious about including sensitive data in environment variables. Consider using [Secrets]({{< ref "/manuals/DockerCompose/How-tos/SecretsinCompose" >}}) for managing sensitive information.

​	对于环境变量中包含的敏感数据要小心。建议使用 [Secrets]({{< ref "/manuals/DockerCompose/How-tos/SecretsinCompose" >}}) 来管理敏感信息。

#### 了解环境变量优先级 Understand environment variable precedence

Be aware of how Docker Compose handles the [precedence of environment variables]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Environmentvariablesprecedence" >}}) from different sources (`.env` files, shell variables, Dockerfiles).

​	请了解 Docker Compose 如何处理来自不同来源的[环境变量优先级]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Environmentvariablesprecedence" >}})（如 `.env` 文件、Shell 变量、Dockerfile 等）。

#### 使用特定的环境文件 Use specific environment files

Consider how your application adapts to different environments. For example development, testing, production, and use different `.env` files as needed.

​	根据应用程序在不同环境中的适应性，例如开发、测试、生产，使用不同的 `.env` 文件。

#### 熟悉插值（Interpolation） Know interpolation

Understand how [interpolation]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Interpolation" >}}) works within compose files for dynamic configurations.

​	了解 Compose 文件中 [插值]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Interpolation" >}}) 的工作原理，以便动态配置。

#### 命令行覆盖 Command line overrides

Be aware that you can [override environment variables](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#cli) from the command line when starting containers. This is useful for testing or when you have temporary changes.

​	注意可以在启动容器时通过命令行 [覆盖环境变量](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#cli)，这对测试或临时更改非常有用。

