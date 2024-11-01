+++
title = "containerd 镜像存储"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/containerd/](https://docs.docker.com/desktop/containerd/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# containerd image store - containerd 镜像存储

This page provides information about the ongoing integration of `containerd` for image and file system management in the Docker Engine.

​	本页面提供了有关 `containerd` 在 Docker 引擎中用于镜像和文件系统管理的集成信息。

> **Note**
>
> 
>
> Images and containers are not shared between the classic image store and the new containerd image store. When you switch image stores, containers and images from the inactive store remain but are hidden until you switch back.
>
> ​	镜像和容器不会在经典镜像存储与新的 containerd 镜像存储之间共享。当您切换镜像存储时，不活跃存储中的容器和镜像会保留，但会被隐藏，直到您切换回去。

## What is containerd?

`containerd` is an abstraction of the low-level kernel features used to run and manage containers on a system. It's a platform used in container software like Docker and Kubernetes.

​	`containerd` 是一种对系统上运行和管理容器所使用的底层内核功能的抽象。它是 Docker 和 Kubernetes 等容器软件中使用的平台。

Docker Engine already uses `containerd` for container lifecycle management, which includes creating, starting, and stopping containers. This page describes the next step of the containerd integration for Docker: the containerd image store.

​	Docker 引擎已使用 `containerd` 来管理容器生命周期，包括创建、启动和停止容器。此页面介绍 Docker 集成 containerd 的下一步：containerd 镜像存储。

## 镜像存储 Image store

The image store is the component responsible for pushing, pulling, and storing images on the filesystem. The classic Docker image store is limited in the types of images that it supports. For example, it doesn't support image indices, containing manifest lists. When you create multi-platform images, for example, the image index resolves all the platform-specific variants of the image. An image index is also required when building images with attestations.

​	镜像存储负责在文件系统上推送、拉取和存储镜像。经典的 Docker 镜像存储支持的镜像类型有限，例如，它不支持包含清单列表的镜像索引。当您创建多平台镜像时，镜像索引可以解析该镜像的所有平台特定变体。构建带有证明的镜像时也需要镜像索引。

The containerd image store extends range of image types that the Docker Engine can natively interact with. While this is a low-level architectural change, it's a prerequisite for unlocking a range of new use cases, including:

​	containerd 镜像存储扩展了 Docker 引擎可以本地交互的镜像类型范围。尽管这是一个底层的架构更改，但它是解锁新用例的前提条件，包括：

- [Build multi-platform images](https://docs.docker.com/desktop/containerd/#build-multi-platform-images) and images with attestations
  - [构建多平台镜像](https://docs.docker.com/desktop/containerd/#build-multi-platform-images)和带有证明的镜像

- Support for using containerd snapshotters with unique characteristics, such as [stargz](https://github.com/containerd/stargz-snapshotter) for lazy-pulling images on container startup, or [nydus](https://github.com/containerd/nydus-snapshotter) and [dragonfly](https://github.com/dragonflyoss/image-service) for peer-to-peer image distribution.
  - 支持使用具有独特特性的 containerd 快照程序，例如在容器启动时延迟拉取镜像的 [stargz](https://github.com/containerd/stargz-snapshotter)，或用于点对点镜像分发的 [nydus](https://github.com/containerd/nydus-snapshotter) 和 [dragonfly](https://github.com/dragonflyoss/image-service)。

- Ability to run [Wasm]({{< ref "/manuals/DockerDesktop/WasmworkloadsBeta" >}}) containers
  - 能够运行 [Wasm]({{< ref "/manuals/DockerDesktop/WasmworkloadsBeta" >}}) 容器

## 启用 containerd 镜像存储 Enable the containerd image store

The containerd image store is enabled by default in Docker Desktop version 4.34 and later, but only for clean installs or if you perform a factory reset. If you upgrade from an earlier version of Docker Desktop, or if you use an older version of Docker Desktop you must manually switch to the containerd image store.

​	在 Docker Desktop 4.34 及更高版本中，containerd 镜像存储默认启用，但仅适用于全新安装或工厂重置。如果从早期版本升级 Docker Desktop 或使用旧版本的 Docker Desktop，则必须手动切换到 containerd 镜像存储。

To manually enable this feature in Docker Desktop:

​	要在 Docker Desktop 中手动启用此功能：

1. Navigate to **Settings** in Docker Desktop. 在 Docker Desktop 中导航到 **设置**。
2. In the **General** tab, check **Use containerd for pulling and storing images**. 在 **常规** 选项卡中，勾选 **使用 containerd 进行镜像的拉取和存储**。
3. Select **Apply & Restart**. 选择 **应用并重启**。

To disable the containerd image store, clear the **Use containerd for pulling and storing images** checkbox.

​	要禁用 containerd 镜像存储，请取消选中 **使用 containerd 进行镜像的拉取和存储** 复选框。

## 构建多平台镜像 Build multi-platform images

The term multi-platform image refers to a bundle of images for multiple different architectures. Out of the box, the default builder for Docker Desktop doesn't support building multi-platform image

​	多平台镜像指的是一组适用于不同架构的镜像。默认情况下，Docker Desktop 的默认构建器不支持构建多平台镜像。



```console
$ docker build --platform=linux/amd64,linux/arm64 .
[+] Building 0.0s (0/0)
ERROR: Multi-platform build is not supported for the docker driver.
Switch to a different driver, or turn on the containerd image store, and try again.
Learn more at https://docs.docker.com/go/build-multi-platform/
```

Enabling the containerd image store lets you build multi-platform images and load them to your local image store:

​	启用 containerd 镜像存储后，您可以构建多平台镜像并将其加载到本地镜像存储中：

<iframe src="https://asciinema.org/a/ZSUI4Mi2foChLjbevl2dxt5GD/iframe?" id="asciicast-iframe-ZSUI4Mi2foChLjbevl2dxt5GD" name="asciicast-iframe-ZSUI4Mi2foChLjbevl2dxt5GD" scrolling="no" allowfullscreen="true" title="Terminal session recording" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border: 0px; display: inline-block; vertical-align: middle; overflow: hidden; margin: 0px; width: 895.996px; visibility: visible; height: 565px;"></iframe>

## 反馈 Feedback

Thanks for trying the new features available with `containerd`. Give feedback or report any bugs you may find through the issues tracker on the [feedback form](https://dockr.ly/3PODIhD).

​	感谢您试用 `containerd` 带来的新功能。请通过 [反馈表单](https://dockr.ly/3PODIhD) 的问题跟踪器提交反馈或报告您发现的任何错误。
