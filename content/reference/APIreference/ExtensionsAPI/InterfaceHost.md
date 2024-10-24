+++
title = "Interface: Host"
date = 2024-10-23T14:54:43+08:00
weight = 160
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/Host/](https://docs.docker.com/reference/api/extensions-sdk/Host/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: Host

**`Since`**

0.2.0

## Methods

### openExternal

▸ **openExternal**(`url`): `void`

Opens an external URL with the system default browser.

**`Since`**

0.2.0



```typescript
ddClient.host.openExternal("https://docker.com");
```

#### Parameters

| Name  | Type     | Description                                                  |
| :---- | :------- | :----------------------------------------------------------- |
| `url` | `string` | The URL the browser will open (must have the protocol `http` or `https`). |

#### Returns

```
void
```

## Properties

### platform

• **platform**: `string`

Returns a string identifying the operating system platform. See https://nodejs.org/api/os.html#osplatform

**`Since`**

0.2.2

------

### arch

• **arch**: `string`

Returns the operating system CPU architecture. See https://nodejs.org/api/os.html#osarch

**`Since`**

0.2.2

------

### hostname

• **hostname**: `string`

Returns the host name of the operating system. See https://nodejs.org/api/os.html#oshostname

**`Since`**

0.2.2
