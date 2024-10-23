+++
title = "CLI reference"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/extensions/extensions-sdk/dev/usage/](https://docs.docker.com/extensions/extensions-sdk/dev/usage/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# CLI reference

The Extensions CLI is an extension development tool that is used to manage Docker extensions. Actions include install, list, remove, and validate extensions.

- `docker extension enable` turns on Docker extensions.
- `docker extension dev` commands for extension development.
- `docker extension disable` turns off Docker extensions.
- `docker extension init` creates a new Docker extension.
- `docker extension install` installs a Docker extension with the specified image.
- `docker extension ls` list installed Docker extensions.
- `docker extension rm` removes a Docker extension.
- `docker extension update` removes and re-installs a Docker extension.
- `docker extension validate` validates the extension metadata file against the JSON schema.
