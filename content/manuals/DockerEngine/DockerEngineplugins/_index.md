+++
title = "Docker Engine 插件"
date = 2024-10-23T14:54:40+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/extend/](https://docs.docker.com/engine/extend/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker Engine managed plugin system - Docker Engine 管理插件系统

- [Installing and using a plugin 安装和使用插件](https://docs.docker.com/engine/extend/#installing-and-using-a-plugin)

- [Developing a plugin 开发插件](https://docs.docker.com/engine/extend/#developing-a-plugin)
- [Debugging plugins 调试插件](https://docs.docker.com/engine/extend/#debugging-plugins)

Docker Engine's plugin system lets you install, start, stop, and remove plugins using Docker Engine.

​	Docker Engine 的插件系统允许你使用 Docker Engine 安装、启动、停止和删除插件。

For information about legacy (non-managed) plugins, refer to [Understand legacy Docker Engine plugins]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}}).

​	关于传统（非管理）插件的信息，请参考 [了解传统的 Docker Engine 插件]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}})。

> **Note**
>
> Docker Engine managed plugins are currently not supported on Windows daemons.
>
> ​	Docker Engine 管理的插件目前不支持 Windows 守护进程。

## 安装和使用插件 Installing and using a plugin

Plugins are distributed as Docker images and can be hosted on Docker Hub or on a private registry.

​	插件作为 Docker 镜像分发，可以托管在 Docker Hub 或私有注册表中。

To install a plugin, use the `docker plugin install` command, which pulls the plugin from Docker Hub or your private registry, prompts you to grant permissions or capabilities if necessary, and enables the plugin.

​	使用 `docker plugin install` 命令安装插件，它会从 Docker Hub 或私有注册表中拉取插件，在必要时提示你授予权限或功能，并启用插件。

To check the status of installed plugins, use the `docker plugin ls` command. Plugins that start successfully are listed as enabled in the output.

​	要检查已安装插件的状态，使用 `docker plugin ls` 命令。启动成功的插件在输出中列为已启用状态。

After a plugin is installed, you can use it as an option for another Docker operation, such as creating a volume.

​	安装插件后，你可以将其用作其他 Docker 操作的选项，例如创建卷。

In the following example, you install the `sshfs` plugin, verify that it is enabled, and use it to create a volume.

​	以下示例展示了如何安装 `sshfs` 插件、验证插件是否已启用，并使用该插件创建卷。

> **Note**
>
> This example is intended for instructional purposes only. Once the volume is created, your SSH password to the remote host is exposed as plaintext when inspecting the volume. Delete the volume as soon as you are done with the example.
>
> ​	此示例仅用于教学目的。一旦创建了卷，在检查该卷时，你的 SSH 密码会以明文方式暴露在其中。完成示例后，请立即删除该卷。

1. Install the `sshfs` plugin.

   安装 `sshfs` 插件。

   ```console
   $ docker plugin install vieux/sshfs
   
   Plugin "vieux/sshfs" is requesting the following privileges:
   - network: [host]
   - capabilities: [CAP_SYS_ADMIN]
   Do you grant the above permissions? [y/N] y
   
   vieux/sshfs
   ```

   The plugin requests 2 privileges:

   ​	插件请求了两个权限：

   - It needs access to the `host` network.需要访问 `host` 网络。
   - It needs the `CAP_SYS_ADMIN` capability, which allows the plugin to run the `mount` command.需要 `CAP_SYS_ADMIN` 能力，允许插件运行 `mount` 命令。

2. Check that the plugin is enabled in the output of `docker plugin ls`.

   使用 `docker plugin ls` 检查插件是否已启用。

   ```console
   $ docker plugin ls
   
   ID                    NAME                  TAG                 DESCRIPTION                   ENABLED
   69553ca1d789          vieux/sshfs           latest              the `sshfs` plugin            true
   ```

3. Create a volume using the plugin. This example mounts the `/remote` directory on host `1.2.3.4` into a volume named `sshvolume`. 使用该插件创建一个卷。此示例将主机 `1.2.3.4` 上的 `/remote` 目录挂载为一个名为 `sshvolume` 的卷。

   This volume can now be mounted into containers.

   该卷现在可以被挂载到容器中使用。

   ```console
   $ docker volume create \
     -d vieux/sshfs \
     --name sshvolume \
     -o sshcmd=user@1.2.3.4:/remote \
     -o password=$(cat file_containing_password_for_remote_host)
   
   sshvolume
   ```

4. Verify that the volume was created successfully.

   验证卷已成功创建。

   ```console
   $ docker volume ls
   
   DRIVER              NAME
   vieux/sshfs         sshvolume
   ```

5. Start a container that uses the volume `sshvolume`.

   启动一个使用 `sshvolume` 卷的容器。

   ```console
   $ docker run --rm -v sshvolume:/data busybox ls /data
   
   <content of /remote on machine 1.2.3.4>
   ```

