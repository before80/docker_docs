+++
title = "顶层元素版本和名称"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/reference/compose-file/version-and-name/](https://docs.docker.com/reference/compose-file/version-and-name/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Version and name top-level elements - 顶层元素版本和名称

## 顶层元素 version（已废弃） Version top-level element (obsolete)

The top-level `version` property is defined by the Compose Specification for backward compatibility. It is only informative and you'll receive a warning message that it is obsolete if used.

​	Compose 规范中定义了顶层的 `version` 属性，用于向后兼容。它仅具有信息性作用，如果使用该属性，您将收到废弃的警告信息。

Compose doesn't use `version` to select an exact schema to validate the Compose file, but prefers the most recent schema when it's implemented.

​	Compose 不使用 `version` 来选择特定的架构来验证 Compose 文件，而是优先使用已实现的最新架构。

Compose validates whether it can fully parse the Compose file. If some fields are unknown, typically because the Compose file was written with fields defined by a newer version of the Specification, you'll receive a warning message.

​	Compose 会验证是否能够完全解析 Compose 文件。如果有些字段未知，通常是因为 Compose 文件中使用了规范更新版本中定义的字段，您将收到警告信息。

## 顶层元素 name - Name top-level element

The top-level `name` property is defined by the Compose Specification as the project name to be used if you don't set one explicitly. Compose offers a way for you to override this name, and sets a default project name to be used if the top-level `name` element is not set.

​	Compose 规范中定义了顶层的 `name` 属性作为项目名称，如果您未显式设置项目名称，将使用该名称。Compose 提供了一种覆盖该名称的方式，并在未设置顶层 `name` 元素时设定一个默认的项目名称。

Whenever a project name is defined by top-level `name` or by some custom mechanism, it is exposed for [interpolation]({{< ref "/reference/Composefilereference/Interpolation" >}}) and environment variable resolution as `COMPOSE_PROJECT_NAME`

​	当项目名称由顶层 `name` 或自定义机制定义时，该名称将以 `COMPOSE_PROJECT_NAME` 的形式用于[插值]({{< ref "/reference/Composefilereference/Interpolation" >}})和环境变量解析。

```yml
name: myapp

services:
  foo:
    image: busybox
    command: echo "I'm running ${COMPOSE_PROJECT_NAME}"
```

For more information on other ways to name Compose projects, see [Specify a project name]({{< ref "/manuals/DockerCompose/How-tos/Specifyaprojectname" >}}).

​	有关其他命名 Compose 项目方式的更多信息，请参阅[指定项目名称]({{< ref "/manuals/DockerCompose/How-tos/Specifyaprojectname" >}})。
