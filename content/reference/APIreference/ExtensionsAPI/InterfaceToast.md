+++
title = "Interface: Toast"
date = 2024-10-23T14:54:43+08:00
weight = 250
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/Toast/](https://docs.docker.com/reference/api/extensions-sdk/Toast/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: Toast

Toasts provide a brief notification to the user. They appear temporarily and shouldn't interrupt the user experience. They also don't require user input to disappear.

**`Since`**

0.2.0

## Methods

### success

▸ **success**(`msg`): `void`

Display a toast message of type success.



```typescript
ddClient.desktopUI.toast.success("message");
```

#### Parameters

| Name  | Type     | Description                          |
| :---- | :------- | :----------------------------------- |
| `msg` | `string` | The message to display in the toast. |

#### Returns

```
void
```

------

### warning

▸ **warning**(`msg`): `void`

Display a toast message of type warning.



```typescript
ddClient.desktopUI.toast.warning("message");
```

#### Parameters

| Name  | Type     | Description                            |
| :---- | :------- | :------------------------------------- |
| `msg` | `string` | The message to display in the warning. |

#### Returns

```
void
```

------

### error

▸ **error**(`msg`): `void`

Display a toast message of type error.



```typescript
ddClient.desktopUI.toast.error("message");
```

#### Parameters

| Name  | Type     | Description                          |
| :---- | :------- | :----------------------------------- |
| `msg` | `string` | The message to display in the toast. |

#### Returns

```
void
```
