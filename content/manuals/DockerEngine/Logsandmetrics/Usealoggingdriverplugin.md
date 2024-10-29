+++
title = "使用日志驱动插件"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/plugins/](https://docs.docker.com/engine/logging/plugins/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use a logging driver plugin - 使用日志驱动插件

Docker logging plugins allow you to extend and customize Docker's logging capabilities beyond those of the [built-in logging drivers]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}}). A logging service provider can [implement their own plugins]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockerlogdriverplugins" >}}) and make them available on Docker Hub, or a private registry. This topic shows how a user of that logging service can configure Docker to use the plugin.

​	Docker 日志插件允许您扩展和自定义 Docker 的日志功能，超出[内置日志驱动]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}})的范围。日志服务提供商可以[实现自己的插件]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockerlogdriverplugins" >}})，并在 Docker Hub 或私有注册表上提供。这部分展示了日志服务用户如何配置 Docker 以使用该插件。

## 安装日志驱动插件 Install the logging driver plugin

To install a logging driver plugin, use `docker plugin install <org/image>`, using the information provided by the plugin developer.

​	要安装日志驱动插件，请使用插件开发者提供的信息运行 `docker plugin install <org/image>`。

You can list all installed plugins using `docker plugin ls`, and you can inspect a specific plugin using `docker inspect`.

​	可以使用 `docker plugin ls` 列出所有已安装的插件，并使用 `docker inspect` 检查特定插件。

## 配置插件作为默认日志驱动 Configure the plugin as the default logging driver

When the plugin is installed, you can configure the Docker daemon to use it as the default by setting the plugin's name as the value of the `log-driver` key in the `daemon.json`, as detailed in the [logging overview](https://docs.docker.com/engine/logging/configure/#configure-the-default-logging-driver). If the logging driver supports additional options, you can set those as the values of the `log-opts` array in the same file.

​	安装插件后，可以在 `daemon.json` 中将 `log-driver` 键的值设置为插件名称，将其配置为默认日志驱动，具体说明详见[日志概览](https://docs.docker.com/engine/logging/configure/#configure-the-default-logging-driver)。如果日志驱动支持其他选项，可以在同一文件中的 `log-opts` 数组中设置这些值。

## 配置容器使用插件作为日志驱动 Configure a container to use the plugin as the logging driver

After the plugin is installed, you can configure a container to use the plugin as its logging driver by specifying the `--log-driver` flag to `docker run`, as detailed in the [logging overview](https://docs.docker.com/engine/logging/configure/#configure-the-logging-driver-for-a-container). If the logging driver supports additional options, you can specify them using one or more `--log-opt` flags with the option name as the key and the option value as the value.

​	安装插件后，可以通过在 `docker run` 中指定 `--log-driver` 标志，配置容器使用该插件作为其日志驱动，具体说明详见[日志概览](https://docs.docker.com/engine/logging/configure/#configure-the-logging-driver-for-a-container)。如果日志驱动支持其他选项，可以使用一个或多个 `--log-opt` 标志指定这些选项，将选项名称作为键，选项值作为值。
