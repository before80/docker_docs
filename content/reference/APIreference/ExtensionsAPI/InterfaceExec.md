+++
title = "Interface: Exec"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/Exec/](https://docs.docker.com/reference/api/extensions-sdk/Exec/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: Exec

## Callable

### Exec

▸ **Exec**(`cmd`, `args`, `options?`): `Promise`< [`ExecResult`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecResult" >}})>

Executes a command.

**`Since`**

0.2.0

#### Parameters

| Name       | Type                                                         | Description                              |
| :--------- | :----------------------------------------------------------- | :--------------------------------------- |
| `cmd`      | `string`                                                     | The command to execute.                  |
| `args`     | `string`[]                                                   | The arguments of the command to execute. |
| `options?` | [`ExecOptions`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecOptions" >}}) | The list of options.                     |

#### Returns

`Promise`< [`ExecResult`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecResult" >}})>

A promise that will resolve once the command finishes.

### Exec

▸ **Exec**(`cmd`, `args`, `options`): [`ExecProcess`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecProcess" >}})

Streams the result of a command if `stream` is specified in the `options` parameter.

Specify the `stream` if the output of your command is too long or if you need to stream things indefinitely (for example container logs).

**`Since`**

0.2.2

#### Parameters

| Name      | Type                                                         | Description                              |
| :-------- | :----------------------------------------------------------- | :--------------------------------------- |
| `cmd`     | `string`                                                     | The command to execute.                  |
| `args`    | `string`[]                                                   | The arguments of the command to execute. |
| `options` | [`SpawnOptions`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceSpawnOptions" >}}) | The list of options.                     |

#### Returns

[`ExecProcess`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecProcess" >}})

The spawned process.
