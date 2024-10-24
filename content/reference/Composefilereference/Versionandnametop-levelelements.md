+++
title = "Version and name top-level elements"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/compose-file/version-and-name/](https://docs.docker.com/reference/compose-file/version-and-name/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Version and name top-level elements

## [Version top-level element (obsolete)](https://docs.docker.com/reference/compose-file/version-and-name/#version-top-level-element-obsolete)

The top-level `version` property is defined by the Compose Specification for backward compatibility. It is only informative and you'll receive a warning message that it is obsolete if used.

Compose doesn't use `version` to select an exact schema to validate the Compose file, but prefers the most recent schema when it's implemented.

Compose validates whether it can fully parse the Compose file. If some fields are unknown, typically because the Compose file was written with fields defined by a newer version of the Specification, you'll receive a warning message.

## [Name top-level element](https://docs.docker.com/reference/compose-file/version-and-name/#name-top-level-element)

The top-level `name` property is defined by the Compose Specification as the project name to be used if you don't set one explicitly. Compose offers a way for you to override this name, and sets a default project name to be used if the top-level `name` element is not set.

Whenever a project name is defined by top-level `name` or by some custom mechanism, it is exposed for [interpolation](https://docs.docker.com/reference/compose-file/interpolation/) and environment variable resolution as `COMPOSE_PROJECT_NAME`



```yml
name: myapp

services:
  foo:
    image: busybox
    command: echo "I'm running ${COMPOSE_PROJECT_NAME}"
```

For more information on other ways to name Compose projects, see [Specify a project name](https://docs.docker.com/compose/how-tos/project-name/).