6. Remove the volume `sshvolume`

   删除卷 `sshvolume`

   ```console
   $ docker volume rm sshvolume
   
   sshvolume
   ```

To disable a plugin, use the `docker plugin disable` command. To completely remove it, use the `docker plugin remove` command. For other available commands and options, see the [command line reference]({{< ref "/reference/CLIreference/docker" >}}).

​	要禁用插件，使用 `docker plugin disable` 命令。要彻底删除插件，使用 `docker plugin remove` 命令。有关其他可用命令和选项，请参阅 [命令行参考]({{< ref "/reference/CLIreference/docker" >}})。

## 开发插件 Developing a plugin

#### The rootfs directory

The `rootfs` directory represents the root filesystem of the plugin. In this example, it was created from a Dockerfile:

​	`rootfs` 目录表示插件的根文件系统。在此示例中，它是通过 Dockerfile 创建的：

> **Note**
>
> The `/run/docker/plugins` directory is mandatory inside of the plugin's filesystem for Docker to communicate with the plugin.
>
> ​	插件文件系统内的 `/run/docker/plugins` 目录是必需的，以便 Docker 可以与插件通信。



```console
$ git clone https://github.com/vieux/docker-volume-sshfs
$ cd docker-volume-sshfs
$ docker build -t rootfsimage .
$ id=$(docker create rootfsimage true) # id was cd851ce43a403 when the image was created
$ sudo mkdir -p myplugin/rootfs
$ sudo docker export "$id" | sudo tar -x -C myplugin/rootfs
$ docker rm -vf "$id"
$ docker rmi rootfsimage
```

#### The config.json file

The `config.json` file describes the plugin. See the [plugins config reference]({{< ref "/manuals/DockerEngine/DockerEngineplugins/PluginConfigVersion1ofPluginV2" >}}).

​	`config.json` 文件描述了插件。请参阅 [插件配置参考]({{< ref "/manuals/DockerEngine/DockerEngineplugins/PluginConfigVersion1ofPluginV2" >}})。

Consider the following `config.json` file.

​	以下是一个 `config.json` 文件示例：

```json
{
  "description": "sshFS plugin for Docker",
  "documentation": "https://docs.docker.com/engine/extend/plugins/",
  "entrypoint": ["/docker-volume-sshfs"],
  "network": {
    "type": "host"
  },
  "interface": {
    "types": ["docker.volumedriver/1.0"],
    "socket": "sshfs.sock"
  },
  "linux": {
    "capabilities": ["CAP_SYS_ADMIN"]
  }
}
```

This plugin is a volume driver. It requires a `host` network and the `CAP_SYS_ADMIN` capability. It depends upon the `/docker-volume-sshfs` entrypoint and uses the `/run/docker/plugins/sshfs.sock` socket to communicate with Docker Engine. This plugin has no runtime parameters.

​	该插件是一个卷驱动程序。它需要 `host` 网络和 `CAP_SYS_ADMIN` 能力。依赖 `/docker-volume-sshfs` 作为入口点，并使用 `/run/docker/plugins/sshfs.sock` 套接字与 Docker Engine 通信。此插件没有运行时参数。

#### 创建插件 Creating the plugin

A new plugin can be created by running `docker plugin create <plugin-name> ./path/to/plugin/data` where the plugin data contains a plugin configuration file `config.json` and a root filesystem in subdirectory `rootfs`.

​	可以通过运行 `docker plugin create <plugin-name> ./path/to/plugin/data` 来创建新插件，其中插件数据包含 `config.json` 配置文件和 `rootfs` 子目录中的根文件系统。

After that the plugin `<plugin-name>` will show up in `docker plugin ls`. Plugins can be pushed to remote registries with `docker plugin push <plugin-name>`.

​	之后，插件 `<plugin-name>` 会显示在 `docker plugin ls` 中。插件可以使用 `docker plugin push <plugin-name>` 推送到远程注册表。

## 调试插件 Debugging plugins

Stdout of a plugin is redirected to dockerd logs. Such entries have a `plugin=<ID>` suffix. Here are a few examples of commands for pluginID `f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62` and their corresponding log entries in the docker daemon logs.

​	插件的 stdout 会重定向到 dockerd 日志。这些条目带有 `plugin=<ID>` 后缀。以下是插件 ID 为 `f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62` 的一些命令示例及其在 Docker 守护进程日志中的相应日志条目。

```console
$ docker plugin install tiborvass/sample-volume-plugin

INFO[0036] Starting...       Found 0 volumes on startup  plugin=f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62
```



```console
$ docker volume create -d tiborvass/sample-volume-plugin samplevol

INFO[0193] Create Called...  Ensuring directory /data/samplevol exists on host...  plugin=f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62
INFO[0193] open /var/lib/docker/plugin-data/local-persist.json: no such file or directory  plugin=f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62
INFO[0193]                   Created volume samplevol with mountpoint /data/samplevol  plugin=f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62
INFO[0193] Path Called...    Returned path /data/samplevol  plugin=f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62
```



