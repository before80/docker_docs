+++
title = "变量"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/bake/variables/](https://docs.docker.com/build/bake/variables/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Variables in Bake - Bake 中的变量

You can define and use variables in a Bake file to set attribute values, interpolate them into other values, and perform arithmetic operations. Variables can be defined with default values, and can be overridden with environment variables.

​	您可以在 Bake 文件中定义和使用变量，以设置属性值、插入其他值以及执行算术运算。变量可以具有默认值，并可通过环境变量进行覆盖。

## 使用变量作为属性值 Using variables as attribute values

Use the `variable` block to define a variable.

​	使用 `variable` 块定义变量。



```hcl
variable "TAG" {
  default = "docker.io/username/webapp:latest"
}
```

The following example shows how to use the `TAG` variable in a target.

​	以下示例展示了如何在目标中使用 `TAG` 变量。



```hcl
target "default" {
  context = "."
  dockerfile = "Dockerfile"
  tags = [ TAG ]
}
```

## 将变量插入到值中 Interpolate variables into values

Bake supports string interpolation of variables into values. You can use the `${}` syntax to interpolate a variable into a value. The following example defines a `TAG` variable with a value of `latest`.

​	Bake 支持将变量字符串插入到值中。您可以使用 `${}` 语法将变量插入到值中。以下示例定义了一个值为 `latest` 的 `TAG` 变量。



```hcl
variable "TAG" {
  default = "latest"
}
```

To interpolate the `TAG` variable into the value of an attribute, use the `${TAG}` syntax.

​	要将 `TAG` 变量插入到属性值中，使用 `${TAG}` 语法。



```hcl
target "default" {
  context = "."
  dockerfile = "Dockerfile"
  tags = ["docker.io/username/webapp:${TAG}"]
}
```

Printing the Bake file with the `--print` flag shows the interpolated value in the resolved build configuration.

​	使用 `--print` 标志打印 Bake 文件会显示插入后的已解析构建配置中的值。



```console
$ docker buildx bake --print
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
      "tags": ["docker.io/username/webapp:latest"]
    }
  }
}
```

## 逃逸变量插入 Escape variable interpolation

If you want to bypass variable interpolation when parsing the Bake definition, use double dollar signs (`$${VARIABLE}`).

​	如果您希望在解析 Bake 定义时跳过变量插入，请使用双美元符号 (`$${VARIABLE}`)。



```hcl
target "default" {
  dockerfile-inline = <<EOF
  FROM alpine
  ARG TARGETARCH
  RUN echo "Building for $${TARGETARCH/amd64/x64}"
  EOF
  platforms = ["linux/amd64", "linux/arm64"]
}
```



```console
$ docker buildx bake --progress=plain
...
#8 [linux/arm64 2/2] RUN echo "Building for arm64"
#8 0.036 Building for arm64
#8 DONE 0.0s

#9 [linux/amd64 2/2] RUN echo "Building for x64"
#9 0.046 Building for x64
#9 DONE 0.1s
...
```

## 跨文件使用变量中的变量 Using variables in variables across files

When multiple files are specified, one file can use variables defined in another file. In the following example, the `vars.hcl` file defines a `BASE_IMAGE` variable with a default value of `docker.io/library/alpine`.

​	当指定多个文件时，一个文件可以使用另一个文件中定义的变量。在以下示例中，`vars.hcl` 文件定义了一个默认值为 `docker.io/library/alpine` 的 `BASE_IMAGE` 变量。

vars.hcl



```hcl
variable "BASE_IMAGE" {
  default = "docker.io/library/alpine"
}
```

The following `docker-bake.hcl` file defines a `BASE_LATEST` variable that references the `BASE_IMAGE` variable.

​	以下 `docker-bake.hcl` 文件定义了一个引用 `BASE_IMAGE` 变量的 `BASE_LATEST` 变量。

docker-bake.hcl



```hcl
variable "BASE_LATEST" {
  default = "${BASE_IMAGE}:latest"
}

target "default" {
  contexts = {
    base = BASE_LATEST
  }
}
```

When you print the resolved build configuration, using the `-f` flag to specify the `vars.hcl` and `docker-bake.hcl` files, you see that the `BASE_LATEST` variable is resolved to `docker.io/library/alpine:latest`.

​	当您使用 `-f` 标志指定 `vars.hcl` 和 `docker-bake.hcl` 文件打印解析后的构建配置时，可以看到 `BASE_LATEST` 变量解析为 `docker.io/library/alpine:latest`。



```console
$ docker buildx bake -f vars.hcl -f docker-bake.hcl --print app
```



```json
{
  "target": {
    "default": {
      "context": ".",
      "contexts": {
        "base": "docker.io/library/alpine:latest"
      },
      "dockerfile": "Dockerfile"
    }
  }
}
```

## 其他资源 Additional resources

Here are some additional resources that show how you can use variables in Bake:

​	以下资源展示了如何在 Bake 中使用变量：

- You can override `variable` values using environment variables. See [Overriding configurations](https://docs.docker.com/build/bake/overrides/#environment-variables) for more information.
  - 您可以使用环境变量覆盖 `variable` 值。有关详细信息，请参阅 [覆盖配置](https://docs.docker.com/build/bake/overrides/#environment-variables)。
- You can refer to and use global variables in functions. See [HCL functions](https://docs.docker.com/build/bake/funcs/#variables-in-functions)
  - 您可以在函数中引用并使用全局变量。请参阅 [HCL 函数](https://docs.docker.com/build/bake/funcs/#variables-in-functions)。
- You can use variable values when evaluating expressions. See [Expression evaluation](https://docs.docker.com/build/bake/expressions/#expressions-with-variables)
  - 您可以在计算表达式时使用变量值。详见 [表达式计算](https://docs.docker.com/build/bake/expressions/#expressions-with-variables)。
