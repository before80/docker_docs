+++
title = "Best practices"
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

# Best practices for working with environment variables in Docker Compose

#### Handle sensitive information securely

Be cautious about including sensitive data in environment variables. Consider using [Secrets]({{< ref "/manuals/DockerCompose/How-tos/SecretsinCompose" >}}) for managing sensitive information.

#### Understand environment variable precedence

Be aware of how Docker Compose handles the [precedence of environment variables]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Environmentvariablesprecedence" >}}) from different sources (`.env` files, shell variables, Dockerfiles).

#### Use specific environment files

Consider how your application adapts to different environments. For example development, testing, production, and use different `.env` files as needed.

#### Know interpolation

Understand how [interpolation]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Interpolation" >}}) works within compose files for dynamic configurations.

#### Command line overrides

Be aware that you can [override environment variables](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#cli) from the command line when starting containers. This is useful for testing or when you have temporary changes.
