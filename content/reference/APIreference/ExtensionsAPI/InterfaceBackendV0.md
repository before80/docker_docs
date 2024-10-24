+++
title = "Interface: BackendV0"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/BackendV0/](https://docs.docker.com/reference/api/extensions-sdk/BackendV0/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: BackendV0

## Container Methods

### execInContainer

▸ **execInContainer**(`container`, `cmd`): `Promise`< [`ExecResultV0`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecResultV0" >}})>

Executes a command inside a container.



```typescript
const output = await window.ddClient.backend.execInContainer(container, cmd);

console.log(output);
```

> **Warning**
>
> 
>
> It will be removed in a future version.

#### Parameters

| Name        | Type     | Description                 |
| :---------- | :------- | :-------------------------- |
| `container` | `string` | -                           |
| `cmd`       | `string` | The command to be executed. |

#### Returns

`Promise`< [`ExecResultV0`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecResultV0" >}})>

------

## HTTP Methods

### get

▸ **get**(`url`): `Promise`<`unknown`>

Performs an HTTP GET request to a backend service.



```typescript
window.ddClient.backend
 .get("/some/service")
 .then((value: any) => console.log(value));
```

> **Warning**
>
> 
>
> It will be removed in a future version. Use [get](https://docs.docker.com/reference/api/extensions-sdk/HttpService/#get) instead.

#### Parameters

| Name  | Type     | Description                     |
| :---- | :------- | :------------------------------ |
| `url` | `string` | The URL of the backend service. |

#### Returns

`Promise`<`unknown`>

------

### post

▸ **post**(`url`, `data`): `Promise`<`unknown`>

Performs an HTTP POST request to a backend service.



```typescript
window.ddClient.backend
 .post("/some/service", { ... })
 .then((value: any) => console.log(value));
```

> **Warning**
>
> 
>
> It will be removed in a future version. Use [post](https://docs.docker.com/reference/api/extensions-sdk/HttpService/#post) instead.

#### Parameters

| Name   | Type     | Description                     |
| :----- | :------- | :------------------------------ |
| `url`  | `string` | The URL of the backend service. |
| `data` | `any`    | The body of the request.        |

#### Returns

`Promise`<`unknown`>

------

### put

▸ **put**(`url`, `data`): `Promise`<`unknown`>

Performs an HTTP PUT request to a backend service.



```typescript
window.ddClient.backend
 .put("/some/service", { ... })
 .then((value: any) => console.log(value));
```

> **Warning**
>
> 
>
> It will be removed in a future version. Use [put](https://docs.docker.com/reference/api/extensions-sdk/HttpService/#put) instead.

#### Parameters

| Name   | Type     | Description                     |
| :----- | :------- | :------------------------------ |
| `url`  | `string` | The URL of the backend service. |
| `data` | `any`    | The body of the request.        |

#### Returns

`Promise`<`unknown`>

------

### patch

▸ **patch**(`url`, `data`): `Promise`<`unknown`>

Performs an HTTP PATCH request to a backend service.



```typescript
window.ddClient.backend
 .patch("/some/service", { ... })
 .then((value: any) => console.log(value));
```

> **Warning**
>
> 
>
> It will be removed in a future version. Use [patch](https://docs.docker.com/reference/api/extensions-sdk/HttpService/#patch) instead.

#### Parameters

| Name   | Type     | Description                     |
| :----- | :------- | :------------------------------ |
| `url`  | `string` | The URL of the backend service. |
| `data` | `any`    | The body of the request.        |

#### Returns

`Promise`<`unknown`>

------

### delete

▸ **delete**(`url`): `Promise`<`unknown`>

Performs an HTTP DELETE request to a backend service.



```typescript
window.ddClient.backend
 .delete("/some/service")
 .then((value: any) => console.log(value));
```

> **Warning**
>
> 
>
> It will be removed in a future version. Use [delete](https://docs.docker.com/reference/api/extensions-sdk/HttpService/#delete) instead.

#### Parameters

| Name  | Type     | Description                     |
| :---- | :------- | :------------------------------ |
| `url` | `string` | The URL of the backend service. |

#### Returns

`Promise`<`unknown`>

------

### head

▸ **head**(`url`): `Promise`<`unknown`>

Performs an HTTP HEAD request to a backend service.



```typescript
window.ddClient.backend
 .head("/some/service")
 .then((value: any) => console.log(value));
```

> **Warning**
>
> 
>
> It will be removed in a future version. Use [head](https://docs.docker.com/reference/api/extensions-sdk/HttpService/#head) instead.

#### Parameters

| Name  | Type     | Description                     |
| :---- | :------- | :------------------------------ |
| `url` | `string` | The URL of the backend service. |

#### Returns

`Promise`<`unknown`>

------

### request

▸ **request**(`config`): `Promise`<`unknown`>

Performs an HTTP request to a backend service.



```typescript
window.ddClient.backend
 .request({ url: "/url", method: "GET", headers: { 'header-key': 'header-value' }, data: { ... }})
 .then((value: any) => console.log(value));
```

> **Warning**
>
> 
>
> It will be removed in a future version. Use [request](https://docs.docker.com/reference/api/extensions-sdk/HttpService/#request) instead.

#### Parameters

| Name     | Type                                                         | Description                     |
| :------- | :----------------------------------------------------------- | :------------------------------ |
| `config` | [`RequestConfigV0`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceRequestConfigV0" >}}) | The URL of the backend service. |

#### Returns

`Promise`<`unknown`>

------

## VM Methods

### execInVMExtension

▸ **execInVMExtension**(`cmd`): `Promise`< [`ExecResultV0`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecResultV0" >}})>

Executes a command inside the backend container. If your extensions ships with additional binaries that should be run inside the backend container you can use the `execInVMExtension` function.



```typescript
const output = await window.ddClient.backend.execInVMExtension(
  `cliShippedInTheVm xxx`
);

console.log(output);
```

> **Warning**
>
> 
>
> It will be removed in a future version. Use [exec](https://docs.docker.com/reference/api/extensions-sdk/ExtensionCli/#exec) instead.

#### Parameters

| Name  | Type     | Description                 |
| :---- | :------- | :-------------------------- |
| `cmd` | `string` | The command to be executed. |

#### Returns

`Promise`< [`ExecResultV0`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecResultV0" >}})>

------

### spawnInVMExtension

▸ **spawnInVMExtension**(`cmd`, `args`, `callback`): `void`

Returns a stream from the command executed in the backend container.



```typescript
window.ddClient.spawnInVMExtension(
  `cmd`,
  [`arg1`, `arg2`],
  (data: any, err: any) => {
    console.log(data.stdout, data.stderr);
    // Once the command exits we get the status code
    if (data.code) {
      console.log(data.code);
    }
  }
);
```

> **Warning**
>
> 
>
> It will be removed in a future version.

#### Parameters

| Name       | Type                                      | Description                                                  |
| :--------- | :---------------------------------------- | :----------------------------------------------------------- |
| `cmd`      | `string`                                  | The command to be executed.                                  |
| `args`     | `string`[]                                | The arguments of the command to execute.                     |
| `callback` | (`data`: `any`, `error`: `any`) => `void` | The callback function where to listen from the command output data and errors. |

#### Returns

```
void
```
