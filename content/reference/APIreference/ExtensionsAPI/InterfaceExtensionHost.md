+++
title = "Interface: ExtensionHost"
date = 2024-10-23T14:54:43+08:00
weight = 140
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/extensions-sdk/ExtensionHost/](https://docs.docker.com/reference/api/extensions-sdk/ExtensionHost/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Interface: ExtensionHost

**`Since`**

0.2.0

## Properties

### cli

• `Readonly` **cli**: [`ExtensionCli`]({{< ref "/reference/APIreference/ExtensionsAPI/InterfaceExtensionCli" >}})

Executes a command in the host.

For example, execute the shipped binary `kubectl -h` command in the host:



```typescript
await ddClient.extension.host.cli.exec("kubectl", ["-h"]);
```

------

Streams the output of the command executed in the backend container or in the host.

Provided the `kubectl` binary is shipped as part of your extension, you can spawn the `kubectl -h` command in the host:



```typescript
await ddClient.extension.host.cli.exec("kubectl", ["-h"], {
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
