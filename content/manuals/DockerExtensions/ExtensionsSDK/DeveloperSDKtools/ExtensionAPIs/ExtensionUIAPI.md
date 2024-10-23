+++
title = "Extension UI API"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/extensions/extensions-sdk/dev/api/overview/](https://docs.docker.com/extensions/extensions-sdk/dev/api/overview/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Extension UI API

The extensions UI runs in a sandboxed environment and doesn't have access to any electron or nodejs APIs.

The extension UI API provides a way for the frontend to perform different actions and communicate with the Docker Desktop dashboard or the underlying system.

JavaScript API libraries, with Typescript support, are available in order to get all the API definitions in to your extension code.

- [@docker/extension-api-client](https://www.npmjs.com/package/@docker/extension-api-client) gives access to the extension API entrypoint `DockerDesktopCLient`.
- [@docker/extension-api-client-types](https://www.npmjs.com/package/@docker/extension-api-client-types) can be added as a dev dependency in order to get types auto-completion in your IDE.



```Typescript
import { createDockerDesktopClient } from '@docker/extension-api-client';

export function App() {
  // obtain Docker Desktop client
  const ddClient = createDockerDesktopClient();
  // use ddClient to perform extension actions
}
```

The `ddClient` object gives access to various APIs:

- [Extension Backend](https://docs.docker.com/extensions/extensions-sdk/dev/api/backend/)
- [Docker](https://docs.docker.com/extensions/extensions-sdk/dev/api/docker/)
- [Dashboard](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard/)
- [Navigation](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard-routes-navigation/)

Find the Extensions API reference [here](https://docs.docker.com/reference/api/extensions-sdk/).
