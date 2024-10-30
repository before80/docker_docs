+++
title = "Docker 插件 API"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/extend/plugin_api/](https://docs.docker.com/engine/extend/plugin_api/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker Plugin API - Docker 插件 API

Docker plugins are out-of-process extensions which add capabilities to the Docker Engine.

​	Docker 插件是独立的进程，增加了 Docker 引擎的功能。

This document describes the Docker Engine plugin API. To view information on plugins managed by Docker Engine, refer to [Docker Engine plugin system]({{< ref "/manuals/DockerEngine/DockerEngineplugins" >}}).

​	本文档描述了 Docker 引擎的插件 API。要查看 Docker 引擎管理的插件信息，请参考 [Docker 引擎插件系统]({{< ref "/manuals/DockerEngine/DockerEngineplugins" >}})。

This page is intended for people who want to develop their own Docker plugin. If you just want to learn about or use Docker plugins, look [here]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}}).

​	本页旨在面向希望开发自己 Docker 插件的人员。如果您只是想了解或使用 Docker 插件，请查看[此处]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}})。

## 什么是插件 What plugins are

A plugin is a process running on the same or a different host as the Docker daemon, which registers itself by placing a file on the daemon host in one of the plugin directories described in [Plugin discovery](https://docs.docker.com/engine/extend/plugin_api/#plugin-discovery).

​	插件是一个运行在与 Docker 守护进程相同或不同主机上的进程，它通过在守护进程主机的插件目录之一中放置文件进行注册，具体目录请参阅 [插件发现](https://docs.docker.com/engine/extend/plugin_api/#plugin-discovery)。

Plugins have human-readable names, which are short, lowercase strings. For example, `flocker` or `weave`.

​	插件有可读性强的名称，一般是简短的小写字符串。例如 `flocker` 或 `weave`。

Plugins can run inside or outside containers. Currently running them outside containers is recommended.

​	插件可以运行在容器内或容器外。目前推荐在容器外运行插件。

## 插件发现 Plugin discovery

Docker discovers plugins by looking for them in the plugin directory whenever a user or container tries to use one by name.

​	当用户或容器尝试按名称使用插件时，Docker 会在插件目录中查找插件。

There are three types of files which can be put in the plugin directory.

​	插件目录中可以包含三种类型的文件：

- `.sock` files are Unix domain sockets.
  - `.sock` 文件是 Unix 域套接字。

- `.spec` files are text files containing a URL, such as `unix:///other.sock` or `tcp://localhost:8080`.
  - `.spec` 文件是包含 URL 的文本文件，例如 `unix:///other.sock` 或 `tcp://localhost:8080`。

- `.json` files are text files containing a full json specification for the plugin.
  - `.json` 文件是包含插件完整 JSON 规范的文本文件。


Plugins with Unix domain socket files must run on the same host as the Docker daemon. Plugins with `.spec` or `.json` files can run on a different host if you specify a remote URL.

​	带有 Unix 域套接字文件的插件必须在与 Docker 守护进程相同的主机上运行。带有 `.spec` 或 `.json` 文件的插件可以在不同的主机上运行，只要指定一个远程 URL 即可。

Unix domain socket files must be located under `/run/docker/plugins`, whereas spec files can be located either under `/etc/docker/plugins` or `/usr/lib/docker/plugins`.

​	Unix 域套接字文件必须位于 `/run/docker/plugins` 下，而 spec 文件可以位于 `/etc/docker/plugins` 或 `/usr/lib/docker/plugins`。

The name of the file (excluding the extension) determines the plugin name.

​	文件名（不包含扩展名）决定插件的名称。

For example, the `flocker` plugin might create a Unix socket at `/run/docker/plugins/flocker.sock`.

​	例如，`flocker` 插件可以在 `/run/docker/plugins/flocker.sock` 创建一个 Unix 套接字。

You can define each plugin into a separated subdirectory if you want to isolate definitions from each other. For example, you can create the `flocker` socket under `/run/docker/plugins/flocker/flocker.sock` and only mount `/run/docker/plugins/flocker` inside the `flocker` container.

​	您可以为每个插件定义单独的子目录来隔离各自的定义。例如，可以在 `/run/docker/plugins/flocker/flocker.sock` 下创建 `flocker` 套接字，并仅在 `flocker` 容器内挂载 `/run/docker/plugins/flocker`。

Docker always searches for Unix sockets in `/run/docker/plugins` first. It checks for spec or json files under `/etc/docker/plugins` and `/usr/lib/docker/plugins` if the socket doesn't exist. The directory scan stops as soon as it finds the first plugin definition with the given name.

​	Docker 首先在 `/run/docker/plugins` 中搜索 Unix 套接字。如果套接字不存在，则会在 `/etc/docker/plugins` 和 `/usr/lib/docker/plugins` 下检查 spec 或 json 文件。目录扫描会在找到第一个匹配的插件定义时停止。

### JSON 规范 JSON specification

This is the JSON format for a plugin:

​	以下是插件的 JSON 格式：

```json
{
  "Name": "plugin-example",
  "Addr": "https://example.com/docker/plugin",
  "TLSConfig": {
    "InsecureSkipVerify": false,
    "CAFile": "/usr/shared/docker/certs/example-ca.pem",
    "CertFile": "/usr/shared/docker/certs/example-cert.pem",
    "KeyFile": "/usr/shared/docker/certs/example-key.pem"
  }
}
```

The `TLSConfig` field is optional and TLS will only be verified if this configuration is present.

​	`TLSConfig` 字段是可选的，仅在存在该配置时才会进行 TLS 验证。

## 插件生命周期 Plugin lifecycle

Plugins should be started before Docker, and stopped after Docker. For example, when packaging a plugin for a platform which supports `systemd`, you might use [`systemd` dependencies](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Before=) to manage startup and shutdown order.

​	插件应在 Docker 启动之前启动，并在 Docker 停止之后停止。例如，在支持 `systemd` 的平台上打包插件时，您可以使用 [`systemd` 依赖项](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Before=) 来管理启动和关闭顺序。

When upgrading a plugin, you should first stop the Docker daemon, upgrade the plugin, then start Docker again.

​	升级插件时，应先停止 Docker 守护进程，升级插件，然后重新启动 Docker。

## 插件激活 Plugin activation

When a plugin is first referred to -- either by a user referring to it by name (e.g. `docker run --volume-driver=foo`) or a container already configured to use a plugin being started -- Docker looks for the named plugin in the plugin directory and activates it with a handshake. See Handshake API below.

​	当插件第一次被引用时——无论是用户按名称引用它（如 `docker run --volume-driver=foo`），还是已配置为使用插件的容器被启动时——Docker 会在插件目录中查找指定的插件并通过握手进行激活。参见下方的握手 API。

Plugins are not activated automatically at Docker daemon startup. Rather, they are activated only lazily, or on-demand, when they are needed.

​	插件不会在 Docker 守护进程启动时自动激活。相反，它们仅在需要时按需激活。

## systemd 套接字激活 Systemd socket activation

Plugins may also be socket activated by `systemd`. The official [Plugins helpers](https://github.com/docker/go-plugins-helpers) natively supports socket activation. In order for a plugin to be socket activated it needs a `service` file and a `socket` file.

​	插件也可以通过 `systemd` 进行套接字激活。官方 [插件辅助工具](https://github.com/docker/go-plugins-helpers) 原生支持套接字激活。为了使插件能够通过套接字激活，它需要一个 `service` 文件和一个 `socket` 文件。

The `service` file (for example `/lib/systemd/system/your-plugin.service`):

​	`service` 文件（例如 `/lib/systemd/system/your-plugin.service`）：



```systemd
[Unit]
Description=Your plugin
Before=docker.service
After=network.target your-plugin.socket
Requires=your-plugin.socket docker.service

[Service]
ExecStart=/usr/lib/docker/your-plugin

[Install]
WantedBy=multi-user.target
```

The `socket` file (for example `/lib/systemd/system/your-plugin.socket`):

​	`socket` 文件（例如 `/lib/systemd/system/your-plugin.socket`）：



```systemd
[Unit]
Description=Your plugin

[Socket]
ListenStream=/run/docker/plugins/your-plugin.sock

[Install]
WantedBy=sockets.target
```

This will allow plugins to be actually started when the Docker daemon connects to the sockets they're listening on (for instance the first time the daemon uses them or if one of the plugin goes down accidentally).

​	这样当 Docker 守护进程连接到插件正在监听的套接字时（例如首次使用它们或其中一个插件意外关闭时），插件就会实际启动。

## API 设计 API design

The Plugin API is RPC-style JSON over HTTP, much like webhooks.

​	插件 API 是类似 RPC 风格的 JSON over HTTP，类似于 webhooks。

Requests flow from the Docker daemon to the plugin. The plugin needs to implement an HTTP server and bind this to the Unix socket mentioned in the "plugin discovery" section.

​	请求从 Docker 守护进程流向插件。插件需要实现一个 HTTP 服务器并绑定到“插件发现”部分中提到的 Unix 套接字。

All requests are HTTP `POST` requests.

​	所有请求都是 HTTP `POST` 请求。

The API is versioned via an Accept header, which currently is always set to `application/vnd.docker.plugins.v1+json`.

​	API 通过 Accept 头进行版本控制，目前始终设置为 `application/vnd.docker.plugins.v1+json`。

## 握手 API - Handshake API

Plugins are activated via the following "handshake" API call.

​	插件通过以下“握手” API 调用进行激活。

### /Plugin.Activate

Request: empty body

​	请求：空请求体

Response:

​	响应：

```json
{
    "Implements": ["VolumeDriver"]
}
```

Responds with a list of Docker subsystems which this plugin implements. After activation, the plugin will then be sent events from this subsystem.

​	响应包含插件实现的 Docker 子系统列表。激活后，该插件将接收来自该子系统的事件。

Possible values are:

​	可能的值包括：

- [`authz`]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Accessauthorizationplugin" >}})
- [`NetworkDriver`]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockernetworkdriverplugins" >}})
- [`VolumeDriver`]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockervolumeplugins" >}})

## 插件重试 Plugin retries

Attempts to call a method on a plugin are retried with an exponential backoff for up to 30 seconds. This may help when packaging plugins as containers, since it gives plugin containers a chance to start up before failing any user containers which depend on them.

​	对插件方法的调用尝试将通过指数回退重试，最长持续 30 秒。这在将插件打包为容器时可能有所帮助，因为它可以在依赖于它们的用户容器失败之前让插件容器有机会启动。

## 插件辅助工具 Plugins helpers

To ease plugins development, we're providing an `sdk` for each kind of plugins currently supported by Docker at [docker/go-plugins-helpers](https://github.com/docker/go-plugins-helpers).

​	为了简化插件开发，我们为 Docker 当前支持的每种插件类型提供了一个 `sdk`，请参见 [docker/go-plugins-helpers](https://github.com/docker/go-plugins-helpers)。
