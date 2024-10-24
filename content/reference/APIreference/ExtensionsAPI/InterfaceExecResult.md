+++
title = "Interface: ExecResult"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/ExecResult/](https://docs.docker.com/reference/api/extensions-sdk/ExecResult/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: ExecResult

**`Since`**

0.2.0

## Hierarchy

- [`RawExecResult`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceRawExecResult" >}})

  ↳ **`ExecResult`**

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

## Properties

### cmd

• `Optional` `Readonly` **cmd**: `string`

#### Inherited from

[RawExecResult]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceRawExecResult" >}}). [cmd](https://docs.docker.com/reference/api/extensions-sdk/RawExecResult/#cmd)

------

### killed

• `Optional` `Readonly` **killed**: `boolean`

#### Inherited from

[RawExecResult]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceRawExecResult" >}}). [killed](https://docs.docker.com/reference/api/extensions-sdk/RawExecResult/#killed)

------

### signal

• `Optional` `Readonly` **signal**: `string`

#### Inherited from

[RawExecResult]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceRawExecResult" >}}). [signal](https://docs.docker.com/reference/api/extensions-sdk/RawExecResult/#signal)

------

### code

• `Optional` `Readonly` **code**: `number`

#### Inherited from

[RawExecResult]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceRawExecResult" >}}). [code](https://docs.docker.com/reference/api/extensions-sdk/RawExecResult/#code)

------

### stdout

• `Readonly` **stdout**: `string`

#### Inherited from

[RawExecResult]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceRawExecResult" >}}). [stdout](https://docs.docker.com/reference/api/extensions-sdk/RawExecResult/#stdout)

------

### stderr

• `Readonly` **stderr**: `string`

#### Inherited from

[RawExecResult]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceRawExecResult" >}}). [stderr](https://docs.docker.com/reference/api/extensions-sdk/RawExecResult/#stderr)
