+++
title = "The build and publish process"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/extensions/extensions-sdk/process/](https://docs.docker.com/extensions/extensions-sdk/process/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# The build and publish process

This documentation is structured so that it matches the steps you need to take when creating your extension.

There are two main parts to creating a Docker extension:

1. Build the foundations
2. Publish the extension

> **Note**
>
> 
>
> You do not need to pay to create a Docker extension. The [Docker Extension SDK](https://www.npmjs.com/package/@docker/extension-api-client) is licensed under the Apache 2.0 License and is free to use. Anyone can create new extensions and share them without constraints.
>
> There is also no constraint on how each extension should be licensed, this is up to you to decide when creating a new extension.

## [Part one: Build the foundations](https://docs.docker.com/extensions/extensions-sdk/process/#part-one-build-the-foundations)

The build process consists of:

- Installing the latest version of Docker Desktop.
- Setting up the directory with files, including the extension’s source code and the required extension-specific files.
- Creating the `Dockerfile` to build, publish, and run your extension in Docker Desktop.
- Configuring the metadata file which is required at the root of the image filesystem.
- Building and installing the extension.

For further inspiration, see the other examples in the [samples folder](https://github.com/docker/extensions-sdk/tree/main/samples).

> **Tip**
>
> 
>
> Whilst creating your extension, make sure you follow the [design](https://docs.docker.com/extensions/extensions-sdk/design/design-guidelines/) and [UI styling](https://docs.docker.com/extensions/extensions-sdk/design/) guidelines to ensure visual consistency and [level AA accessibility standards](https://www.w3.org/WAI/WCAG2AA-Conformance).

## [Part two: Publish and distribute your extension](https://docs.docker.com/extensions/extensions-sdk/process/#part-two-publish-and-distribute-your-extension)

Docker Desktop displays published extensions in the Extensions Marketplace. The Extensions Marketplace is a curated space where developers can discover extensions to improve their developer experience and upload their own extension to share with the world.

If you want your extension published in the Marketplace, read the [publish documentation](https://docs.docker.com/extensions/extensions-sdk/extensions/publish/).

> Already built an extension?
>
> Let us know about your experience using the [feedback form](https://survey.alchemer.com/s3/7184948/Publishers-Feedback-Form).

## [What’s next?](https://docs.docker.com/extensions/extensions-sdk/process/#whats-next)

If you want to get up and running with creating a Docker Extension, see the [Quickstart guide](https://docs.docker.com/extensions/extensions-sdk/quickstart/).

Alternatively, get started with reading the "Part one: Build" section for more in-depth information about each step of the extension creation process.

For an in-depth tutorial of the entire build process, we recommend the following video walkthrough from DockerCon 2022.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Yv7OG-EGJsg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border-width: 0px; border-style: solid; border-color: initial; display: block; vertical-align: middle; margin-bottom: 0px;"></iframe>