```console
$ docker run -v samplevol:/tmp busybox sh

INFO[0421] Get Called...     Found samplevol                plugin=f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62
INFO[0421] Mount Called...   Mounted samplevol              plugin=f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62
INFO[0421] Path Called...    Returned path /data/samplevol  plugin=f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62
INFO[0421] Unmount Called... Unmounted samplevol            plugin=f52a3df433b9aceee436eaada0752f5797aab1de47e5485f1690a073b860ff62
```

#### 使用 `runc` 获取日志文件并进入插件的 shell - Using runc to obtain logfiles and shell into the plugin.

Use `runc`, the default docker container runtime, for debugging plugins by collecting plugin logs redirected to a file.

​	使用 `runc`（Docker 的默认容器运行时）来调试插件，将插件日志重定向到文件。

```console
$ sudo runc --root /run/docker/runtime-runc/plugins.moby list

ID                                                                 PID         STATUS      BUNDLE                                                                                                                                       CREATED                          OWNER
93f1e7dbfe11c938782c2993628c895cf28e2274072c4a346a6002446c949b25   15806       running     /run/docker/containerd/daemon/io.containerd.runtime.v1.linux/moby-plugins/93f1e7dbfe11c938782c2993628c895cf28e2274072c4a346a6002446c949b25   2018-02-08T21:40:08.621358213Z   root
9b4606d84e06b56df84fadf054a21374b247941c94ce405b0a261499d689d9c9   14992       running     /run/docker/containerd/daemon/io.containerd.runtime.v1.linux/moby-plugins/9b4606d84e06b56df84fadf054a21374b247941c94ce405b0a261499d689d9c9   2018-02-08T21:35:12.321325872Z   root
c5bb4b90941efcaccca999439ed06d6a6affdde7081bb34dc84126b57b3e793d   14984       running     /run/docker/containerd/daemon/io.containerd.runtime.v1.linux/moby-plugins/c5bb4b90941efcaccca999439ed06d6a6affdde7081bb34dc84126b57b3e793d   2018-02-08T21:35:12.321288966Z   root
```



```console
$ sudo runc --root /run/docker/runtime-runc/plugins.moby exec 93f1e7dbfe11c938782c2993628c895cf28e2274072c4a346a6002446c949b25 cat /var/log/plugin.log
```

If the plugin has a built-in shell, then exec into the plugin can be done as follows:

​	若插件内置 shell，可使用以下命令进入插件：

```console
$ sudo runc --root /run/docker/runtime-runc/plugins.moby exec -t 93f1e7dbfe11c938782c2993628c895cf28e2274072c4a346a6002446c949b25 sh
```

#### 使用 `curl` 调试插件套接字问题 Using curl to debug plugin socket issues.

To verify if the plugin API socket that the docker daemon communicates with is responsive, use curl. In this example, we will make API calls from the docker host to volume and network plugins using curl 7.47.0 to ensure that the plugin is listening on the said socket. For a well functioning plugin, these basic requests should work. Note that plugin sockets are available on the host under `/var/run/docker/plugins/<pluginID>`

​	要验证 Docker 守护进程与插件通信的 API socket 是否正常响应，可以使用 `curl`。在此示例中，我们从 Docker 主机对卷和网络插件使用 `curl 7.47.0` 发出 API 请求，以确保插件正在监听该 socket。对于功能正常的插件，这些基本请求应该能够成功。注意，插件 socket 位于主机的 `/var/run/docker/plugins/<pluginID>` 目录下。

​	

```console
$ curl -H "Content-Type: application/json" -XPOST -d '{}' --unix-socket /var/run/docker/plugins/e8a37ba56fc879c991f7d7921901723c64df6b42b87e6a0b055771ecf8477a6d/plugin.sock http:/VolumeDriver.List

{"Mountpoint":"","Err":"","Volumes":[{"Name":"myvol1","Mountpoint":"/data/myvol1"},{"Name":"myvol2","Mountpoint":"/data/myvol2"}],"Volume":null}
```



```console
$ curl -H "Content-Type: application/json" -XPOST -d '{}' --unix-socket /var/run/docker/plugins/45e00a7ce6185d6e365904c8bcf62eb724b1fe307e0d4e7ecc9f6c1eb7bcdb70/plugin.sock http:/NetworkDriver.GetCapabilities

{"Scope":"local"}
```

When using curl 7.5 and above, the URL should be of the form `http://hostname/APICall`, where `hostname` is the valid hostname where the plugin is installed and `APICall` is the call to the plugin API.

​	当使用 `curl 7.5` 及更高版本时，URL 格式应为 `http://hostname/APICall`，其中 `hostname` 是插件安装的有效主机名，`APICall` 是插件 API 的调用。

For example, `http://localhost/VolumeDriver.Li`

​	例如：`http://localhost/VolumeDriver.List`
