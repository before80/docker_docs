+++
title = "Functions"
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

# Functions

HCL functions are great for when you need to manipulate values in your build configuration in more complex ways than just concatenation or interpolation.

## Standard library

Bake ships with built-in support for the [`go-cty` standard library functions](https://github.com/zclconf/go-cty/tree/main/cty/function/stdlib). The following example shows the `add` function.

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

## User-defined functions

You can create [user-defined functions](https://github.com/hashicorp/hcl/tree/main/ext/userfunc) that do just what you want, if the built-in standard library functions don't meet your needs.

The following example defines an `increment` function.

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

## Variables in functions

You can make references to [variables]({{< ref "/manuals/DockerBuild/Bake/Variables" >}}) and standard library functions inside your functions.

You can't reference user-defined functions from other functions.

The following example uses a global variable (`REPO`) in a custom function.

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
