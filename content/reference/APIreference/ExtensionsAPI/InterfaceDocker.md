+++
title = "Interface: Docker"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/Docker/](https://docs.docker.com/reference/api/extensions-sdk/Docker/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: Docker

**`Since`**

0.2.0

## Properties

### cli

• `Readonly` **cli**: [`DockerCommand`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceDockerCommand" >}})

You can also directly execute the Docker binary.



```typescript
const output = await ddClient.docker.cli.exec("volume", [
  "ls",
  "--filter",
  "dangling=true"
]);
```

Output:



```json
{
  "stderr": "...",
  "stdout": "..."
}
```

For convenience, the command result object also has methods to easily parse it depending on output format. See [ExecResult]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExecResult" >}}) instead.

------

Streams the output as a result of the execution of a Docker command. It is useful when the output of the command is too long, or you need to get the output as a stream.



```typescript
await ddClient.docker.cli.exec("logs", ["-f", "..."], {
  stream: {
    onOutput(data): void {
        // As we can receive both `stdout` and `stderr`, we wrap them in a JSON object
        JSON.stringify(
          {
            stdout: data.stdout,
            stderr: data.stderr,
          },
          null,
          "  "
        );
    },
    onError(error: any): void {
      console.error(error);
    },
    onClose(exitCode: number): void {
      console.log("onClose with exit code " + exitCode);
    },
  },
});
```

## Methods

### listContainers

▸ **listContainers**(`options?`): `Promise`<`unknown`>

Get the list of running containers (same as `docker ps`).

By default, this will not list stopped containers. You can use the option `{"all": true}` to list all the running and stopped containers.



```typescript
const containers = await ddClient.docker.listContainers();
```

#### Parameters

| Name       | Type  | Description                                                  |
| :--------- | :---- | :----------------------------------------------------------- |
| `options?` | `any` | (Optional). A JSON like `{ "all": true, "limit": 10, "size": true, "filters": JSON.stringify({ status: ["exited"] }), }` For more information about the different properties see [the Docker API endpoint documentation](https://docs.docker.com/engine/api/v1.41/#operation/ContainerList). |

#### Returns

`Promise`<`unknown`>

------

### listImages

▸ **listImages**(`options?`): `Promise`<`unknown`>

Get the list of local container images



```typescript
const images = await ddClient.docker.listImages();
```

#### Parameters

| Name       | Type  | Description                                                  |
| :--------- | :---- | :----------------------------------------------------------- |
| `options?` | `any` | (Optional). A JSON like `{ "all": true, "filters": JSON.stringify({ dangling: ["true"] }), "digests": true * }` For more information about the different properties see [the Docker API endpoint documentation](https://docs.docker.com/engine/api/v1.41/#tag/Image). |

#### Returns

`Promise`<`unknown`>
