+++
title = "Interface: RawExecResult"
date = 2024-10-23T14:54:43+08:00
weight = 200
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/RawExecResult/](https://docs.docker.com/reference/api/extensions-sdk/RawExecResult/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: RawExecResult

**`Since`**

0.2.0

## Hierarchy

- **`RawExecResult`**

  ↳ [`ExecResult`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecResult" >}})

## Properties

### cmd

• `Optional` `Readonly` **cmd**: `string`

------

### killed

• `Optional` `Readonly` **killed**: `boolean`

------

### signal

• `Optional` `Readonly` **signal**: `string`

------

### code

• `Optional` `Readonly` **code**: `number`

------

### stdout

• `Readonly` **stdout**: `string`

------

### stderr

• `Readonly` **stderr**: `string`
