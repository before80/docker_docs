+++
title = "containerd image store"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/containerd/](https://docs.docker.com/desktop/containerd/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# containerd image store

This page provides information about the ongoing integration of `containerd` for image and file system management in the Docker Engine.

> **Note**
>
> 
>
> Images and containers are not shared between the classic image store and the new containerd image store. When you switch image stores, containers and images from the inactive store remain but are hidden until you switch back.

## [What is containerd?](https://docs.docker.com/desktop/containerd/#what-is-containerd)

`containerd` is an abstraction of the low-level kernel features used to run and manage containers on a system. It's a platform used in container software like Docker and Kubernetes.

Docker Engine already uses `containerd` for container lifecycle management, which includes creating, starting, and stopping containers. This page describes the next step of the containerd integration for Docker: the containerd image store.

## [Image store](https://docs.docker.com/desktop/containerd/#image-store)

The image store is the component responsible for pushing, pulling, and storing images on the filesystem. The classic Docker image store is limited in the types of images that it supports. For example, it doesn't support image indices, containing manifest lists. When you create multi-platform images, for example, the image index resolves all the platform-specific variants of the image. An image index is also required when building images with attestations.

The containerd image store extends range of image types that the Docker Engine can natively interact with. While this is a low-level architectural change, it's a prerequisite for unlocking a range of new use cases, including:

- [Build multi-platform images](https://docs.docker.com/desktop/containerd/#build-multi-platform-images) and images with attestations
- Support for using containerd snapshotters with unique characteristics, such as [stargz](https://github.com/containerd/stargz-snapshotter) for lazy-pulling images on container startup, or [nydus](https://github.com/containerd/nydus-snapshotter) and [dragonfly](https://github.com/dragonflyoss/image-service) for peer-to-peer image distribution.
- Ability to run [Wasm](https://docs.docker.com/desktop/wasm/) containers

## [Enable the containerd image store](https://docs.docker.com/desktop/containerd/#enable-the-containerd-image-store)

The containerd image store is enabled by default in Docker Desktop version 4.34 and later, but only for clean installs or if you perform a factory reset. If you upgrade from an earlier version of Docker Desktop, or if you use an older version of Docker Desktop you must manually switch to the containerd image store.

To manually enable this feature in Docker Desktop:

1. Navigate to **Settings** in Docker Desktop.
2. In the **General** tab, check **Use containerd for pulling and storing images**.
3. Select **Apply & Restart**.

To disable the containerd image store, clear the **Use containerd for pulling and storing images** checkbox.

## [Build multi-platform images](https://docs.docker.com/desktop/containerd/#build-multi-platform-images)

The term multi-platform image refers to a bundle of images for multiple different architectures. Out of the box, the default builder for Docker Desktop doesn't support building multi-platform images.



```console
$ docker build --platform=linux/amd64,linux/arm64 .
[+] Building 0.0s (0/0)
ERROR: Multi-platform build is not supported for the docker driver.
Switch to a different driver, or turn on the containerd image store, and try again.
Learn more at https://docs.docker.com/go/build-multi-platform/
```

Enabling the containerd image store lets you build multi-platform images and load them to your local image store:

<iframe src="https://asciinema.org/a/ZSUI4Mi2foChLjbevl2dxt5GD/iframe?" id="asciicast-iframe-ZSUI4Mi2foChLjbevl2dxt5GD" name="asciicast-iframe-ZSUI4Mi2foChLjbevl2dxt5GD" scrolling="no" allowfullscreen="true" title="Terminal session recording" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border: 0px; display: inline-block; vertical-align: middle; overflow: hidden; margin: 0px; width: 895.996px; visibility: visible; height: 565px;"></iframe>

## [Feedback](https://docs.docker.com/desktop/containerd/#feedback)

Thanks for trying the new features available with `containerd`. Give feedback or report any bugs you may find through the issues tracker on the [feedback form](https://dockr.ly/3PODIhD).
