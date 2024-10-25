+++
title = "Part 9: 接下来"
date = 2024-10-23T14:54:35+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/get-started/workshop/10_what_next/](https://docs.docker.com/get-started/workshop/10_what_next/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# What next after the Docker workshop - Docker 讲习班之后的下一步

Although you're done with the workshop, there's still a lot more to learn about containers.

​	虽然你已经完成了讲习班，但关于容器还有很多内容值得学习。

Here are a few other areas to look at next.

​	以下是一些可以进一步了解的领域。

## 容器编排 Container orchestration

Running containers in production is tough. You don't want to log into a machine and simply run a `docker run` or `docker compose up`. Why not? Well, what happens if the containers die? How do you scale across several machines? Container orchestration solves this problem. Tools like Kubernetes, Swarm, Nomad, and ECS all help solve this problem, all in slightly different ways.

​	在生产环境中运行容器是个难题。你不希望仅仅登录到一台机器上运行 `docker run` 或 `docker compose up`。为什么呢？如果容器崩溃了怎么办？如何在多台机器之间进行扩展？容器编排可以解决这些问题。像 Kubernetes、Swarm、Nomad 和 ECS 这样的工具以不同的方式帮助解决这些问题。

The general idea is that you have managers who receive the expected state. This state might be "I want to run two instances of my web app and expose port 80." The managers then look at all of the machines in the cluster and delegate work to worker nodes. The managers watch for changes (such as a container quitting) and then work to make the actual state reflect the expected state.

​	一般的想法是，你有一些管理者节点接收期望的状态。这个状态可能是“我想要运行我的 Web 应用程序的两个实例并暴露 80 端口。”然后管理者查看集群中的所有机器，并将任务分配给工作节点。管理者监控状态变化（例如一个容器退出），并确保实际状态与期望状态保持一致。

## 云原生计算基金会项目 Cloud Native Computing Foundation projects

The CNCF is a vendor-neutral home for various open-source projects, including Kubernetes, Prometheus, Envoy, Linkerd, NATS, and more. You can view the [graduated and incubated projects here](https://www.cncf.io/projects/) and the entire [CNCF Landscape here](https://landscape.cncf.io/). There are a lot of projects to help solve problems around monitoring, logging, security, image registries, messaging, and more.

​	CNCF 是一个供应商中立的开源项目组织，包含 Kubernetes、Prometheus、Envoy、Linkerd、NATS 等。你可以查看[毕业和孵化的项目](https://www.cncf.io/projects/)和[整个 CNCF 项目全景图](https://landscape.cncf.io/)。这里有许多项目可以帮助解决监控、日志记录、安全、镜像仓库、消息传递等方面的问题。

## 入门视频讲习班 Getting started video workshop

Docker recommends watching the video workshop from DockerCon 2022. Watch the entire video or use the following links to open the video at a particular section.

​	Docker 推荐观看 DockerCon 2022 的视频讲习班。你可以观看完整视频，或者使用以下链接跳转到特定部分。

- [Docker overview and installation Docker 概述与安装](https://youtu.be/gAGEar5HQoU)
- [Pull, run, and explore containers 拉取、运行和探索容器](https://youtu.be/gAGEar5HQoU?t=1400)
- [Build a container image 构建容器镜像](https://youtu.be/gAGEar5HQoU?t=3185)
- [Containerize an app 将应用程序容器化](https://youtu.be/gAGEar5HQoU?t=4683)
- [Connect a DB and set up a bind mount 连接数据库并设置绑定挂载](https://youtu.be/gAGEar5HQoU?t=6305)
- [Deploy a container to the cloud 将容器部署到云端](https://youtu.be/gAGEar5HQoU?t=8280)



<iframe src="https://www.youtube-nocookie.com/embed/" width="560" height="auto" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border-width: 0px; border-style: solid; border-color: initial; display: block; vertical-align: middle; color: rgb(0, 0, 0); font-family: &quot;Roboto Flex&quot;, system-ui, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Oxygen, Ubuntu, Cantarell, &quot;Open Sans&quot;, &quot;Helvetica Neue&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; max-width: 100%; aspect-ratio: 16 / 9;"></iframe>

## 从头创建一个容器 Creating a container from scratch

If you'd like to see how containers are built from scratch, Liz Rice from Aqua Security has a fantastic talk in which she creates a container from scratch in Go. While the talk does not go into networking, using images for the filesystem, and other advanced topics, it gives a deep dive into how things are working.

​	如果你想了解容器是如何从头构建的，Aqua Security 的 Liz Rice 有一个非常棒的演讲，在其中她用 Go 从零开始创建一个容器。虽然演讲没有涉及网络、使用镜像作为文件系统等高级主题，但深入展示了底层原理的工作机制。

<iframe src="https://www.youtube-nocookie.com/embed/8fi7uSYlOdc" width="560" height="auto" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border-width: 0px; border-style: solid; border-color: initial; display: block; vertical-align: middle; margin-bottom: 0px; max-width: 100%; aspect-ratio: 16 / 9;"></iframe>

