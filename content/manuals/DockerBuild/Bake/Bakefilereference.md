+++
title = "Bake file reference"
date = 2024-10-23T14:54:40+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/bake/reference/](https://docs.docker.com/build/bake/reference/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Bake file reference - Bake 文件参考

The Bake file is a file for defining workflows that you run using `docker buildx bake`.

​	Bake 文件用于定义通过 `docker buildx bake` 运行的工作流。

## 文件格式 File format

You can define your Bake file in the following file formats:

​	您可以使用以下文件格式定义 Bake 文件：

- HashiCorp Configuration Language (HCL)
  - HashiCorp 配置语言（HCL）

- JSON
- YAML (Compose file)

By default, Bake uses the following lookup order to find the configuration file:

​	默认情况下，Bake 按以下顺序查找配置文件：

1. `compose.yaml`
2. `compose.yml`
3. `docker-compose.yml`
4. `docker-compose.yaml`
5. `docker-bake.json`
6. `docker-bake.override.json`
7. `docker-bake.hcl`
8. `docker-bake.override.hcl`

You can specify the file location explicitly using the `--file` flag:

​	您可以使用 `--file` 标志显式指定文件位置：

```console
$ docker buildx bake --file ../docker/bake.hcl --print
```

If you don't specify a file explicitly, Bake searches for the file in the current working directory. If more than one Bake file is found, all files are merged into a single definition. Files are merged according to the lookup order. That means that if your project contains both a `compose.yaml` file and a `docker-bake.hcl` file, Bake loads the `compose.yaml` file first, and then the `docker-bake.hcl` file.

​	如果未显式指定文件，Bake 会在当前工作目录中查找文件。如果找到多个 Bake 文件，所有文件将合并为一个定义，按查找顺序合并。例如，如果项目包含 `compose.yaml` 文件和 `docker-bake.hcl` 文件，Bake 会首先加载 `compose.yaml` 文件，然后是 `docker-bake.hcl` 文件。

If merged files contain duplicate attribute definitions, those definitions are either merged or overridden by the last occurrence, depending on the attribute. The following attributes are overridden by the last occurrence:

​	若合并的文件包含重复的属性定义，则视属性而定，要么合并，要么由最后出现的定义覆盖。以下属性由最后的定义覆盖：

- `target.cache-to`
- `target.dockerfile-inline`
- `target.dockerfile`
- `target.outputs`
- `target.platforms`
- `target.pull`
- `target.tags`
- `target.target`

For example, if `compose.yaml` and `docker-bake.hcl` both define the `tags` attribute, the `docker-bake.hcl` is used.

​	例如，如果 `compose.yaml` 和 `docker-bake.hcl` 都定义了 `tags` 属性，则使用 `docker-bake.hcl` 中的定义。

```console
$ cat compose.yaml
services:
  webapp:
    build:
      context: .
      tags:
        - bar
$ cat docker-bake.hcl
target "webapp" {
  tags = ["foo"]
}
$ docker buildx bake --print webapp
{
  "group": {
    "default": {
      "targets": [
        "webapp"
      ]
    }
  },
  "target": {
    "webapp": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "tags": [
        "foo"
      ]
    }
  }
}
```

All other attributes are merged. For example, if `compose.yaml` and `docker-bake.hcl` both define unique entries for the `labels` attribute, all entries are included. Duplicate entries for the same label are overridden.

​	所有其他属性将被合并。例如，如果 `compose.yaml` 和 `docker-bake.hcl` 都定义了 `labels` 属性的唯一条目，则所有条目都会被包含。同一标签的重复条目将被覆盖。

```console
$ cat compose.yaml
services:
  webapp:
    build:
      context: .
      labels: 
        com.example.foo: "foo"
        com.example.name: "Alice"
$ cat docker-bake.hcl
target "webapp" {
  labels = {
    "com.example.bar" = "bar"
    "com.example.name" = "Bob"
  }
}
$ docker buildx bake --print webapp
{
  "group": {
    "default": {
      "targets": [
        "webapp"
      ]
    }
  },
  "target": {
    "webapp": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "labels": {
        "com.example.foo": "foo",
        "com.example.bar": "bar",
        "com.example.name": "Bob"
      }
    }
  }
}
```

## 语法 Syntax

The Bake file supports the following property types:

​	Bake 文件支持以下属性类型：

- `target`: build targets 构建目标
- `group`: collections of build targets 构建目标的集合
- `variable`: build arguments and variables 构建参数和变量
- `function`: custom Bake functions 自定义 Bake 函数

You define properties as hierarchical blocks in the Bake file. You can assign one or more attributes to a property.

​	您可以在 Bake 文件中将属性定义为层次结构块，并为属性分配一个或多个属性。

The following snippet shows a JSON representation of a simple Bake file. This Bake file defines three properties: a variable, a group, and a target.

​	以下是一个 Bake 文件的简单 JSON 表示示例，其中定义了一个变量、一个组和一个目标：

```json
{
  "variable": {
    "TAG": {
      "default": "latest"
    }
  },
  "group": {
    "default": {
      "targets": ["webapp"]
    }
  },
  "target": {
    "webapp": {
      "dockerfile": "Dockerfile",
      "tags": ["docker.io/username/webapp:${TAG}"]
    }
  }
}
```

In the JSON representation of a Bake file, properties are objects, and attributes are values assigned to those objects.

​	在 Bake 文件的 JSON 表示中，属性为对象，属性值是分配给这些对象的值。

The following example shows the same Bake file in the HCL format:

​	以下示例展示了相同的 Bake 文件，采用 HCL 格式：

```hcl
variable "TAG" {
  default = "latest"
}

group "default" {
  targets = ["webapp"]
}

target "webapp" {
  dockerfile = "Dockerfile"
  tags = ["docker.io/username/webapp:${TAG}"]
}
```

HCL is the preferred format for Bake files. Aside from syntactic differences, HCL lets you use features that the JSON and YAML formats don't support.

​	HCL 是 Bake 文件的首选格式。除了语法差异外，HCL 还允许使用 JSON 和 YAML 格式不支持的功能。

The examples in this document use the HCL format.

​	文档中的示例使用 HCL 格式。

## Target

A target reflects a single `docker build` invocation. Consider the following build command:

​	目标表示单个 `docker build` 调用。考虑以下构建命令：



```console
$ docker build \
  --file=Dockerfile.webapp \
  --tag=docker.io/username/webapp:latest \
  https://github.com/username/webapp
```

You can express this command in a Bake file as follows:

​	可以在 Bake 文件中如下表示此命令：



```hcl
target "webapp" {
  dockerfile = "Dockerfile.webapp"
  tags = ["docker.io/username/webapp:latest"]
  context = "https://github.com/username/webapp"
}
```

The following table shows the complete list of attributes that you can assign to a target:

​	以下表格显示了可以分配给目标的完整属性列表：

| Name                                                         | Type    | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`args`](https://docs.docker.com/build/bake/reference/#targetargs) | Map     | 构建参数 Build arguments                                     |
| [`annotations`](https://docs.docker.com/build/bake/reference/#targetannotations) | List    | 导出器注解 Exporter annotations                              |
| [`attest`](https://docs.docker.com/build/bake/reference/#targetattest) | List    | 构建声明 Build attestations                                  |
| [`cache-from`](https://docs.docker.com/build/bake/reference/#targetcache-from) | List    | 外部缓存源 External cache sources                            |
| [`cache-to`](https://docs.docker.com/build/bake/reference/#targetcache-to) | List    | 外部缓存目标 External cache destinations                     |
| [`context`](https://docs.docker.com/build/bake/reference/#targetcontext) | String  | 文件集位置或 URL Set of files located in the specified path or URL |
| [`contexts`](https://docs.docker.com/build/bake/reference/#targetcontexts) | Map     | 额外的构建上下文 Additional build contexts                   |
| [`dockerfile-inline`](https://docs.docker.com/build/bake/reference/#targetdockerfile-inline) | String  | 内联 Dockerfile 字符串 Inline Dockerfile string              |
| [`dockerfile`](https://docs.docker.com/build/bake/reference/#targetdockerfile) | String  | Dockerfile 位置 Dockerfile location                          |
| [`inherits`](https://docs.docker.com/build/bake/reference/#targetinherits) | List    | 从其他目标继承属性 Inherit attributes from other targets     |
| [`labels`](https://docs.docker.com/build/bake/reference/#targetlabels) | Map     | 图像元数据 Metadata for images                               |
| [`matrix`](https://docs.docker.com/build/bake/reference/#targetmatrix) | Map     | 定义变量集合，将目标分叉为多个目标 Define a set of variables that forks a target into multiple targets. |
| [`name`](https://docs.docker.com/build/bake/reference/#targetname) | String  | 使用矩阵时覆盖目标名称 Override the target name when using a matrix. |
| [`no-cache-filter`](https://docs.docker.com/build/bake/reference/#targetno-cache-filter) | List    | 禁用特定阶段的构建缓存 Disable build cache for specific stages |
| [`no-cache`](https://docs.docker.com/build/bake/reference/#targetno-cache) | Boolean | 完全禁用构建缓存 Disable build cache completely              |
| [`output`](https://docs.docker.com/build/bake/reference/#targetoutput) | List    | 输出目标 Output destinations                                 |
| [`platforms`](https://docs.docker.com/build/bake/reference/#targetplatforms) | List    | 目标平台 Target platforms                                    |
| [`pull`](https://docs.docker.com/build/bake/reference/#targetpull) | Boolean | 始终拉取图像 Always pull images                              |
| [`secret`](https://docs.docker.com/build/bake/reference/#targetsecret) | List    | 向构建中暴露的机密 Secrets to expose to the build            |
| [`shm-size`](https://docs.docker.com/build/bake/reference/#targetshm-size) | List    | `/dev/shm` 的大小 Size of `/dev/shm`                         |
| [`ssh`](https://docs.docker.com/build/bake/reference/#targetssh) | List    | 暴露给构建的 SSH 代理套接字或密钥 SSH agent sockets or keys to expose to the build |
| [`tags`](https://docs.docker.com/build/bake/reference/#targettags) | List    | 镜像名称和标签 Image names and tags                          |
| [`target`](https://docs.docker.com/build/bake/reference/#targettarget) | String  | 目标构建阶段 Target build stage                              |
| [`ulimits`](https://docs.docker.com/build/bake/reference/#targetulimits) | List    | Ulimit 选项 Ulimit options                                   |

### `target.args`

Use the `args` attribute to define build arguments for the target. This has the same effect as passing a [`--build-arg`](https://docs.docker.com/reference/cli/docker/image/build/#build-arg) flag to the build command.

​	使用 `args` 属性定义目标的构建参数。这与将 [`--build-arg`](https://docs.docker.com/reference/cli/docker/image/build/#build-arg) 标志传递给构建命令效果相同。

```hcl
target "default" {
  args = {
    VERSION = "0.0.0+unknown"
  }
}
```

You can set `args` attributes to use `null` values. Doing so forces the `target` to use the `ARG` value specified in the Dockerfile.

​	您可以将 `args` 属性设置为 `null` 值，这样会强制 `target` 使用 Dockerfile 中指定的 `ARG` 值。



```hcl
variable "GO_VERSION" {
  default = "1.20.3"
}

target "webapp" {
  dockerfile = "webapp.Dockerfile"
  tags = ["docker.io/username/webapp"]
}

target "db" {
  args = {
    GO_VERSION = null
  }
  dockerfile = "db.Dockerfile"
  tags = ["docker.io/username/db"]
}
```

### `target.annotations`

The `annotations` attribute lets you add annotations to images built with bake. The key takes a list of annotations, in the format of `KEY=VALUE`.

​	`annotations` 属性允许为使用 bake 构建的镜像添加注解。该键接收注解列表，格式为 `KEY=VALUE`。



```hcl
target "default" {
  output = ["type=image,name=foo"]
  annotations = ["org.opencontainers.image.authors=dvdksn"]
}
```

is the same as

等同于

```hcl
target "default" {
  output = ["type=image,name=foo,annotation.org.opencontainers.image.authors=dvdksn"]
}
```

By default, the annotation is added to image manifests. You can configure the level of the annotations by adding a prefix to the annotation, containing a comma-separated list of all the levels that you want to annotate. The following example adds annotations to both the image index and manifests.

​	默认情况下，注解会添加到镜像清单中。您可以通过在注解前添加前缀来配置注解级别，其中包含要注解的所有级别的逗号分隔列表。以下示例将注解添加到镜像索引和清单中。



```hcl
target "default" {
  output = ["type=image,name=foo"]
  annotations = ["index,manifest:org.opencontainers.image.authors=dvdksn"]
}
```

Read about the supported levels in [Specifying annotation levels](https://docs.docker.com/build/building/annotations/#specifying-annotation-levels).

​	请阅读 [指定注解级别](https://docs.docker.com/build/building/annotations/#specifying-annotation-levels) 了解支持的级别。

### `target.attest`

The `attest` attribute lets you apply [build attestations](https://docs.docker.com/build/attestations/) to the target. This attribute accepts the long-form CSV version of attestation parameters.

​	`attest` 属性允许对目标应用 [构建声明](https://docs.docker.com/build/attestations/)。此属性接受声明参数的长格式 CSV 版本。



```hcl
target "default" {
  attest = [
    "type=provenance,mode=min",
    "type=sbom"
  ]
}
```

### `target.cache-from`

Build cache sources. The builder imports cache from the locations you specify. It uses the [Buildx cache storage backends]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}}), and it works the same way as the [`--cache-from`](https://docs.docker.com/reference/cli/docker/buildx/build/#cache-from) flag. This takes a list value, so you can specify multiple cache sources.

​	构建缓存源。生成器从您指定的位置导入缓存。它使用 [Buildx 缓存存储后端]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}})，与 [`--cache-from`](https://docs.docker.com/reference/cli/docker/buildx/build/#cache-from) 标志的工作方式相同。此属性接受列表值，因此您可以指定多个缓存源。



```hcl
target "app" {
  cache-from = [
    "type=s3,region=eu-west-1,bucket=mybucket",
    "user/repo:cache",
  ]
}
```

### `target.cache-to`

Build cache export destinations. The builder exports its build cache to the locations you specify. It uses the [Buildx cache storage backends]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}}), and it works the same way as the [`--cache-to` flag](https://docs.docker.com/reference/cli/docker/buildx/build/#cache-to). This takes a list value, so you can specify multiple cache export targets.

​	构建缓存导出目标。生成器将其构建缓存导出到您指定的位置。它使用 [Buildx 缓存存储后端]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}})，与 [`--cache-to`](https://docs.docker.com/reference/cli/docker/buildx/build/#cache-to) 标志的工作方式相同。此属性接受列表值，因此您可以指定多个缓存导出目标。



```hcl
target "app" {
  cache-to = [
    "type=s3,region=eu-west-1,bucket=mybucket",
    "type=inline"
  ]
}
```

### `target.context`

Specifies the location of the build context to use for this target. Accepts a URL or a directory path. This is the same as the [build context](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context) positional argument that you pass to the build command.

​	指定用于此目标的构建上下文的位置。接受 URL 或目录路径。这与您传递给构建命令的 [构建上下文](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context) 位置参数相同。



```hcl
target "app" {
  context = "./src/www"
}
```

This resolves to the current working directory (`"."`) by default.

​	默认解析为当前工作目录 (`"."`)。



```console
$ docker buildx bake --print -f - <<< 'target "default" {}'
[+] Building 0.0s (0/0)
{
  "target": {
    "default": {
      "context": ".",
      "dockerfile": "Dockerfile"
    }
  }
}
```

### `target.contexts`

Additional build contexts. This is the same as the [`--build-context` flag](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context). This attribute takes a map, where keys result in named contexts that you can reference in your builds.

​	额外的构建上下文。这与 [`--build-context` 标志](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context) 相同。此属性接受一个映射，其中键将导致命名上下文，您可以在构建中引用。

You can specify different types of contexts, such local directories, Git URLs, and even other Bake targets. Bake automatically determines the type of a context based on the pattern of the context value.

​	您可以指定不同类型的上下文，例如本地目录、Git URL，甚至其他 Bake 目标。Bake 会根据上下文值的模式自动确定上下文类型。

| Context type             | Example                                   |
| ------------------------ | ----------------------------------------- |
| Container image 容器镜像 | `docker-image://alpine@sha256:0123456789` |
| Git URL                  | `https://github.com/user/proj.git`        |
| HTTP URL                 | `https://example.com/files`               |
| Local directory 本地目录 | `../path/to/src`                          |
| Bake target              | `target:base`                             |

#### 固定镜像版本 Pin an image version



```hcl
# docker-bake.hcl
target "app" {
    contexts = {
        alpine = "docker-image://alpine:3.13"
    }
}
```



```Dockerfile
# Dockerfile
FROM alpine
RUN echo "Hello world"
```

#### 使用本地目录 Use a local directory



```hcl
# docker-bake.hcl
target "app" {
    contexts = {
        src = "../path/to/source"
    }
}
```



```Dockerfile
# Dockerfile
FROM scratch AS src
FROM golang
COPY --from=src . .
```

#### 使用另一个目标作为基础 Use another target as base

> **Note**
>
> You should prefer to use regular multi-stage builds over this option. You can Use this feature when you have multiple Dockerfiles that can't be easily merged into one.
>
> ​	您应该优先使用常规的多阶段构建而不是此选项。当您有多个无法轻松合并为一个的 Dockerfile 时，可以使用此功能。



```hcl
# docker-bake.hcl
target "base" {
    dockerfile = "baseapp.Dockerfile"
}
target "app" {
    contexts = {
        baseapp = "target:base"
    }
}
```



```Dockerfile
# Dockerfile
FROM baseapp
RUN echo "Hello world"
```

### `target.dockerfile-inline`

Uses the string value as an inline Dockerfile for the build target.

​	将字符串值用作构建目标的内联 Dockerfile。

```hcl
target "default" {
  dockerfile-inline = "FROM alpine\nENTRYPOINT [\"echo\", \"hello\"]"
}
```

The `dockerfile-inline` takes precedence over the `dockerfile` attribute. If you specify both, Bake uses the inline version.

​	`dockerfile-inline` 优先于 `dockerfile` 属性。如果同时指定，Bake 使用内联版本。

### `target.dockerfile`

Name of the Dockerfile to use for the build. This is the same as the [`--file` flag](https://docs.docker.com/reference/cli/docker/image/build/#file) for the `docker build` command.

​	用于构建的 Dockerfile 名称。这与 `docker build` 命令的 [`--file` 标志](https://docs.docker.com/reference/cli/docker/image/build/#file) 相同。

```hcl
target "default" {
  dockerfile = "./src/www/Dockerfile"
}
```

Resolves to `"Dockerfile"` by default.

​	默认为 `"Dockerfile"`。

```console
$ docker buildx bake --print -f - <<< 'target "default" {}'
[+] Building 0.0s (0/0)
{
  "target": {
    "default": {
      "context": ".",
      "dockerfile": "Dockerfile"
    }
  }
}
```

### `target.entitlements`

Entitlements are permissions that the build process requires to run.

​	授权是构建过程运行所需的权限。

Currently supported entitlements are:

​	当前支持的授权有：

- `network.host`: Allows the build to use commands that access the host network. In Dockerfile, use [`RUN --network=host`](https://docs.docker.com/reference/dockerfile/#run---networkhost) to run a command with host network enabled.
  - `network.host`：允许构建使用访问主机网络的命令。在 Dockerfile 中，使用 [`RUN --network=host`](https://docs.docker.com/reference/dockerfile/#run---networkhost) 以主机网络启用运行命令。

- `security.insecure`: Allows the build to run commands in privileged containers that are not limited by the default security sandbox. Such container may potentially access and modify system resources. In Dockerfile, use [`RUN --security=insecure`](https://docs.docker.com/reference/dockerfile/#run---security) to run a command in a privileged container.
  - `security.insecure`：允许构建在特权容器中运行，特权容器不受默认安全沙箱的限制，可能访问和修改系统资源。在 Dockerfile 中，使用 [`RUN --security=insecure`](https://docs.docker.com/reference/dockerfile/#run---security) 在特权容器中运行命令。




```hcl
target "integration-tests" {
  # this target requires privileged containers to run nested containers
  # 此目标需要特权容器以运行嵌套容器
  entitlements = ["security.insecure"]
}
```

Entitlements are enabled with a two-step process. First, a target must declare the entitlements it requires. Secondly, when invoking the `bake` command, the user must grant the entitlements by passing the `--allow` flag or confirming the entitlements when prompted in an interactive terminal. This is to ensure that the user is aware of the possibly insecure permissions they are granting to the build process.

​	授权通过两步过程启用。首先，目标必须声明其需要的授权。其次，在调用 `bake` 命令时，用户必须通过传递 `--allow` 标志或在交互式终端中确认授权，从而确保用户了解授予构建过程的可能不安全权限。

### `target.inherits`

A target can inherit attributes from other targets. Use `inherits` to reference from one target to another.

​	目标可以从其他目标继承属性。使用 `inherits` 以从一个目标引用另一个目标。

In the following example, the `app-dev` target specifies an image name and tag. The `app-release` target uses `inherits` to reuse the tag name.

​	在以下示例中，`app-dev` 目标指定了一个镜像名称和标签。`app-release` 目标使用 `inherits` 重新使用标签名称。

```hcl
variable "TAG" {
  default = "latest"
}

target "app-dev" {
  tags = ["docker.io/username/myapp:${TAG}"]
}

target "app-release" {
  inherits = ["app-dev"]
  platforms = ["linux/amd64", "linux/arm64"]
}
```

The `inherits` attribute is a list, meaning you can reuse attributes from multiple other targets. In the following example, the `app-release` target reuses attributes from both the `app-dev` and `_release` targets.

​	`inherits` 属性是一个列表，意味着您可以重用多个其他目标的属性。在以下示例中，`app-release` 目标重用了 `app-dev` 和 `_release` 目标的属性。

```hcl
target "app-dev" {
  args = {
    GO_VERSION = "1.20"
    BUILDX_EXPERIMENTAL = 1
  }
  tags = ["docker.io/username/myapp"]
  dockerfile = "app.Dockerfile"
  labels = {
    "org.opencontainers.image.source" = "https://github.com/username/myapp"
  }
}

target "_release" {
  args = {
    BUILDKIT_CONTEXT_KEEP_GIT_DIR = 1
    BUILDX_EXPERIMENTAL = 0
  }
}

target "app-release" {
  inherits = ["app-dev", "_release"]
  platforms = ["linux/amd64", "linux/arm64"]
}
```

When inheriting attributes from multiple targets and there's a conflict, the target that appears last in the `inherits` list takes precedence. The previous example defines the `BUILDX_EXPERIMENTAL` argument twice for the `app-release` target. It resolves to `0` because the `_release` target appears last in the inheritance chain:

​	当从多个目标继承属性且存在冲突时，`inherits` 列表中最后出现的目标优先。上例为 `app-release` 目标定义了两次 `BUILDX_EXPERIMENTAL` 参数。由于 `_release` 目标在继承链中最后出现，因此该参数最终解析为 `0`。

```console
$ docker buildx bake --print app-release
[+] Building 0.0s (0/0)
{
  "group": {
    "default": {
      "targets": [
        "app-release"
      ]
    }
  },
  "target": {
    "app-release": {
      "context": ".",
      "dockerfile": "app.Dockerfile",
      "args": {
        "BUILDKIT_CONTEXT_KEEP_GIT_DIR": "1",
        "BUILDX_EXPERIMENTAL": "0",
        "GO_VERSION": "1.20"
      },
      "labels": {
        "org.opencontainers.image.source": "https://github.com/username/myapp"
      },
      "tags": [
        "docker.io/username/myapp"
      ],
      "platforms": [
        "linux/amd64",
        "linux/arm64"
      ]
    }
  }
}
```

### `target.labels`

Assigns image labels to the build. This is the same as the `--label` flag for `docker build`.

​	分配镜像标签到构建。这与 `docker build` 的 `--label` 标志相同。

```hcl
target "default" {
  labels = {
    "org.opencontainers.image.source" = "https://github.com/username/myapp"
    "com.docker.image.source.entrypoint" = "Dockerfile"
  }
}
```

It's possible to use a `null` value for labels. If you do, the builder uses the label value specified in the Dockerfile.

​	可以为标签使用 `null` 值，如果这样做，构建器将使用 Dockerfile 中指定的标签值。

### `target.matrix`

A matrix strategy lets you fork a single target into multiple different variants, based on parameters that you specify. This works in a similar way to [Matrix strategies for GitHub Actions]. You can use this to reduce duplication in your bake definition.

​	矩阵策略允许您基于指定的参数将单个目标分叉为多个不同的变体。这与 [GitHub Actions 的矩阵策略](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs)类似，您可以使用它减少在 Bake 定义中的重复。

The `matrix` attribute is a map of parameter names to lists of values. Bake builds each possible combination of values as a separate target.

​	`matrix` 属性是参数名到值列表的映射。Bake 将每个可能的值组合构建为单独的目标。

Each generated target **must** have a unique name. To specify how target names should resolve, use the `name` attribute.

​	每个生成的目标**必须**有一个唯一的名称。使用 `name` 属性指定目标名称的解析方式。

The following example resolves the `app` target to `app-foo` and `app-bar`. It also uses the matrix value to define the [target build stage](https://docs.docker.com/build/bake/reference/#targettarget).

​	以下示例将 `app` 目标解析为 `app-foo` 和 `app-bar`。它还使用矩阵值定义了[构建阶段](https://docs.docker.com/build/bake/reference/#targettarget)。



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

#### 多个轴 Multiple axes

You can specify multiple keys in your matrix to fork a target on multiple axes. When using multiple matrix keys, Bake builds every possible variant.

​	可以在矩阵中指定多个键，将目标分叉为多个轴。使用多个矩阵键时，Bake 将构建每个可能的变体。

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

#### 每个矩阵目标的多个值 Multiple values per matrix target

If you want to differentiate the matrix on more than just a single value, you can use maps as matrix values. Bake creates a target for each map, and you can access the nested values using dot notation.

​	如果要在不止一个单独值上区分矩阵，可以使用映射作为矩阵值。Bake 为每个映射创建一个目标，您可以使用点符号访问嵌套值。

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

### `target.name`

Specify name resolution for targets that use a matrix strategy. The following example resolves the `app` target to `app-foo` and `app-bar`.

​	为使用矩阵策略的目标指定名称解析。以下示例将 `app` 目标解析为 `app-foo` 和 `app-bar`。



```hcl
target "app" {
  name = "app-${tgt}"
  matrix = {
    tgt = ["foo", "bar"]
  }
  target = tgt
}
```

### `target.network`

Specify the network mode for the whole build request. This will override the default network mode for all the `RUN` instructions in the Dockerfile. Accepted values are `default`, `host`, and `none`.

​	为整个构建请求指定网络模式。这会覆盖所有 `RUN` 指令在 Dockerfile 中的默认网络模式。接受的值为 `default`、`host` 和 `none`。

Usually, a better approach to set the network mode for your build steps is to instead use `RUN --network=<value>` in your Dockerfile. This way, you can set the network mode for individual build steps and everyone building the Dockerfile gets consistent behavior without needing to pass additional flags to the build command.

​	通常，设置构建步骤的网络模式的更好方法是直接在 Dockerfile 中使用 `RUN --network=<value>`，这样可以为每个构建步骤设置一致的网络模式，而无需为构建命令传递额外的标志。

If you set network mode to `host` in your Bake file, you must also grant `network.host` entitlement when invoking the `bake` command. This is because `host` network mode requires elevated privileges and can be a security risk. You can pass `--allow=network.host` to the `docker buildx bake` command to grant the entitlement, or you can confirm the entitlement when prompted if you are using an interactive terminal.

​	如果在 Bake 文件中将网络模式设置为 `host`，则必须在执行 `bake` 命令时授予 `network.host` 授权，因为 `host` 网络模式需要提升权限，可能存在安全风险。可以通过向 `docker buildx bake` 命令传递 `--allow=network.host` 授权，或者在交互式终端中确认授权。



```hcl
target "app" {
  # make sure this build does not access internet
  network = "none"
}
```

### `target.no-cache-filter`

Don't use build cache for the specified stages. This is the same as the `--no-cache-filter` flag for `docker build`. The following example avoids build cache for the `foo` build stage.

​	不为指定的阶段使用构建缓存。这与 `docker build` 的 `--no-cache-filter` 标志相同。以下示例避免了 `foo` 构建阶段的构建缓存。



```hcl
target "default" {
  no-cache-filter = ["foo"]
}
```

### `target.no-cache`

Don't use cache when building the image. This is the same as the `--no-cache` flag for `docker build`.

​	在构建镜像时不使用缓存。这与 `docker build` 的 `--no-cache` 标志相同。



```hcl
target "default" {
  no-cache = 1
}
```

### `target.output`

Configuration for exporting the build output. This is the same as the [`--output` flag](https://docs.docker.com/reference/cli/docker/buildx/build/#output). The following example configures the target to use a cache-only output,

​	导出构建输出的配置。这与 [`--output` 标志](https://docs.docker.com/reference/cli/docker/buildx/build/#output) 相同。以下示例将目标配置为仅使用缓存输出。



```hcl
target "default" {
  output = ["type=cacheonly"]
}
```

### `target.platforms`

Set target platforms for the build target. This is the same as the [`--platform` flag](https://docs.docker.com/reference/cli/docker/buildx/build/#platform). The following example creates a multi-platform build for three architectures.

​	设置构建目标的目标平台。这与 [`--platform` 标志](https://docs.docker.com/reference/cli/docker/buildx/build/#platform) 相同。以下示例为三种架构创建了多平台构建。



```hcl
target "default" {
  platforms = ["linux/amd64", "linux/arm64", "linux/arm/v7"]
}
```

### `target.pull`

Configures whether the builder should attempt to pull images when building the target. This is the same as the `--pull` flag for `docker build`. The following example forces the builder to always pull all images referenced in the build target.

​	配置构建器是否应在构建目标时尝试拉取镜像。这与 `docker build` 的 `--pull` 标志相同。以下示例强制构建器始终拉取构建目标中引用的所有镜像。



```hcl
target "default" {
  pull = "always"
}
```

### `target.secret`

Defines secrets to expose to the build target. This is the same as the [`--secret` flag](https://docs.docker.com/reference/cli/docker/buildx/build/#secret).

​	定义要公开给构建目标的密钥。这与 [`--secret` 标志](https://docs.docker.com/reference/cli/docker/buildx/build/#secret) 相同。



```hcl
variable "HOME" {
  default = null
}

target "default" {
  secret = [
    "type=env,id=KUBECONFIG",
    "type=file,id=aws,src=${HOME}/.aws/credentials"
  ]
}
```

This lets you [mount the secret](https://docs.docker.com/reference/dockerfile/#run---mounttypesecret) in your Dockerfile.

​	这允许您在 Dockerfile 中[挂载密钥](https://docs.docker.com/reference/dockerfile/#run---mounttypesecret)。



```dockerfile
RUN --mount=type=secret,id=aws,target=/root/.aws/credentials \
    aws cloudfront create-invalidation ...
RUN --mount=type=secret,id=KUBECONFIG,env=KUBECONFIG \
    helm upgrade --install
```

### `target.shm-size`

Sets the size of the shared memory allocated for build containers when using `RUN` instructions.

​	设置在使用 `RUN` 指令时为构建容器分配的共享内存大小。

The format is `<number><unit>`. `number` must be greater than `0`. Unit is optional and can be `b` (bytes), `k` (kilobytes), `m` (megabytes), or `g` (gigabytes). If you omit the unit, the system uses bytes.

​	格式为 `<number><unit>`。`number` 必须大于 `0`。单位是可选的，可以是 `b`（字节）、`k`（千字节）、`m`（兆字节）或 `g`（千兆字节）。如果省略单位，则系统使用字节。

This is the same as the `--shm-size` flag for `docker build`.

​	这与 `docker build` 的 `--shm-size` 标志相同。



```hcl
target "default" {
  shm-size = "128m"
}
```

> **Note**
>
> In most cases, it is recommended to let the builder automatically determine the appropriate configurations. Manual adjustments should only be considered when specific performance tuning is required for complex build scenarios.
>
> ​	在大多数情况下，建议让构建器自动确定适当的配置。仅在复杂构建场景中需要特定性能调优时考虑手动调整。

### `target.ssh`

Defines SSH agent sockets or keys to expose to the build. This is the same as the [`--ssh` flag](https://docs.docker.com/reference/cli/docker/buildx/build/#ssh). This can be useful if you need to access private repositories during a build.

​	定义要公开给构建的 SSH 代理套接字或密钥。这与 [`--ssh` 标志](https://docs.docker.com/reference/cli/docker/buildx/build/#ssh) 相同。如果在构建期间需要访问私有存储库，这将很有用。



```hcl
target "default" {
  ssh = ["default"]
}
```



```dockerfile
FROM alpine
RUN --mount=type=ssh \
    apk add git openssh-client \
    && install -m 0700 -d ~/.ssh \
    && ssh-keyscan github.com >> ~/.ssh/known_hosts \
    && git clone git@github.com:user/my-private-repo.git
```

### `target.tags`

Image names and tags to use for the build target. This is the same as the [`--tag` flag](https://docs.docker.com/reference/cli/docker/image/build/#tag).

​	为构建目标使用的镜像名称和标签。这与 [`--tag` 标志](https://docs.docker.com/reference/cli/docker/image/build/#tag) 相同。



```hcl
target "default" {
  tags = [
    "org/repo:latest",
    "myregistry.azurecr.io/team/image:v1"
  ]
}
```

### `target.target`

Set the target build stage to build. This is the same as the [`--target` flag](https://docs.docker.com/reference/cli/docker/image/build/#target).

​	将目标构建阶段设置为要构建的阶段。此设置与 [`--target` 参数](https://docs.docker.com/reference/cli/docker/image/build/#target) 相同。



```hcl
target "default" {
  target = "binaries"
}
```

### `target.ulimits`

Ulimits overrides the default ulimits of build's containers when using `RUN` instructions and are specified with a soft and hard limit as such: `<type>=<soft limit>[:<hard limit>]`, for example:

​	Ulimits 允许在使用 `RUN` 指令时覆盖构建容器的默认 ulimit，并通过软限制和硬限制来指定，例如：`<type>=<soft limit>[:<hard limit>]`。示例如下：



```hcl
target "app" {
  ulimits = [
    "nofile=1024:1024"
  ]
}
```

> **Note**
>
> If you do not provide a `hard limit`, the `soft limit` is used for both values. If no `ulimits` are set, they are inherited from the default `ulimits` set on the daemon.
>
> ​	如果未提供 `hard limit`，则 `soft limit` 会同时用于软限制和硬限制。如果没有设置 `ulimits`，则将从守护进程的默认 `ulimits` 继承。

> **Note**
>
> In most cases, it is recommended to let the builder automatically determine the appropriate configurations. Manual adjustments should only be considered when specific performance tuning is required for complex build scenarios.
>
> ​	在大多数情况下，建议让构建器自动确定合适的配置。只有在复杂构建场景需要特定性能优化时才应考虑手动调整。

## Group

Groups allow you to invoke multiple builds (targets) at once.

​	组允许您一次调用多个构建（目标）。

```hcl
group "default" {
  targets = ["db", "webapp-dev"]
}

target "webapp-dev" {
  dockerfile = "Dockerfile.webapp"
  tags = ["docker.io/username/webapp:latest"]
}

target "db" {
  dockerfile = "Dockerfile.db"
  tags = ["docker.io/username/db"]
}
```

Groups take precedence over targets, if both exist with the same name. The following bake file builds the `default` group. Bake ignores the `default` target.

​	如果组和目标具有相同名称，则组优先。以下 Bake 文件将构建 `default` 组，忽略 `default` 目标。



```hcl
target "default" {
  dockerfile-inline = "FROM ubuntu"
}

group "default" {
  targets = ["alpine", "debian"]
}
target "alpine" {
  dockerfile-inline = "FROM alpine"
}
target "debian" {
  dockerfile-inline = "FROM debian"
}
```

## Variable

The HCL file format supports variable block definitions. You can use variables as build arguments in your Dockerfile, or interpolate them in attribute values in your Bake file.

​	HCL 文件格式支持变量块定义。您可以将变量用作 Dockerfile 中的构建参数，或在 Bake 文件中的属性值中插值。



```hcl
variable "TAG" {
  default = "latest"
}

target "webapp-dev" {
  dockerfile = "Dockerfile.webapp"
  tags = ["docker.io/username/webapp:${TAG}"]
}
```

You can assign a default value for a variable in the Bake file, or assign a `null` value to it. If you assign a `null` value, Buildx uses the default value from the Dockerfile instead.

​	您可以在 Bake 文件中为变量分配默认值，或分配 `null` 值。如果分配 `null` 值，Buildx 将使用 Dockerfile 中指定的默认值。

You can override variable defaults set in the Bake file using environment variables. The following example sets the `TAG` variable to `dev`, overriding the default `latest` value shown in the previous example.

​	可以使用环境变量覆盖在 Bake 文件中设置的变量默认值。以下示例将 `TAG` 变量设置为 `dev`，覆盖前例中默认的 `latest` 值。

```console
$ TAG=dev docker buildx bake webapp-dev
```

### 内置变量 Built-in variables

The following variables are built-ins that you can use with Bake without having to define them.

​	以下变量是内置的，可在 Bake 中使用而无需定义。

| Variable              | Description                                                  |
| --------------------- | ------------------------------------------------------------ |
| `BAKE_CMD_CONTEXT`    | 在使用远程 Bake 文件构建时保存主上下文。 Holds the main context when building using a remote Bake file. |
| `BAKE_LOCAL_PLATFORM` | 返回当前平台的默认平台规范（例如 `linux/amd64`）。 Returns the current platform’s default platform specification (e.g. `linux/amd64`). |

### 使用环境变量作为默认值 Use environment variable as default

You can set a Bake variable to use the value of an environment variable as a default value:

​	可以将 Bake 变量设置为使用环境变量的值作为默认值：



```hcl
variable "HOME" {
  default = "$HOME"
}
```

### 在属性中插值变量 Interpolate variables into attributes

To interpolate a variable into an attribute string value, you must use curly brackets. The following doesn't work:

​	要在属性字符串值中插值变量，必须使用大括号。以下方法不起作用：



```hcl
variable "HOME" {
  default = "$HOME"
}

target "default" {
  ssh = ["default=$HOME/.ssh/id_rsa"]
}
```

Wrap the variable in curly brackets where you want to insert it:

​	在要插入变量的位置用大括号将其包裹：



```diff
  variable "HOME" {
    default = "$HOME"
  }

  target "default" {
-   ssh = ["default=$HOME/.ssh/id_rsa"]
+   ssh = ["default=${HOME}/.ssh/id_rsa"]
  }
```

Before you can interpolate a variable into an attribute, first you must declare it in the bake file, as demonstrated in the following example.

​	在将变量插值到属性之前，必须在 Bake 文件中声明它，如以下示例所示。



```console
$ cat docker-bake.hcl
target "default" {
  dockerfile-inline = "FROM ${BASE_IMAGE}"
}
$ docker buildx bake
[+] Building 0.0s (0/0)
docker-bake.hcl:2
--------------------
   1 |     target "default" {
   2 | >>>   dockerfile-inline = "FROM ${BASE_IMAGE}"
   3 |     }
   4 |
--------------------
ERROR: docker-bake.hcl:2,31-41: Unknown variable; There is no variable named "BASE_IMAGE"., and 1 other diagnostic(s)
$ cat >> docker-bake.hcl

variable "BASE_IMAGE" {
  default = "alpine"
}

$ docker buildx bake
[+] Building 0.6s (5/5) FINISHED
```

## Function

A [set of general-purpose functions](https://github.com/docker/buildx/blob/master/bake/hclparser/stdlib.go) provided by [go-cty](https://github.com/zclconf/go-cty/tree/main/cty/function/stdlib) are available for use in HCL files:

​	可以在 HCL 文件中使用一组由 [go-cty](https://github.com/zclconf/go-cty/tree/main/cty/function/stdlib) 提供的 [通用函数](https://github.com/docker/buildx/blob/master/bake/hclparser/stdlib.go)：

```hcl
# docker-bake.hcl
target "webapp-dev" {
  dockerfile = "Dockerfile.webapp"
  tags = ["docker.io/username/webapp:latest"]
  args = {
    buildno = "${add(123, 1)}"
  }
}
```

In addition, [user defined functions](https://github.com/hashicorp/hcl/tree/main/ext/userfunc) are also supported:

​	此外，还支持 [用户定义的函数](https://github.com/hashicorp/hcl/tree/main/ext/userfunc)：

```hcl
# docker-bake.hcl
function "increment" {
  params = [number]
  result = number + 1
}

target "webapp-dev" {
  dockerfile = "Dockerfile.webapp"
  tags = ["docker.io/username/webapp:latest"]
  args = {
    buildno = "${increment(123)}"
  }
}
```

> **Note**
>
> See [User defined HCL functions](https://docs.docker.com/build/bake/hcl-funcs/) page for more details.
>
> ​	请参阅 [用户定义 HCL 函数](https://docs.docker.com/build/bake/hcl-funcs/) 页面了解更多详情。
