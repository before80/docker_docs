+++
title = "矩阵目标"
date = 2024-10-23T14:54:40+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/bake/matrices/](https://docs.docker.com/build/bake/matrices/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Matrix targets - 矩阵目标

A matrix strategy lets you fork a single target into multiple different variants, based on parameters that you specify. This works in a similar way to [Matrix strategies for GitHub Actions](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs). You can use this to reduce duplication in your Bake definition.

​	矩阵策略允许您基于指定的参数，将一个目标分叉为多个不同的变体。这与 [GitHub Actions 的矩阵策略](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs)类似。您可以使用此方法减少 Bake 定义中的重复。

The matrix attribute is a map of parameter names to lists of values. Bake builds each possible combination of values as a separate target.

​	矩阵属性是一个参数名称到值列表的映射。Bake 会构建值的每个可能组合作为单独的目标。

Each generated target must have a unique name. To specify how target names should resolve, use the name attribute.

​	每个生成的目标必须有一个唯一的名称。要指定目标名称的解析方式，请使用 `name` 属性。

The following example resolves the app target to `app-foo` and `app-bar`. It also uses the matrix value to define the [target build stage](https://docs.docker.com/build/bake/reference/#targettarget).

​	以下示例将 `app` 目标解析为 `app-foo` 和 `app-bar`。它还使用矩阵值定义了 [目标构建阶段](https://docs.docker.com/build/bake/reference/#targettarget)。



```hcl
target "app" {
  name = "app-${tgt}"
  matrix = {
    tgt = ["foo", "bar"]
  }
  target = tgt
}
```



```console
$ docker buildx bake --print app
[+] Building 0.0s (0/0)
{
  "group": {
    "app": {
      "targets": [
        "app-foo",
        "app-bar"
      ]
    },
    "default": {
      "targets": [
        "app"
      ]
    }
  },
  "target": {
    "app-bar": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "target": "bar"
    },
    "app-foo": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "target": "foo"
    }
  }
}
```

## 多重轴 Multiple axes

You can specify multiple keys in your matrix to fork a target on multiple axes. When using multiple matrix keys, Bake builds every possible variant.

​	您可以在矩阵中指定多个键，以在多个轴上分叉一个目标。当使用多个矩阵键时，Bake 会构建每个可能的变体。

The following example builds four targets:

​	以下示例构建了四个目标：

- `app-foo-1-0`

- `app-foo-2-0`
- `app-bar-1-0`
- `app-bar-2-0`



```hcl
target "app" {
  name = "app-${tgt}-${replace(version, ".", "-")}"
  matrix = {
    tgt = ["foo", "bar"]
    version = ["1.0", "2.0"]
  }
  target = tgt
  args = {
    VERSION = version
  }
}
```

## 每个矩阵目标的多值 Multiple values per matrix target

If you want to differentiate the matrix on more than just a single value, you can use maps as matrix values. Bake creates a target for each map, and you can access the nested values using dot notation.

​	如果您希望在单一值之上对矩阵进行更多的区分，可以使用映射作为矩阵值。Bake 会为每个映射创建一个目标，并且您可以使用点表示法访问嵌套的值。

The following example builds two targets:

​	以下示例构建了两个目标：

- `app-foo-1-0`
- `app-bar-2-0`



```hcl
target "app" {
  name = "app-${item.tgt}-${replace(item.version, ".", "-")}"
  matrix = {
    item = [
      {
        tgt = "foo"
        version = "1.0"
      },
      {
        tgt = "bar"
        version = "2.0"
      }
    ]
  }
  target = item.tgt
  args = {
    VERSION = item.version
  }
}
```
