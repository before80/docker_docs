+++
title = "Metadata"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/extensions/extensions-sdk/architecture/metadata/](https://docs.docker.com/extensions/extensions-sdk/architecture/metadata/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Extension metadata

## [The metadata.json file](https://docs.docker.com/extensions/extensions-sdk/architecture/metadata/#the-metadatajson-file)

The `metadata.json` file is the entry point for your extension. It contains the metadata for your extension, such as the name, version, and description. It also contains the information needed to build and run your extension. The image for a Docker extension must include a `metadata.json` file at the root of its filesystem.

The format of the `metadata.json` file must be:



```json
{
    "icon": "extension-icon.svg",
    "ui": ...
    "vm": ...
    "host": ...
}
```

The `ui`, `vm`, and `host` sections are optional and depend on what a given extension provides. They describe the extension content to be installed.

### [UI section](https://docs.docker.com/extensions/extensions-sdk/architecture/metadata/#ui-section)

The `ui` section defines a new tab that's added to the dashboard in Docker Desktop. It follows the form:



```json
"ui":{
    "dashboard-tab":
    {
        "title":"MyTitle",
        "root":"/ui",
        "src":"index.html"
    }
}
```

`root` specifies the folder where the UI code is within the extension image filesystem. `src` specifies the entrypoint that should be loaded in the extension tab.

Other UI extension points will be available in the future.

### [VM section](https://docs.docker.com/extensions/extensions-sdk/architecture/metadata/#vm-section)

The `vm` section defines a backend service that runs inside the Desktop VM. It must define either an `image` or a `docker-compose.yaml` file that specifies what service to run in the Desktop VM.



```json
"vm": {
    "image":"${DESKTOP_PLUGIN_IMAGE}"
},
```

When you use `image`, a default compose file is generated for the extension.

> `${DESKTOP_PLUGIN_IMAGE}` is a specific keyword that allows an easy way to refer to the image packaging the extension. It is also possible to specify any other full image name here. However, in many cases using the same image makes things easier for extension development.



```json
"vm": {
    "composefile": "docker-compose.yaml"
},
```

The Compose file, with a volume definition for example, would look like:



```yaml
services:
  myExtension:
    image: ${DESKTOP_PLUGIN_IMAGE}
    volumes:
      - /host/path:/container/path
```

### [Host section](https://docs.docker.com/extensions/extensions-sdk/architecture/metadata/#host-section)

The `host` section defines executables that Docker Desktop copies on the host.



```json
  "host": {
    "binaries": [
      {
        "darwin": [
          {
            "path": "/darwin/myBinary"
          },
        ],
        "windows": [
          {
            "path": "/windows/myBinary.exe"
          },
        ],
        "linux": [
          {
            "path": "/linux/myBinary"
          },
        ]
      }
    ]
  }
```

`binaries` defines a list of binaries Docker Desktop copies from the extension image to the host.

`path` specifies the binary path in the image filesystem. Docker Desktop is responsible for copying these files in its own location, and the JavaScript API allows invokes these binaries.

Learn how to [invoke executables](https://docs.docker.com/extensions/extensions-sdk/guides/invoke-host-binaries/).
