+++
title = "docker image tag"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/tag/](https://docs.docker.com/reference/cli/docker/image/tag/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image tag

| Description | Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE    |
| :---------- | -------------------------------------------------------- |
| Usage       | `docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]` |
| Aliases     | `docker tag`                                             |

## Description

A full image name has the following format and components:

​	完整的镜像名称格式和组件如下：

```
[HOST[:PORT_NUMBER]/]PATH
```

- `HOST`: The optional registry hostname specifies where the image is located. The hostname must comply with standard DNS rules, but may not contain underscores. If you don't specify a hostname, the command uses Docker's public registry at `registry-1.docker.io` by default. Note that `docker.io` is the canonical reference for Docker's public registry.
  - `HOST`：可选的注册表主机名，指定镜像所在的位置。主机名必须遵循标准 DNS 规则，但不能包含下划线。如果未指定主机名，命令默认使用 Docker 的公共注册表 `registry-1.docker.io`。请注意，`docker.io` 是 Docker 公共注册表的规范引用。

- `PORT_NUMBER`: If a hostname is present, it may optionally be followed by a registry port number in the format `:8080`.
  - `PORT_NUMBER`：如果存在主机名，可以选择性地在后面添加注册表端口号，格式为 `:8080`。

- `PATH`: The path consists of slash-separated components. Each component may contain lowercase letters, digits and separators. A separator is defined as a period, one or two underscores, or one or more hyphens. A component may not start or end with a separator. While theWhile the [OCI Distribution Specification](https://github.com/opencontainers/distribution-spec) supports more than two slash-separated components, most registries only support two slash-separated components. For Docker's public registry, the path format is as follows:
  - `PATH`：路径由斜杠分隔的组件组成。每个组件可以包含小写字母、数字和分隔符。分隔符定义为一个句点、一个或两个下划线，或一个或多个连字符。组件不能以分隔符开头或结尾。虽然 [OCI Distribution Specification](https://github.com/opencontainers/distribution-spec) 支持两个以上斜杠分隔的组件，但大多数注册表仅支持两个斜杠分隔的组件。对于 Docker 的公共注册表，路径格式如下：
  - `[NAMESPACE/]REPOSITORY`: The first, optional component is typically a user's or an organization's namespace. The second, mandatory component is the repository name. When the namespace is not present, Docker uses `library` as the default namespace.
    - `[NAMESPACE/]REPOSITORY`：第一个可选组件通常是用户或组织的命名空间。第二个强制组件是仓库名称。当命名空间不存在时，Docker 使用 `library` 作为默认命名空间。


After the image name, the optional `TAG` is a custom, human-readable manifest identifier that's typically a specific version or variant of an image. The tag must be valid ASCII and can contain lowercase and uppercase letters, digits, underscores, periods, and hyphens. It can't start with a period or hyphen and must be no longer than 128 characters. If you don't specify a tag, the command uses `latest` by default.

​	在镜像名称之后，可选的 `TAG` 是一个自定义的人类可读清单标识符，通常是镜像的特定版本或变体。标签必须是有效的 ASCII，并可以包含小写和大写字母、数字、下划线、句点和连字符。标签不能以句点或连字符开头，并且不得超过 128 个字符。如果未指定标签，命令默认使用 `latest`。

You can group your images together using names and tags, and then [push]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpush" >}}) them to a registry.

​	你可以使用名称和标签将镜像分组，然后将其 [推送]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpush" >}}) 到注册表。

## Examples

### 按 ID 标记镜像 Tag an image referenced by ID

To tag a local image with ID `0e5574283393` as `fedora/httpd` with the tag `version1.0`:

​	将本地镜像 ID `0e5574283393` 标记为 `fedora/httpd`，标签为 `version1.0`：



```console
$ docker tag 0e5574283393 fedora/httpd:version1.0
```

### 按名称标记镜像 Tag an image referenced by Name

To tag a local image `httpd` as `fedora/httpd` with the tag `version1.0`:

​	将本地镜像 `httpd` 标记为 `fedora/httpd`，标签为 `version1.0`：



```console
$ docker tag httpd fedora/httpd:version1.0
```

Note that since the tag name isn't specified, the alias is created for an existing local version `httpd:latest`.

​	请注意，由于未指定标签名称，因此为现有本地版本 `httpd:latest` 创建了别名。

### 按名称和标签标记镜像 Tag an image referenced by Name and Tag

To tag a local image with the name `httpd` and the tag `test` as `fedora/httpd` with the tag `version1.0.test`:

​	将名称为 `httpd` 和标签为 `test` 的本地镜像标记为 `fedora/httpd`，标签为 `version1.0.test`：



```console
$ docker tag httpd:test fedora/httpd:version1.0.test
```

### 为私有注册表标记镜像 Tag an image for a private registry

To push an image to a private registry and not the public Docker registry you must include the registry hostname and port (if needed).

​	要将镜像推送到私有注册表而不是公共 Docker 注册表，你必须包含注册表主机名和端口（如有需要）。

```console
$ docker tag 0e5574283393 myregistryhost:5000/fedora/httpd:version1.0
```
