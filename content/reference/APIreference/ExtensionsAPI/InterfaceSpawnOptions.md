+++
title = "Interface: SpawnOptions"
date = 2024-10-23T14:54:43+08:00
weight = 240
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/SpawnOptions/](https://docs.docker.com/reference/api/extensions-sdk/SpawnOptions/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: SpawnOptions

**`Since`**

0.3.0

## Hierarchy

- [`ExecOptions`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecOptions" >}})

  ↳ **`SpawnOptions`**

## Properties

### cwd

• `Optional` **cwd**: `string`

#### Inherited from

[ExecOptions]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecOptions" >}}). [cwd](https://docs.docker.com/reference/api/extensions-sdk/ExecOptions/#cwd)

------

### env

• `Optional` **env**: `ProcessEnv`

#### Inherited from

[ExecOptions]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecOptions" >}}). [env](https://docs.docker.com/reference/api/extensions-sdk/ExecOptions/#env)

------

### stream

• **stream**: [`ExecStreamOptions`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecStreamOptions" >}})
