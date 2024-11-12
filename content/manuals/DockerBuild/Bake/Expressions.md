+++
title = "表达式"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/bake/expressions/](https://docs.docker.com/build/bake/expressions/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Expression evaluation in Bake - 在 Bake 中进行表达式求值

Bake files in the HCL format support expression evaluation, which lets you perform arithmetic operations, conditionally set values, and more.

​	HCL 格式的 Bake 文件支持表达式求值，允许执行算术操作、条件设置值等。

## 算术操作 Arithmetic operations

You can perform arithmetic operations in expressions. The following example shows how to multiply two numbers.

​	您可以在表达式中执行算术运算。以下示例展示了如何将两个数字相乘：



docker-bake.hcl



```hcl
sum = 7*6

target "default" {
  args = {
    answer = sum
  }
}
```

Printing the Bake file with the `--print` flag shows the evaluated value for the `answer` build argument.

​	使用 `--print` 标志打印 Bake 文件，可以显示 `answer` 构建参数的计算值。



```console
$ docker buildx bake --print app
```



```json
{
  "target": {
    "default": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "args": {
        "answer": "42"
      }
    }
  }
}
```

## 三元运算符 Ternary operators

You can use ternary operators to conditionally register a value.

​	您可以使用三元运算符来有条件地设置值。

The following example adds a tag only when a variable is not empty, using the built-in `notequal` [function]({{< ref "/manuals/DockerBuild/Bake/Functions" >}}).

​	以下示例仅在变量不为空时添加一个标签，使用了内置的 `notequal` [函数]({{< ref "/manuals/DockerBuild/Bake/Functions" >}})。

docker-bake.hcl



```hcl
variable "TAG" {}

target "default" {
  context="."
  dockerfile="Dockerfile"
  tags = [
    "my-image:latest",
    notequal("",TAG) ? "my-image:${TAG}": "",
  ]
}
```

In this case, `TAG` is an empty string, so the resulting build configuration only contains the hard-coded `my-image:latest` tag.

​	在此示例中，`TAG` 是一个空字符串，因此最终的构建配置仅包含硬编码的 `my-image:latest` 标签。



```console
$ docker buildx bake --print
```



```json
{
  "group": {
    "default": {
      "targets": ["default"]
    }
  },
  "target": {
    "webapp": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "tags": ["my-image:latest"]
    }
  }
}
```

## 带变量的表达式 Expressions with variables

You can use expressions with [variables]({{< ref "/manuals/DockerBuild/Bake/Variables" >}}) to conditionally set values, or to perform arithmetic operations.

​	您可以使用带有[变量]({{< ref "/manuals/DockerBuild/Bake/Variables" >}})的表达式来有条件地设置值或执行算术运算。

The following example uses expressions to set values based on the value of variables. The `v1` build argument is set to "higher" if the variable `FOO` is greater than 5, otherwise it is set to "lower". The `v2` build argument is set to "yes" if the `IS_FOO` variable is true, otherwise it is set to "no".

​	以下示例使用表达式根据变量的值来设置值。如果变量 `FOO` 大于 5，则将构建参数 `v1` 设置为 "higher"，否则设置为 "lower"。如果变量 `IS_FOO` 为 true，则将构建参数 `v2` 设置为 "yes"，否则设置为 "no"。

docker-bake.hcl



```hcl
variable "FOO" {
  default = 3
}

variable "IS_FOO" {
  default = true
}

target "app" {
  args = {
    v1 = FOO > 5 ? "higher" : "lower"
    v2 = IS_FOO ? "yes" : "no"
  }
}
```

Printing the Bake file with the `--print` flag shows the evaluated values for the `v1` and `v2` build arguments.

​	使用 `--print` 标志打印 Bake 文件，可以显示 `v1` 和 `v2` 构建参数的计算值。

```console
$ docker buildx bake --print app
```



```json
{
  "group": {
    "default": {
      "targets": ["app"]
    }
  },
  "target": {
    "app": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "args": {
        "v1": "lower",
        "v2": "yes"
      }
    }
  }
}
```
