+++
title = "Use a logging driver plugin"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/engine/logging/plugins/](https://docs.docker.com/engine/logging/plugins/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use a logging driver plugin

Docker logging plugins allow you to extend and customize Docker's logging capabilities beyond those of the [built-in logging drivers](https://docs.docker.com/engine/logging/configure/). A logging service provider can [implement their own plugins](https://docs.docker.com/engine/extend/plugins_logging/) and make them available on Docker Hub, or a private registry. This topic shows how a user of that logging service can configure Docker to use the plugin.

## [Install the logging driver plugin](https://docs.docker.com/engine/logging/plugins/#install-the-logging-driver-plugin)

To install a logging driver plugin, use `docker plugin install <org/image>`, using the information provided by the plugin developer.

You can list all installed plugins using `docker plugin ls`, and you can inspect a specific plugin using `docker inspect`.

## [Configure the plugin as the default logging driver](https://docs.docker.com/engine/logging/plugins/#configure-the-plugin-as-the-default-logging-driver)

When the plugin is installed, you can configure the Docker daemon to use it as the default by setting the plugin's name as the value of the `log-driver` key in the `daemon.json`, as detailed in the [logging overview](https://docs.docker.com/engine/logging/configure/#configure-the-default-logging-driver). If the logging driver supports additional options, you can set those as the values of the `log-opts` array in the same file.

## [Configure a container to use the plugin as the logging driver](https://docs.docker.com/engine/logging/plugins/#configure-a-container-to-use-the-plugin-as-the-logging-driver)

After the plugin is installed, you can configure a container to use the plugin as its logging driver by specifying the `--log-driver` flag to `docker run`, as detailed in the [logging overview](https://docs.docker.com/engine/logging/configure/#configure-the-logging-driver-for-a-container). If the logging driver supports additional options, you can specify them using one or more `--log-opt` flags with the option name as the key and the option value as the value.
