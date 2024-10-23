+++
title = "Best practices"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/compose/how-tos/environment-variables/best-practices/](https://docs.docker.com/compose/how-tos/environment-variables/best-practices/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Best practices for working with environment variables in Docker Compose

#### [Handle sensitive information securely](https://docs.docker.com/compose/how-tos/environment-variables/best-practices/#handle-sensitive-information-securely)

Be cautious about including sensitive data in environment variables. Consider using [Secrets](https://docs.docker.com/compose/how-tos/use-secrets/) for managing sensitive information.

#### [Understand environment variable precedence](https://docs.docker.com/compose/how-tos/environment-variables/best-practices/#understand-environment-variable-precedence)

Be aware of how Docker Compose handles the [precedence of environment variables](https://docs.docker.com/compose/how-tos/environment-variables/envvars-precedence/) from different sources (`.env` files, shell variables, Dockerfiles).

#### [Use specific environment files](https://docs.docker.com/compose/how-tos/environment-variables/best-practices/#use-specific-environment-files)

Consider how your application adapts to different environments. For example development, testing, production, and use different `.env` files as needed.

#### [Know interpolation](https://docs.docker.com/compose/how-tos/environment-variables/best-practices/#know-interpolation)

Understand how [interpolation](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/) works within compose files for dynamic configurations.

#### [Command line overrides](https://docs.docker.com/compose/how-tos/environment-variables/best-practices/#command-line-overrides)

Be aware that you can [override environment variables](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#cli) from the command line when starting containers. This is useful for testing or when you have temporary changes.
