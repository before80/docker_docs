+++
title = "Docker 对象标签"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/manage-resources/labels/](https://docs.docker.com/engine/manage-resources/labels/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker object labels - Docker 对象标签

Labels are a mechanism for applying metadata to Docker objects, including:

​	标签是一种用于将元数据应用于 Docker 对象的机制，包括：

- Images

- Containers
- Local daemons
- Volumes
- Networks
- Swarm nodes
- Swarm services

You can use labels to organize your images, record licensing information, annotate relationships between containers, volumes, and networks, or in any way that makes sense for your business or application.

​	可以使用标签来组织镜像、记录许可信息、注释容器、卷和网络之间的关系，或以其他方式应用于您的业务或应用程序。

## 标签键和值 Label keys and values

A label is a key-value pair, stored as a string. You can specify multiple labels for an object, but each key must be unique within an object. If the same key is given multiple values, the most-recently-written value overwrites all previous values.

​	标签是一个键值对，以字符串形式存储。可以为一个对象指定多个标签，但在一个对象内每个键必须唯一。如果同一键被赋予多个值，最新写入的值将覆盖所有之前的值。

### 键格式建议 Key format recommendations

A label key is the left-hand side of the key-value pair. Keys are alphanumeric strings which may contain periods (`.`), underscores (`_`), slashes (`/`), and hyphens (`-`). Most Docker users use images created by other organizations, and the following guidelines help to prevent inadvertent duplication of labels across objects, especially if you plan to use labels as a mechanism for automation.

​	标签键是键值对的左侧部分。键是由字母和数字组成的字符串，可以包含点 (`.`)、下划线 (`_`)、斜线 (`/`) 和连字符 (`-`)。大多数 Docker 用户使用其他组织创建的镜像，以下指南有助于防止对象之间标签的意外重复，特别是当您计划将标签用作自动化机制时。

- Authors of third-party tools should prefix each label key with the reverse DNS notation of a domain they own, such as `com.example.some-label`.
  - 第三方工具的作者应使用他们拥有的域名的反向 DNS 表示法来前缀每个标签键，例如 `com.example.some-label`。

- Don't use a domain in your label key without the domain owner's permission.
  - 未经域名所有者许可，不要在标签键中使用其域名。

- The `com.docker.*`, `io.docker.*`, and `org.dockerproject.*` namespaces are reserved by Docker for internal use.
  - `com.docker.*`、`io.docker.*` 和 `org.dockerproject.*` 命名空间由 Docker 保留供内部使用。

- Label keys should begin and end with a lower-case letter and should only contain lower-case alphanumeric characters, the period character (`.`), and the hyphen character (`-`). Consecutive periods or hyphens aren't allowed.
  - 标签键应以小写字母开头和结尾，只包含小写字母和数字字符、点 (`.`) 和连字符 (`-`)，不允许连续的点或连字符。

- The period character (`.`) separates namespace "fields". Label keys without namespaces are reserved for CLI use, allowing users of the CLI to interactively label Docker objects using shorter typing-friendly strings.
  - 点 (`.`) 用于分隔命名空间“字段”。没有命名空间的标签键保留用于 CLI，以便 CLI 用户可以使用较短的易于输入的字符串与 Docker 对象进行交互式标记。


These guidelines aren't currently enforced and additional guidelines may apply to specific use cases.

​	这些指南目前不强制执行，特定用例可能适用其他指南。

### 值指南 Value guidelines

Label values can contain any data type that can be represented as a string, including (but not limited to) JSON, XML, CSV, or YAML. The only requirement is that the value be serialized to a string first, using a mechanism specific to the type of structure. For instance, to serialize JSON into a string, you might use the `JSON.stringify()` JavaScript method.

​	标签值可以包含任何可以表示为字符串的数据类型，包括（但不限于）JSON、XML、CSV 或 YAML。唯一的要求是首先将值序列化为字符串，使用特定于结构类型的机制。例如，将 JSON 序列化为字符串，可以使用 `JSON.stringify()` JavaScript 方法。

Since Docker doesn't deserialize the value, you can't treat a JSON or XML document as a nested structure when querying or filtering by label value unless you build this functionality into third-party tooling.

​	由于 Docker 不会反序列化值，因此在查询或按标签值过滤时，不能将 JSON 或 XML 文档视为嵌套结构，除非将此功能构建到第三方工具中。

## 管理对象上的标签 Manage labels on objects

Each type of object with support for labels has mechanisms for adding and managing them and using them as they relate to that type of object. These links provide a good place to start learning about how you can use labels in your Docker deployments.

​	支持标签的每种对象类型都有添加和管理标签以及使用标签的机制，这些机制与该类型的对象相关。这些链接提供了如何在 Docker 部署中使用标签的起点。

Labels on images, containers, local daemons, volumes, and networks are static for the lifetime of the object. To change these labels you must recreate the object. Labels on Swarm nodes and services can be updated dynamically.

​	对于镜像、容器、本地守护进程、卷和网络的标签是对象生命周期内的静态标签。要更改这些标签，必须重新创建对象。Swarm 节点和服务的标签可以动态更新。

- Images and containers 镜像和容器

- - [Adding labels to images 为镜像添加标签]({{< ref "/reference/Dockerfilereference#label">}})
  - [Overriding a container's labels at runtime 在运行时覆盖容器的标签](https://docs.docker.com/reference/cli/docker/container/run/#label)
  - [Inspecting labels on images or containers 检查镜像或容器的标签]({{< ref "/reference/CLIreference/docker/dockerinspect" >}})
  - [Filtering images by label 按标签过滤镜像]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimages#filtering---filter">}})
  - [Filtering containers by label 按标签过滤容器]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerps#filtering---filter">}})
- Local Docker daemons 本地 Docker 守护进程
  - [Adding labels to a Docker daemon at runtime 在运行时为 Docker 守护进程添加标签]({{< ref "/reference/CLIreference/dockerd" >}})
  - [Inspecting a Docker daemon's labels 检查 Docker 守护进程的标签]({{< ref "/reference/CLIreference/docker/dockersystem/dockerinfo" >}})
- Volumes 卷
  - [Adding labels to volumes 为卷添加标签]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumecreate" >}})
  - [Inspecting a volume's labels 检查卷的标签]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumeinspect" >}})
  - [Filtering volumes by label 按标签过滤卷]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumels#filtering---filter">}})
- Networks 网络
  - [Adding labels to a network 为网络添加标签]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkcreate" >}})
  - [Inspecting a network's labels 检查网络的标签]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkinspect" >}})
  - [Filtering networks by label 按标签过滤网络]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkls#filtering---filter">}})
- Swarm nodes - Swarm 节点
  - [Adding or updating a Swarm node's labels 为 Swarm 节点添加或更新标签]({{< ref "/reference/CLIreference/docker/dockernode">}})
  - [Inspecting a Swarm node's labels 检查 Swarm 节点的标签]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeinspect" >}})
  - [Filtering Swarm nodes by label 按标签过滤 Swarm 节点]({{< ref "/reference/CLIreference/docker/dockernode/dockernodels#filtering---filter">}})
- Swarm services - Swarm 服务
  - [Adding labels when creating a Swarm service 创建 Swarm 服务时添加标签](https://docs.docker.com/reference/cli/docker/service/create/#label)
  - [Updating a Swarm service's labels 更新 Swarm 服务的标签]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}})
  - [Inspecting a Swarm service's labels 检查 Swarm 服务的标签]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceinspect" >}})
  - [Filtering Swarm services by label 按标签过滤 Swarm 服务]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicels#filtering---filter">}})
