+++
title = "继承"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/bake/inheritance/](https://docs.docker.com/build/bake/inheritance/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Inheritance in Bake - Bake 中的继承

Targets can inherit attributes from other targets, using the `inherits` attribute. For example, imagine that you have a target that builds a Docker image for a development environment:

​	目标可以使用 `inherits` 属性从其他目标继承属性。例如，假设您有一个构建开发环境 Docker 镜像的目标：



```hcl
target "app-dev" {
  args = {
    GO_VERSION = "1.23"
  }
  tags = ["docker.io/username/myapp:dev"]
  labels = {
    "org.opencontainers.image.source" = "https://github.com/username/myapp"
    "org.opencontainers.image.author" = "moby.whale@example.com"
  }
}
```

You can create a new target that uses the same build configuration, but with slightly different attributes for a production build. In this example, the `app-release` target inherits the `app-dev` target, but overrides the `tags` attribute and adds a new `platforms` attribute:

​	您可以创建一个新目标，使用相同的构建配置，但略微修改属性以用于生产环境构建。在此示例中，`app-release` 目标继承了 `app-dev` 目标，但覆盖了 `tags` 属性并新增了 `platforms` 属性：



```hcl
target "app-release" {
  inherits = ["app-dev"]
  tags = ["docker.io/username/myapp:latest"]
  platforms = ["linux/amd64", "linux/arm64"]
}
```

## 通用可复用目标 Common reusable targets

One common inheritance pattern is to define a common target that contains shared attributes for all or many of the build targets in the project. For example, the following `_common` target defines a common set of build arguments:

​	一种常见的继承模式是定义一个包含项目中所有或许多构建目标共享属性的通用目标。例如，以下 `_common` 目标定义了一组通用的构建参数：



```hcl
target "_common" {
  args = {
    GO_VERSION = "1.23"
    BUILDKIT_CONTEXT_KEEP_GIT_DIR = 1
  }
}
```

You can then inherit the `_common` target in other targets to apply the shared attributes:

​	然后，您可以在其他目标中继承 `_common` 目标，以应用共享属性：



```hcl
target "lint" {
  inherits = ["_common"]
  dockerfile = "./dockerfiles/lint.Dockerfile"
  output = ["type=cacheonly"]
}

target "docs" {
  inherits = ["_common"]
  dockerfile = "./dockerfiles/docs.Dockerfile"
  output = ["./docs/reference"]
}

target "test" {
  inherits = ["_common"]
  target = "test-output"
  output = ["./test"]
}

target "binaries" {
  inherits = ["_common"]
  target = "binaries"
  output = ["./build"]
  platforms = ["local"]
}
```

## 覆盖继承的属性 Overriding inherited attributes

When a target inherits another target, it can override any of the inherited attributes. For example, the following target overrides the `args` attribute from the inherited target:

​	当一个目标继承另一个目标时，它可以覆盖任何继承的属性。例如，以下目标覆盖了继承目标的 `args` 属性：



```hcl
target "app-dev" {
  inherits = ["_common"]
  args = {
    GO_VERSION = "1.17"
  }
  tags = ["docker.io/username/myapp:dev"]
}
```

The `GO_VERSION` argument in `app-release` is set to `1.17`, overriding the `GO_VERSION` argument from the `app-dev` target.

​	在 `app-release` 中，`GO_VERSION` 参数被设置为 `1.17`，覆盖了 `app-dev` 目标中的 `GO_VERSION` 参数。

For more information about overriding attributes, see the [Overriding configurations]({{< ref "/manuals/DockerBuild/Bake/Overridingconfigurations" >}}) page.

​	有关覆盖属性的更多信息，请参阅 [Overriding configurations]({{< ref "/manuals/DockerBuild/Bake/Overridingconfigurations" >}}) 页面。

## 从多个目标继承 Inherit from multiple targets

The `inherits` attribute is a list, meaning you can reuse attributes from multiple other targets. In the following example, the app-release target reuses attributes from both the `app-dev` and `_common` targets.

​	`inherits` 属性是一个列表，这意味着您可以从多个其他目标中重用属性。以下示例中，`app-release` 目标重用了 `app-dev` 和 `_common` 目标中的属性。



```hcl
target "_common" {
  args = {
    GO_VERSION = "1.23"
    BUILDKIT_CONTEXT_KEEP_GIT_DIR = 1
  }
}

target "app-dev" {
  inherits = ["_common"]
  args = {
    BUILDKIT_CONTEXT_KEEP_GIT_DIR = 0
  }
  tags = ["docker.io/username/myapp:dev"]
  labels = {
    "org.opencontainers.image.source" = "https://github.com/username/myapp"
    "org.opencontainers.image.author" = "moby.whale@example.com"
  }
}

target "app-release" {
  inherits = ["app-dev", "_common"]
  tags = ["docker.io/username/myapp:latest"]
  platforms = ["linux/amd64", "linux/arm64"]
}
```

When inheriting attributes from multiple targets and there's a conflict, the target that appears last in the inherits list takes precedence. The previous example defines the `BUILDKIT_CONTEXT_KEEP_GIT_DIR` in the `_common` target and overrides it in the `app-dev` target.

​	当从多个目标继承且存在冲突时，继承列表中最后出现的目标具有优先级。以上示例中，`BUILDKIT_CONTEXT_KEEP_GIT_DIR` 在 `_common` 目标中定义，并在 `app-dev` 目标中被覆盖。

The `app-release` target inherits both `app-dev` target and the `_common` target. The `BUILDKIT_CONTEXT_KEEP_GIT_DIR` argument is set to 0 in the `app-dev` target and 1 in the `_common` target. The `BUILDKIT_CONTEXT_KEEP_GIT_DIR` argument in the `app-release` target is set to 1, not 0, because the `_common` target appears last in the inherits list.

​	`app-release` 目标继承了 `app-dev` 和 `_common` 目标，其中 `BUILDKIT_CONTEXT_KEEP_GIT_DIR` 参数在 `app-dev` 中设为 0，而在 `_common` 中为 1。由于 `_common` 在继承列表中最后出现，所以 `app-release` 中的 `BUILDKIT_CONTEXT_KEEP_GIT_DIR` 设置为 1，而非 0。

## 从目标中重用单个属性 Reusing single attributes from targets

If you only want to inherit a single attribute from a target, you can reference an attribute from another target using dot notation. For example, in the following Bake file, the `bar` target reuses the `tags` attribute from the `foo` target:

​	如果您只想从一个目标中继承单个属性，可以使用点表示法引用另一个目标的属性。例如，在以下 Bake 文件中，`bar` 目标重用了 `foo` 目标的 `tags` 属性。

docker-bake.hcl



```hcl
target "foo" {
  dockerfile = "foo.Dockerfile"
  tags       = ["myapp:latest"]
}
target "bar" {
  dockerfile = "bar.Dockerfile"
  tags       = target.foo.tags
}
```
