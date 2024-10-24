+++
title = "Interface: Dialog"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/Dialog/](https://docs.docker.com/reference/api/extensions-sdk/Dialog/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: Dialog

Allows opening native dialog boxes.

**`Since`**

0.2.3

## Methods

### showOpenDialog

▸ **showOpenDialog**(`dialogProperties`): `Promise`< [`OpenDialogResult`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceOpenDialogResult" >}})>

Display a native open dialog. Lets you select a file or a folder.



```typescript
ddClient.desktopUI.dialog.showOpenDialog({properties: ['openFile']});
```

#### Parameters

| Name               | Type  | Description                                                  |
| :----------------- | :---- | :----------------------------------------------------------- |
| `dialogProperties` | `any` | Properties to specify the open dialog behaviour, see https://www.electronjs.org/docs/latest/api/dialog#dialogshowopendialogbrowserwindow-options. |

#### Returns

`Promise`< [`OpenDialogResult`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceOpenDialogResult" >}})>
