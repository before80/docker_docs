+++
title = "Interface: ExecResultV0"
date = 2024-10-23T14:54:43+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/ExecResultV0/](https://docs.docker.com/reference/api/extensions-sdk/ExecResultV0/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: ExecResultV0

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

## Methods

### lines

▸ **lines**(): `string`[]

Split output lines.

#### Returns

`string`[]

The list of lines.

------

### parseJsonLines

▸ **parseJsonLines**(): `any`[]

Parse each output line as a JSON object.

#### Returns

`any`[]

The list of lines where each line is a JSON object.

------

### parseJsonObject

▸ **parseJsonObject**(): `any`

Parse a well-formed JSON output.

#### Returns

```
any
```

The JSON object.
