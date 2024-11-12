+++
title = "函数"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/bake/funcs/](https://docs.docker.com/build/bake/funcs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Functions - 函数

HCL functions are great for when you need to manipulate values in your build configuration in more complex ways than just concatenation or interpolation.

​	当您需要在构建配置中对值进行更复杂的操作时，HCL 函数非常有用，超出了简单的拼接或插值的需求。

## 标准库 Standard library

Bake ships with built-in support for the [`go-cty` standard library functions](https://github.com/zclconf/go-cty/tree/main/cty/function/stdlib). The following example shows the `add` function.

​	Bake 内置支持 [`go-cty` 标准库函数](https://github.com/zclconf/go-cty/tree/main/cty/function/stdlib)。以下示例展示了 `add` 函数的使用。

docker-bake.hcl



```hcl
variable "TAG" {
  default = "latest"
}

group "default" {
  targets = ["webapp"]
}

target "webapp" {
  args = {
    buildno = "${add(123, 1)}"
  }
}
```



```console
$ docker buildx bake --print webapp
```



```json
{
  "group": {
    "default": {
      "targets": ["webapp"]
    }
  },
  "target": {
    "webapp": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "args": {
        "buildno": "124"
      }
    }
  }
}
```

## 用户定义函数 User-defined functions

You can create [user-defined functions](https://github.com/hashicorp/hcl/tree/main/ext/userfunc) that do just what you want, if the built-in standard library functions don't meet your needs.

​	如果内置的标准库函数无法满足您的需求，您可以创建[用户定义函数](https://github.com/hashicorp/hcl/tree/main/ext/userfunc)。

The following example defines an `increment` function.

​	以下示例定义了一个 `increment` 函数。

docker-bake.hcl



```hcl
function "increment" {
  params = [number]
  result = number + 1
}

group "default" {
  targets = ["webapp"]
}

target "webapp" {
  args = {
    buildno = "${increment(123)}"
  }
}
```



```console
$ docker buildx bake --print webapp
```



```json
{
  "group": {
    "default": {
      "targets": ["webapp"]
    }
  },
  "target": {
    "webapp": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "args": {
        "buildno": "124"
      }
    }
  }
}
```

## 函数中的变量 Variables in functions

You can make references to [variables]({{< ref "/manuals/DockerBuild/Bake/Variables" >}}) and standard library functions inside your functions.

​	您可以在函数中引用[变量]({{< ref "/manuals/DockerBuild/Bake/Variables" >}})和标准库函数。

You can't reference user-defined functions from other functions.

​	您不能在一个函数中引用其他用户定义的函数。

The following example uses a global variable (`REPO`) in a custom function.

​	以下示例在自定义函数中使用了全局变量 (`REPO`)。

docker-bake.hcl



```hcl
# docker-bake.hcl
variable "REPO" {
  default = "user/repo"
}

function "tag" {
  params = [tag]
  result = ["${REPO}:${tag}"]
}

target "webapp" {
  tags = tag("v1")
}
```

Printing the Bake file with the `--print` flag shows that the `tag` function uses the value of `REPO` to set the prefix of the tag.

​	使用 `--print` 标志打印 Bake 文件，显示 `tag` 函数使用了 `REPO` 的值来设置标签的前缀。



```console
$ docker buildx bake --print webapp
```



```json
{
  "group": {
    "default": {
      "targets": ["webapp"]
    }
  },
  "target": {
    "webapp": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "tags": ["user/repo:v1"]
    }
  }
}
```
