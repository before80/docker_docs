+++
title = "Targets"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/build/bake/targets/](https://docs.docker.com/build/bake/targets/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Bake targets

A target in a Bake file represents a build invocation. It holds all the information you would normally pass to a `docker build` command using flags.



```hcl
target "webapp" {
  dockerfile = "webapp.Dockerfile"
  tags = ["docker.io/username/webapp:latest"]
  context = "https://github.com/username/webapp"
}
```

To build a target with Bake, pass name of the target to the `bake` command.



```console
$ docker buildx bake webapp
```

You can build multiple targets at once by passing multiple target names to the `bake` command.



```console
$ docker buildx bake webapp api tests
```

## [Default target](https://docs.docker.com/build/bake/targets/#default-target)

If you don't specify a target when running `docker buildx bake`, Bake will build the target named `default`.



```hcl
target "default" {
  dockerfile = "webapp.Dockerfile"
  tags = ["docker.io/username/webapp:latest"]
  context = "https://github.com/username/webapp"
}
```

To build this target, run `docker buildx bake` without any arguments:



```console
$ docker buildx bake
```

## [Target properties](https://docs.docker.com/build/bake/targets/#target-properties)

The properties you can set for a target closely resemble the CLI flags for `docker build`, with a few additional properties that are specific to Bake.

For all the properties you can set for a target, see the [Bake reference](https://docs.docker.com/build/bake/reference#target).

## [Grouping targets](https://docs.docker.com/build/bake/targets/#grouping-targets)

You can group targets together using the `group` block. This is useful when you want to build multiple targets at once.



```hcl
group "all" {
  targets = ["webapp", "api", "tests"]
}

target "webapp" {
  dockerfile = "webapp.Dockerfile"
  tags = ["docker.io/username/webapp:latest"]
  context = "https://github.com/username/webapp"
}

target "api" {
  dockerfile = "api.Dockerfile"
  tags = ["docker.io/username/api:latest"]
  context = "https://github.com/username/api"
}

target "tests" {
  dockerfile = "tests.Dockerfile"
  contexts = {
    webapp = "target:webapp",
    api = "target:api",
  }
  output = ["type=local,dest=build/tests"]
  context = "."
}
```

To build all the targets in a group, pass the name of the group to the `bake` command.



```console
$ docker buildx bake all
```

## [Additional resources](https://docs.docker.com/build/bake/targets/#additional-resources)

Refer to the following pages to learn more about Bake's features:

- Learn how to use [variables](https://docs.docker.com/build/bake/variables/) in Bake to make your build configuration more flexible.
- Learn how you can use matrices to build multiple images with different configurations in [Matrices](https://docs.docker.com/build/bake/matrices/).
- Head to the [Bake file reference](https://docs.docker.com/build/bake/reference/) to learn about all the properties you can set in a Bake file, and its syntax.
