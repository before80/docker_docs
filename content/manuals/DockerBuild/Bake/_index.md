+++
title = "Bake"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/build/bake/](https://docs.docker.com/build/bake/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Bake

**Experimental**

Bake is an experimental feature, and we are looking for [feedback from users](https://github.com/docker/buildx/issues).

Bake is a feature of Docker Buildx that lets you define your build configuraton using a declarative file, as opposed to specifying a complex CLI expression. It also lets you run multiple builds concurrently with a single invocation.

A Bake file can be written in HCL, JSON, or YAML formats, where the YAML format is an extension of a Docker Compose file. Here's an example Bake file in HCL format:



```hcl
group "default" {
  targets = ["frontend", "backend"]
}

target "frontend" {
  context = "./frontend"
  dockerfile = "frontend.Dockerfile"
  args = {
    NODE_VERSION = "22"
  }
  tags = ["myapp/frontend:latest"]
}

target "backend" {
  context = "./backend"
  dockerfile = "backend.Dockerfile"
  args = {
    GO_VERSION = "1.23"
  }
  tags = ["myapp/backend:latest"]
}
```

The `group` block defines a group of targets that can be built concurrently. Each `target` block defines a build target with its own configuration, such as the build context, Dockerfile, and tags.

To invoke a build using the above Bake file, you can run:



```console
$ docker buildx bake
```

This executes the `default` group, which builds the `frontend` and `backend` targets concurrently.

## [Get started](https://docs.docker.com/build/bake/#get-started)

To learn how to get started with Bake, head over to the [Bake introduction](https://docs.docker.com/build/bake/introduction/).
