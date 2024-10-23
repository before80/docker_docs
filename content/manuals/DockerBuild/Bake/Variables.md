+++
title = "Variables"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/build/bake/variables/](https://docs.docker.com/build/bake/variables/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Variables in Bake

You can define and use variables in a Bake file to set attribute values, interpolate them into other values, and perform arithmetic operations. Variables can be defined with default values, and can be overridden with environment variables.

## [Using variables as attribute values](https://docs.docker.com/build/bake/variables/#using-variables-as-attribute-values)

Use the `variable` block to define a variable.



```hcl
variable "TAG" {
  default = "docker.io/username/webapp:latest"
}
```

The following example shows how to use the `TAG` variable in a target.



```hcl
target "default" {
  context = "."
  dockerfile = "Dockerfile"
  tags = [ TAG ]
}
```

## [Interpolate variables into values](https://docs.docker.com/build/bake/variables/#interpolate-variables-into-values)

Bake supports string interpolation of variables into values. You can use the `${}` syntax to interpolate a variable into a value. The following example defines a `TAG` variable with a value of `latest`.



```hcl
variable "TAG" {
  default = "latest"
}
```

To interpolate the `TAG` variable into the value of an attribute, use the `${TAG}` syntax.



```hcl
target "default" {
  context = "."
  dockerfile = "Dockerfile"
  tags = ["docker.io/username/webapp:${TAG}"]
}
```

Printing the Bake file with the `--print` flag shows the interpolated value in the resolved build configuration.



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

## [Escape variable interpolation](https://docs.docker.com/build/bake/variables/#escape-variable-interpolation)

If you want to bypass variable interpolation when parsing the Bake definition, use double dollar signs (`$${VARIABLE}`).



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

## [Using variables in variables across files](https://docs.docker.com/build/bake/variables/#using-variables-in-variables-across-files)

When multiple files are specified, one file can use variables defined in another file. In the following example, the `vars.hcl` file defines a `BASE_IMAGE` variable with a default value of `docker.io/library/alpine`.

vars.hcl



```hcl
variable "BASE_IMAGE" {
  default = "docker.io/library/alpine"
}
```

The following `docker-bake.hcl` file defines a `BASE_LATEST` variable that references the `BASE_IMAGE` variable.

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

## [Additional resources](https://docs.docker.com/build/bake/variables/#additional-resources)

Here are some additional resources that show how you can use variables in Bake:

- You can override `variable` values using environment variables. See [Overriding configurations](https://docs.docker.com/build/bake/overrides/#environment-variables) for more information.
- You can refer to and use global variables in functions. See [HCL functions](https://docs.docker.com/build/bake/funcs/#variables-in-functions)
- You can use variable values when evaluating expressions. See [Expression evaluation](https://docs.docker.com/build/bake/expressions/#expressions-with-variables)
