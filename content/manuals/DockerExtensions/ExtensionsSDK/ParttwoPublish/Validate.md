+++
title = "Validate"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/extensions/extensions-sdk/extensions/validate/](https://docs.docker.com/extensions/extensions-sdk/extensions/validate/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Validate your extension

Validate your extension before you share or publish it. Validating the extension ensures that the extension:

- Is built with the [image labels]({{< ref "/manuals/DockerExtensions/ExtensionsSDK/ParttwoPublish/Addlabels" >}}) it requires to display correctly in the marketplace
- Installs and runs without problems

The Extensions CLI lets you validate your extension before installing and running it locally.

The validation checks if the extension’s `Dockerfile` specifies all the required labels and if the metadata file is valid against the JSON schema file.

To validate, run:



```console
$ docker extension validate <name-of-your-extension>
```

If your extension is valid, the following message displays:



```console
The extension image "name-of-your-extension" is valid
```

Before the image is built, it's also possible to validate only the `metadata.json` file:



```console
$ docker extension validate /path/to/metadata.json
```

The JSON schema used to validate the `metadata.json` file against can be found under the [releases page](https://github.com/docker/extensions-sdk/releases/latest).
