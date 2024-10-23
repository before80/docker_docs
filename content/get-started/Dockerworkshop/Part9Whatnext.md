+++
title = "Part 9: What next"
date = 2024-10-23T14:54:35+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/get-started/workshop/10_what_next/](https://docs.docker.com/get-started/workshop/10_what_next/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# What next after the Docker workshop

Although you're done with the workshop, there's still a lot more to learn about containers.

Here are a few other areas to look at next.

## [Container orchestration](https://docs.docker.com/get-started/workshop/10_what_next/#container-orchestration)

Running containers in production is tough. You don't want to log into a machine and simply run a `docker run` or `docker compose up`. Why not? Well, what happens if the containers die? How do you scale across several machines? Container orchestration solves this problem. Tools like Kubernetes, Swarm, Nomad, and ECS all help solve this problem, all in slightly different ways.

The general idea is that you have managers who receive the expected state. This state might be "I want to run two instances of my web app and expose port 80." The managers then look at all of the machines in the cluster and delegate work to worker nodes. The managers watch for changes (such as a container quitting) and then work to make the actual state reflect the expected state.

## [Cloud Native Computing Foundation projects](https://docs.docker.com/get-started/workshop/10_what_next/#cloud-native-computing-foundation-projects)

The CNCF is a vendor-neutral home for various open-source projects, including Kubernetes, Prometheus, Envoy, Linkerd, NATS, and more. You can view the [graduated and incubated projects here](https://www.cncf.io/projects/) and the entire [CNCF Landscape here](https://landscape.cncf.io/). There are a lot of projects to help solve problems around monitoring, logging, security, image registries, messaging, and more.

## [Getting started video workshop](https://docs.docker.com/get-started/workshop/10_what_next/#getting-started-video-workshop)

Docker recommends watching the video workshop from DockerCon 2022. Watch the entire video or use the following links to open the video at a particular section.

- [Docker overview and installation](https://youtu.be/gAGEar5HQoU)
- [Pull, run, and explore containers](https://youtu.be/gAGEar5HQoU?t=1400)
- [Build a container image](https://youtu.be/gAGEar5HQoU?t=3185)
- [Containerize an app](https://youtu.be/gAGEar5HQoU?t=4683)
- [Connect a DB and set up a bind mount](https://youtu.be/gAGEar5HQoU?t=6305)
- [Deploy a container to the cloud](https://youtu.be/gAGEar5HQoU?t=8280)

<iframe src="https://www.youtube-nocookie.com/embed/gAGEar5HQoU" width="560" height="auto" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border-width: 0px; border-style: solid; border-color: initial; display: block; vertical-align: middle; color: rgb(0, 0, 0); font-family: &quot;Roboto Flex&quot;, system-ui, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Oxygen, Ubuntu, Cantarell, &quot;Open Sans&quot;, &quot;Helvetica Neue&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; max-width: 100%; aspect-ratio: 16 / 9;"></iframe>

## [Creating a container from scratch](https://docs.docker.com/get-started/workshop/10_what_next/#creating-a-container-from-scratch)

If you'd like to see how containers are built from scratch, Liz Rice from Aqua Security has a fantastic talk in which she creates a container from scratch in Go. While the talk does not go into networking, using images for the filesystem, and other advanced topics, it gives a deep dive into how things are working.

<iframe src="https://www.youtube-nocookie.com/embed/8fi7uSYlOdc" width="560" height="auto" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border-width: 0px; border-style: solid; border-color: initial; display: block; vertical-align: middle; margin-bottom: 0px; max-width: 100%; aspect-ratio: 16 / 9;"></iframe>

