+++
title = "Customize"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/compose/bridge/customize/](https://docs.docker.com/compose/bridge/customize/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Customize Compose Bridge - Compose Bridge 概述

**Experimental 实验性功能**

Compose Bridge is an [Experimental](https://docs.docker.com/release-lifecycle/#experimental) product.

​	Compose Bridge 是一个[实验性](https://docs.docker.com/release-lifecycle/#experimental)产品。

This page explains how Compose Bridge utilizes templating to efficiently translate Docker Compose files into Kubernetes manifests. It also explain how you can customize these templates for your specific requirements and needs, or how you can build your own transformation.

​	本页面解释了 Compose Bridge 如何利用模板将 Docker Compose 文件高效转换为 Kubernetes 清单。还说明了如何根据您的具体需求自定义这些模板，或者如何构建自己的转换。

## 工作原理 How it works

Compose bridge uses transformations to let you convert a Compose model into another form.

​	Compose Bridge 使用转换来让您将 Compose 模型转换为另一种形式。

A transformation is packaged as a Docker image that receives the fully-resolved Compose model as `/in/compose.yaml` and can produce any target format file under `/out`.

​	转换被打包为一个 Docker 镜像，该镜像接收已解析的 Compose 模型（`/in/compose.yaml`）并可在 `/out` 目录下生成任何目标格式的文件。

Compose Bridge provides its transformation for Kubernetes using Go templates, so that it is easy to extend for customization by just replacing or appending your own templates.

​	Compose Bridge 提供了用于 Kubernetes 的转换，通过 Go 模板，可以轻松扩展自定义，只需替换或添加模板即可。

### 语法 Syntax

Compose Bridge make use of templates to transform a Compose configuration file into Kubernetes manifests. Templates are plain text files that use the [Go templating syntax](https://pkg.go.dev/text/template). This enables the insertion of logic and data, making the templates dynamic and adaptable according to the Compose model.

​	Compose Bridge 使用模板将 Compose 配置文件转换为 Kubernetes 清单。模板是使用[Go 模板语法](https://pkg.go.dev/text/template)的纯文本文件。此功能允许插入逻辑和数据，使模板根据 Compose 模型动态调整。

When a template is executed, it must produce a YAML file which is the standard format for Kubernetes manifests. Multiple files can be generated as long as they are separated by `---`

​	当模板执行时，它必须生成一个 YAML 文件，这是 Kubernetes 清单的标准格式。只要文件之间以 `---` 分隔，就可以生成多个文件。

Each YAML output file begins with custom header notation, for example:

​	每个 YAML 输出文件都以自定义标头注释开始，例如：

```yaml
#! manifest.yaml
```

In the following example, a template iterates over services defined in a `compose.yaml` file. For each service, a dedicated Kubernetes manifest file is generated, named according to the service and containing specified configurations.

​	在以下示例中，模板遍历 `compose.yaml` 文件中定义的服务。对于每个服务，将生成一个专用的 Kubernetes 清单文件，并按服务命名，包含指定的配置。

```yaml
{{ range $name, $service := .services }}
---
#! {{ $name }}-manifest.yaml
# Generated code, do not edit
key: value
## ...
{{ end }}
```

### 输入 Input

The input Compose model is the canonical YAML model you can get by running `docker compose config`. Within the templates, data from the `compose.yaml` is accessed using dot notation, allowing you to navigate through nested data structures. For example, to access the deployment mode of a service, you would use `service.deploy.mode`:

​	输入的 Compose 模型是您可以通过运行 `docker compose config` 获得的规范 YAML 模型。在模板中，可以使用点符号访问 `compose.yaml` 中的数据，从而遍历嵌套的数据结构。例如，要访问服务的部署模式，可以使用 `service.deploy.mode`：

```yaml
# iterate over a yaml sequence
{{ range $name, $service := .services }}
 # access a nested attribute using dot notation
 {{ if eq $service.deploy.mode "global" }}
kind: DaemonSet
 {{ end }}
{{ end }}
```

You can check the [Compose Specification JSON schema](https://github.com/compose-spec/compose-go/blob/main/schema/compose-spec.json) to have a full overview of the Compose model. This schema outlines all possible configurations and their data types in the Compose model.

​	您可以查看 [Compose 规范 JSON schema](https://github.com/compose-spec/compose-go/blob/main/schema/compose-spec.json) 以全面了解 Compose 模型。此 schema 列出了 Compose 模型中所有可能的配置及其数据类型。

### 辅助函数 Helpers

As part of the Go templating syntax, Compose Bridge offers a set of YAML helper functions designed to manipulate data within the templates efficiently:

​	作为 Go 模板语法的一部分，Compose Bridge 提供了一组 YAML 辅助函数，用于在模板中高效处理数据：

- `seconds`: Converts a [duration](https://docs.docker.com/reference/compose-file/extension/#specifying-durations) into an integer
  - `seconds`: 将[持续时间](https://docs.docker.com/reference/compose-file/extension/#specifying-durations)转换为整数

- `uppercase`: Converts a string into upper case characters
  - `uppercase`: 将字符串转换为大写字符

- `title`: Converts a string by capitalizing the first letter of each word
  - `title`: 将字符串中的每个单词首字母大写

- `safe`: Converts a string into a safe identifier, replacing all characters (except lowercase a-z) with `-`
  - `safe`: 将字符串转换为安全标识符，替换所有字符（除小写 a-z 外）为 `-`

- `truncate`: Removes the N first elements from a list
  - `truncate`: 从列表中移除前 N 个元素

- `join`: Groups elements from a list into a single string, using a separator
  - `join`: 使用分隔符将列表元素组合成一个字符串

- `base64`: Encodes a string as base64 used in Kubernetes for encoding secrets
  - `base64`: 将字符串编码为 base64，用于 Kubernetes 编码密钥

- `map`: Transforms a value according to mappings expressed as `"value -> newValue"` strings
  - `map`: 根据表达的映射如 `"value -> newValue"` 转换值

- `indent`: Writes string content indented by N spaces
  - `indent`: 按 N 空格缩进字符串内容

- `helmValue`: Writes the string content as a template value in the final file
  - `helmValue`: 将字符串内容写入最终文件作为模板值


In the following example, the template checks if a healthcheck interval is specified for a service, applies the `seconds` function to convert this interval into seconds and assigns the value to the `periodSeconds` attribute.

​	在以下示例中，模板检查是否为服务指定了健康检查间隔，使用 `seconds` 函数将此间隔转换为秒，并将值分配给 `periodSeconds` 属性。

```yaml
{{ if $service.healthcheck.interval }}
            periodSeconds: {{ $service.healthcheck.interval | seconds }}{{ end }}
{{ end }}
```

## 自定义 Customization

As Kubernetes is a versatile platform, there are many ways to map Compose concepts into Kubernetes resource definitions. Compose Bridge lets you customize the transformation to match your own infrastructure decisions and preferences, with various level of flexibility and effort.

​	由于 Kubernetes 是一个灵活的平台，有多种方式可以将 Compose 概念映射到 Kubernetes 资源定义。Compose Bridge 允许您根据自己的基础架构决策和偏好自定义转换，并提供不同程度的灵活性和工作量。

### 修改默认模板 Modify the default templates

You can extract templates used by the default transformation `docker/compose-bridge-kubernetes`, by running `compose-bridge transformations create --from docker/compose-bridge-kubernetes my-template` and adjusting the templates to match your needs.

​	您可以通过运行 `compose-bridge transformations create --from docker/compose-bridge-kubernetes my-template` 提取默认转换 `docker/compose-bridge-kubernetes` 使用的模板，并调整这些模板以符合您的需求。

The templates are extracted into a directory named after your template name, in this case `my-template`.
It includes a Dockerfile that lets you create your own image to distribute your template, as well as a directory containing the templating files.

​	模板将被提取到以您的模板名称命名的目录中，在此示例中为 `my-template`。它包含一个 Dockerfile，让您可以创建自己的镜像以分发模板，还包含模板文件的目录。

You are free to edit the existing files, delete them, or [add new ones](https://docs.docker.com/compose/bridge/customize/#add-your-own-templates) to subsequently generate Kubernetes manifests that meet your needs.

​	您可以自由编辑现有文件、删除文件或[添加新文件](https://docs.docker.com/compose/bridge/customize/#add-your-own-templates)，以生成符合您需求的 Kubernetes 清单。

You can then use the generated Dockerfile to package your changes into a new transformation image, which you can then use with Compose Bridge:

​	然后您可以使用生成的 Dockerfile 将更改打包到新的转换镜像中，然后使用 Compose Bridge 进行转换：

```console
$ docker build --tag mycompany/transform --push .
```

You can then use your transformation as a replacement:

​	然后，您可以使用您的转换作为替代：

```console
$ compose-bridge convert --transformations mycompany/transform 
```

### 添加自定义模板 Add your own templates

For resources that are not managed by Compose Bridge's default transformation, you can build your own templates. The `compose.yaml` model may not offer all the configuration attributes required to populate the target manifest. If this is the case, you can then rely on Compose custom extensions to better describe the application, and offer an agnostic transformation.

​	对于 Compose Bridge 默认转换不管理的资源，您可以构建自己的模板。`compose.yaml` 模型可能无法提供填充目标清单所需的所有配置属性。如果是这样，您可以依赖 Compose 自定义扩展来更好地描述应用程序，并提供通用转换。

For example, if you add `x-virtual-host` metadata to service definitions in the `compose.yaml` file, you can use the following custom attribute to produce Ingress rules:

​	例如，如果在 `compose.yaml` 文件中的服务定义中添加 `x-virtual-host` 元数据，可以使用以下自定义属性来生成 Ingress 规则：

```yaml
{{ $project := .name }}
#! {{ $name }}-ingress.yaml
# Generated code, do not edit
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: virtual-host-ingress
  namespace: {{ $project }}
spec:
  rules:  
{{ range $name, $service := .services }}
{{ if $service.x-virtual-host }}
  - host: ${{ $service.x-virtual-host }}
    http:
      paths:
      - path: "/"
        backend:
          service:
            name: ${{ name }}
            port:
              number: 80  
{{ end }}
{{ end }}
```

Once packaged into a Docker image, you can use this custom template when transforming Compose models into Kubernetes in addition to other transformations:

​	将其打包到 Docker 镜像后，您可以在将 Compose 模型转换为 Kubernetes 时使用此自定义模板以及其他转换：

```console
$ compose-bridge convert \
    --transformation docker/compose-bridge-kubernetes \
    --transformation mycompany/transform 
```

### 构建自己的转换 Build your own transformation

While Compose Bridge templates make it easy to customize with minimal changes, you may want to make significant changes, or rely on an existing conversion tool.

​	虽然 Compose Bridge 模板使得轻量级定制变得简单，但您可能希望进行更大的更改，或者依赖现有的转换工具。

A Compose Bridge transformation is a Docker image that is designed to get a Compose model from `/in/compose.yaml` and produce platform manifests under `/out`. This simple contract makes it easy to bundle an alternate transformation using [Kompose](https://kompose.io/):

​	Compose Bridge 转换是一个 Docker 镜像，设计为从 `/in/compose.yaml` 获取 Compose 模型并在 `/out` 下生成平台清单。这个简单的契约使得使用 [Kompose](https://kompose.io/) 捆绑替代转换变得容易：

```Dockerfile
FROM alpine

# Get kompose from github release page
# 从 GitHub 发布页面获取 Kompose
RUN apk add --no-cache curl
ARG VERSION=1.32.0
RUN ARCH=$(uname -m | sed 's/armv7l/arm/g' | sed 's/aarch64/arm64/g' | sed 's/x86_64/amd64/g') && \
    curl -fsL \
    "https://github.com/kubernetes/kompose/releases/download/v${VERSION}/kompose-linux-${ARCH}" \
    -o /usr/bin/kompose
RUN chmod +x /usr/bin/kompose

CMD ["/usr/bin/kompose", "convert", "-f", "/in/compose.yaml", "--out", "/out"]
```

This Dockerfile bundles Kompose and defines the command to run this tool according to the Compose Bridge transformation contract.

​	此 Dockerfile 包含 Kompose，并根据 Compose Bridge 转换协议定义运行该工具的命令。

## 接下来 What's next?

- [Explore the advanced integration 探索高级集成]({{< ref "/manuals/DockerCompose/ComposeBridge/Advanced" >}})
