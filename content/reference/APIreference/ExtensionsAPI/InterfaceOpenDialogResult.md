+++
title = "Interface: OpenDialogResult"
date = 2024-10-23T14:54:43+08:00
weight = 190
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/OpenDialogResult/](https://docs.docker.com/reference/api/extensions-sdk/OpenDialogResult/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: OpenDialogResult

**`Since`**

0.2.3

## Properties

### canceled

• `Readonly` **canceled**: `boolean`

Whether the dialog was canceled.

------

### filePaths

• `Readonly` **filePaths**: `string`[]

An array of file paths chosen by the user. If the dialog is cancelled this will be an empty array.

------

### bookmarks

• `Optional` `Readonly` **bookmarks**: `string`[]

macOS only. An array matching the `filePaths` array of `base64` encoded strings which contains security scoped bookmark data. `securityScopedBookmarks` must be enabled for this to be populated.
