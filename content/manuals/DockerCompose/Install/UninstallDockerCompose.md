+++
title = "卸载 Docker Compose"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/install/uninstall/](https://docs.docker.com/compose/install/uninstall/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Uninstall Docker Compose - 卸载 Docker Compose

Uninstalling Docker Compose depends on the method you have used to install Docker Compose. On this page you can find specific instructions to uninstall Docker Compose.

​	卸载 Docker Compose 的方式取决于安装方法。以下是具体的卸载说明。

## Uninstalling Docker Desktop

If you want to uninstall Compose and you have installed Docker Desktop, see [Uninstall Docker Desktop]({{< ref "/manuals/DockerDesktop/Uninstall" >}}).

​	如果您安装了 Docker Desktop 并希望卸载 Compose，请参见 [卸载 Docker Desktop]({{< ref "/manuals/DockerDesktop/Uninstall" >}})。

> **Note**
>
> 
>
> Unless you have other Docker instances installed on that specific environment, you would be removing Docker altogether by uninstalling the Desktop.
>
> ​	卸载 Docker Desktop 将会删除整个 Docker，除非您在特定环境中安装了其他 Docker 实例。

## 卸载 Docker Compose CLI 插件 Uninstalling the Docker Compose CLI plugin

To remove the Compose CLI plugin, run:

​	要删除 Compose CLI 插件，请运行：

Ubuntu, Debian:



```console
$ sudo apt-get remove docker-compose-plugin
```

RPM-based distros:



```console
$ sudo yum remove docker-compose-plugin
```

### Manually installed

If you used `curl` to install Compose CLI plugin, to uninstall it, run:

​	如果使用 `curl` 安装了 Compose CLI 插件，请运行以下命令进行卸载：



```console
$ rm $DOCKER_CONFIG/cli-plugins/docker-compose
```

### Remove for all users

Or, if you have installed Compose for all users, run:

​	如果为所有用户安装了 Compose，请运行：



```console
$ rm /usr/local/lib/docker/cli-plugins/docker-compose
```

> **Note**
>
> 
>
> If you get a **Permission denied** error using either of the above methods, you do not have the permissions allowing you to remove Docker Compose. To force the removal, prepend `sudo` to either of the above instructions and run it again.
>
> ​	如果使用上述方法遇到 **Permission denied** 错误，说明您没有权限删除 Docker Compose。可以在上述指令前添加 `sudo` 以强制删除。

### 检查 Compose CLI 插件的位置 Inspect the location of the Compose CLI plugin

To check where Compose is installed, use:

​	要检查 Compose 的安装位置，请使用以下命令：

```console
$ docker info --format '{{range .ClientInfo.Plugins}}{{if eq .Name "compose"}}{{.Path}}{{end}}{{end}}'
```
