+++
title = "配置覆盖"
date = 2024-10-23T14:54:40+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/bake/overrides/](https://docs.docker.com/build/bake/overrides/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overriding configurations - 配置覆盖

Bake supports loading build definitions from files, but sometimes you need even more flexibility to configure these definitions. For example, you might want to override an attribute when building in a particular environment or for a specific target.

​	Bake 支持从文件加载构建定义，但有时您需要更灵活地配置这些定义。例如，您可能希望在特定环境或特定目标中覆盖某个属性。

The following list of attributes can be overridden:

​	以下属性列表可以被覆盖：

- `args`
- `cache-from`
- `cache-to`
- `context`
- `dockerfile`
- `labels`
- `no-cache`
- `output`
- `platform`
- `pull`
- `secrets`
- `ssh`
- `tags`
- `target`

To override these attributes, you can use the following methods:

​	要覆盖这些属性，可以使用以下方法：

- [File overrides](https://docs.docker.com/build/bake/overrides/#file-overrides)

- [CLI overrides](https://docs.docker.com/build/bake/overrides/#command-line)
- [Environment variable overrides](https://docs.docker.com/build/bake/overrides/#environment-variables)

## 文件覆盖 File overrides

You can load multiple Bake files that define build configurations for your targets. This is useful when you want to separate configurations into different files for better organization, or to conditionally override configurations based on which files are loaded.

​	您可以加载多个定义目标构建配置的 Bake 文件。这在您想将配置拆分到不同文件中以便更好组织，或基于加载的文件有条件地覆盖配置时非常有用。

### 默认文件查找 Default file lookup

You can use the `--file` or `-f` flag to specify which files to load. If you don't specify any files, Bake will use the following lookup order:

​	您可以使用 `--file` 或 `-f` 标志指定要加载的文件。如果您没有指定任何文件，Bake 将使用以下查找顺序：

1. `compose.yaml`
2. `compose.yml`
3. `docker-compose.yml`
4. `docker-compose.yaml`
5. `docker-bake.json`
6. `docker-bake.override.json`
7. `docker-bake.hcl`
8. `docker-bake.override.hcl`

If more than one Bake file is found, all files are loaded and merged into a single definition. Files are merged according to the lookup order.

​	如果找到多个 Bake 文件，所有文件都会被加载并合并为一个定义。文件会按照查找顺序进行合并。



```console
$ docker buildx bake bake --print
[+] Building 0.0s (1/1) FINISHED                                                                                                                                                                                            
 => [internal] load local bake definitions                                                                                                                                                                             0.0s
 => => reading compose.yaml 45B / 45B                                                                                                                                                                                  0.0s
 => => reading docker-bake.hcl 113B / 113B                                                                                                                                                                             0.0s
 => => reading docker-bake.override.hcl 65B / 65B
```

If merged files contain duplicate attribute definitions, those definitions are either merged or overridden by the last occurrence, depending on the attribute.

​	如果合并的文件包含重复的属性定义，根据属性的不同，这些定义会被合并或被最后出现的覆盖。

Bake will attempt to load all of the files in the order they are found. If multiple files define the same target, attributes are either merged or overridden. In the case of overrides, the last one loaded takes precedence.

For example, given the following files:

​	例如，以下文件：

docker-bake.hcl



```hcl
variable "TAG" {
  default = "foo"
}

target "default" {
  tags = ["username/my-app:${TAG}"]
}
```

docker-bake.override.hcl



```hcl
variable "TAG" {
  default = "bar"
}
```

Since `docker-bake.override.hcl` is loaded last in the default lookup order, the `TAG` variable is overridden with the value `bar`.

​	因为 `docker-bake.override.hcl` 在默认查找顺序中最后加载，所以 `TAG` 变量会被覆盖为 `bar`。



```console
$ docker buildx bake --print
{
  "target": {
    "default": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "tags": ["username/my-app:bar"]
    }
  }
}
```

### 手动文件覆盖 Manual file overrides

You can use the `--file` flag to explicitly specify which files to load, and use this as a way to conditionally apply override files.

​	您可以使用 `--file` 标志显式指定要加载的文件，并以此作为条件性应用覆盖文件的方法。

For example, you can create a file that defines a set of configurations for a specific environment, and load it only when building for that environment. The following example shows how to load an `override.hcl` file that sets the `TAG` variable to `bar`. The `TAG` variable is then used in the `default` target.

​	例如，您可以创建一个文件，为特定环境定义一组配置，并仅在为该环境构建时加载它。以下示例展示了如何加载一个 `override.hcl` 文件，将 `TAG` 变量设置为 `bar`。`TAG` 变量随后用于 `default` 目标。

docker-bake.hcl



```hcl
variable "TAG" {
  default = "foo"
}

target "default" {
  tags = ["username/my-app:${TAG}"]
}
```

overrides.hcl



```hcl
variable "TAG" {
  default = "bar"
}
```

Printing the build configuration without the `--file` flag shows the `TAG` variable is set to the default value `foo`.

​	不使用 `--file` 标志打印构建配置时，`TAG` 变量设置为默认值 `foo`。



```console
$ docker buildx bake --print
{
  "target": {
    "default": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "tags": [
        "username/my-app:foo"
      ]
    }
  }
}
```

Using the `--file` flag to load the `overrides.hcl` file overrides the `TAG` variable with the value `bar`.

​	使用 `--file` 标志加载 `overrides.hcl` 文件，将 `TAG` 变量覆盖为值 `bar`。



```console
$ docker buildx bake -f docker-bake.hcl -f overrides.hcl --print
{
  "target": {
    "default": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "tags": [
        "username/my-app:bar"
      ]
    }
  }
}
```

## 命令行 Command line

You can also override target configurations from the command line with the [`--set` flag](https://docs.docker.com/reference/cli/docker/buildx/bake/#set):

​	您也可以使用 [`--set` 标志](https://docs.docker.com/reference/cli/docker/buildx/bake/#set) 从命令行覆盖目标配置：



```hcl
# docker-bake.hcl
target "app" {
  args = {
    mybuildarg = "foo"
  }
}
```



```console
$ docker buildx bake --set app.args.mybuildarg=bar --set app.platform=linux/arm64 app --print
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
        "mybuildarg": "bar"
      },
      "platforms": ["linux/arm64"]
    }
  }
}
```

Pattern matching syntax defined in https://golang.org/pkg/path/#Match is also supported:

​	还支持在 https://golang.org/pkg/path/#Match 中定义的模式匹配语法：



```console
$ docker buildx bake --set foo*.args.mybuildarg=value  # overrides build arg for all targets starting with "foo" 覆盖所有以 "foo" 开头的目标的构建参数
$ docker buildx bake --set *.platform=linux/arm64      # overrides platform for all targets 覆盖所有目标的平台
$ docker buildx bake --set foo*.no-cache               # bypass caching only for targets starting with "foo" 仅对以 "foo" 开头的目标禁用缓存
```

Complete list of attributes that can be overridden with `--set` are:

​	使用 `--set` 可以覆盖的属性完整列表：

- `args`
- `cache-from`
- `cache-to`
- `context`
- `dockerfile`
- `labels`
- `no-cache`
- `output`
- `platform`
- `pull`
- `secrets`
- `ssh`
- `tags`
- `target`

## 环境变量 Environment variables

You can also use environment variables to override configurations.

​	您还可以使用环境变量覆盖配置。

Bake lets you use environment variables to override the value of a `variable` block. Only `variable` blocks can be overridden with environment variables. This means you need to define the variables in the bake file and then set the environment variable with the same name to override it.

​	Bake 允许您使用环境变量覆盖 `variable` 块的值。只有 `variable` 块可以通过环境变量覆盖。这意味着您需要在 bake 文件中定义变量，然后设置相同名称的环境变量来覆盖它。

The following example shows how you can define a `TAG` variable with a default value in the Bake file, and override it with an environment variable.

​	以下示例展示了如何在 Bake 文件中定义带有默认值的 `TAG` 变量，并使用环境变量覆盖它。



```hcl
variable "TAG" {
  default = "latest"
}

target "default" {
  context = "."
  dockerfile = "Dockerfile"
  tags = ["docker.io/username/webapp:${TAG}"]
}
```



```console
$ export TAG=$(git rev-parse --short HEAD)
$ docker buildx bake --print webapp
```

The `TAG` variable is overridden with the value of the environment variable, which is the short commit hash generated by `git rev-parse --short HEAD`.

​	`TAG` 变量被环境变量的值覆盖，该值是由 `git rev-parse --short HEAD` 生成的简短提交哈希。



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
      "tags": ["docker.io/username/webapp:985e9e9"]
    }
  }
}
```

### 类型强制 Type coercion

Overriding non-string variables with environment variables is supported. Values passed as environment variables are coerced into suitable types first.

​	使用环境变量覆盖非字符串变量是支持的。作为环境变量传递的值首先会强制转换为合适的类型。

The following example defines a `PORT` variable with a default value of `8080`. The `default` target uses a [ternary operator](https://docs.docker.com/build/bake/expressions/#ternary-operators) to set the `PORT` variable to the value of the environment variable `PORT` if it is greater than `1024`, otherwise it uses the default value.

​	以下示例定义了一个默认值为 `8080` 的 `PORT` 变量。`default` 目标使用一个[三元运算符](https://docs.docker.com/build/bake/expressions/#ternary-operators)将 `PORT` 变量设置为环境变量 `PORT` 的值（如果其大于 `1024`），否则使用默认值。

In this case, the `PORT` variable is coerced to an integer before the ternary operator is evaluated.

​	在这种情况下，`PORT` 变量在三元运算符求值前被强制转换为整数。



```hcl
default_port = 8080

variable "PORT" {
  default = default_port
}

target "default" {
  args = {
    PORT = PORT > 1024 ? PORT : default_port
  }
}
```

Attempting to set the `PORT` variable with a value less than `1024` will result in the default value being used.

​	尝试设置小于 `1024` 的 `PORT` 值将导致使用默认值。



```console
$ PORT=80 docker buildx bake --print
```



```json
{
  "target": {
    "default": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "args": {
        "PORT": "8080"
      }
    }
  }
}
```
