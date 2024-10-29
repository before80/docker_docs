+++
title = "使用远程日志驱动查看 Docker 日志"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/dual-logging/](https://docs.docker.com/engine/logging/dual-logging/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use docker logs with remote logging drivers - 使用远程日志驱动查看 Docker 日志

## 概述 Overview

You can use the `docker logs` command to read container logs regardless of the configured logging driver or plugin. Docker Engine uses the [`local`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}}) logging driver to act as cache for reading the latest logs of your containers. This is called dual logging. By default, the cache has log-file rotation enabled, and is limited to a maximum of 5 files of 20 MB each (before compression) per container.

​	您可以使用 `docker logs` 命令来读取容器日志，无论已配置的日志驱动或插件如何。Docker 引擎使用 [`local`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}}) 日志驱动作为缓存来读取容器的最新日志，这种机制称为双重日志记录。默认情况下，缓存启用日志文件轮转，限制每个容器最多保存 5 个大小为 20 MB（压缩前）的日志文件。

Refer to the [configuration options](https://docs.docker.com/engine/logging/dual-logging/#configuration-options) section to customize these defaults, or to the [disable dual logging](https://docs.docker.com/engine/logging/dual-logging/#disable-the-dual-logging-cache) section to disable this feature.

​	请参阅[配置选项](https://docs.docker.com/engine/logging/dual-logging/#configuration-options)部分以自定义这些默认设置，或[禁用双重日志记录](https://docs.docker.com/engine/logging/dual-logging/#disable-the-dual-logging-cache)部分以禁用该功能。

## 前置条件 Prerequisites

Docker Engine automatically enables dual logging if the configured logging driver doesn't support reading logs.

​	如果配置的日志驱动不支持读取日志，Docker 引擎会自动启用双重日志记录。

The following examples show the result of running a `docker logs` command with and without dual logging availability:

​	以下示例显示了启用和禁用双重日志记录时运行 `docker logs` 命令的结果：

### 不具备双重日志记录功能 Without dual logging capability

When a container is configured with a remote logging driver such as `splunk`, and dual logging is disabled, an error is displayed when attempting to read container logs locally:

​	当使用远程日志驱动（如 `splunk`）配置容器且禁用双重日志记录时，尝试本地读取容器日志会显示错误：

- Step 1: Configure Docker daemon 

  步骤 1：配置 Docker 守护进程

	```
  $ cat /etc/docker/daemon.json
  {
    "log-driver": "splunk",
    "log-opts": {
      "cache-disabled": "true",
      ... (options for "splunk" logging driver)
    }
  }
  ```

- Step 2: Start the container

  步骤 2：启动容器

  ```console
  $ docker run -d busybox --name testlog top
  ```

- Step 3: Read the container logs

  步骤 3：读取容器日志

  ```console
  $ docker logs 7d6ac83a89a0
  Error response from daemon: configured logging driver does not support reading
  ```

### 具备双重日志记录功能 With dual logging capability

With the dual logging cache enabled, the `docker logs` command can be used to read logs, even if the logging driver doesn't support reading logs. The following example shows a daemon configuration that uses the `splunk` remote logging driver as a default, with dual logging caching enabled:

​	启用双重日志记录缓存后，即使日志驱动不支持读取日志，您仍然可以使用 `docker logs` 命令读取日志。以下示例展示了使用 `splunk` 远程日志驱动作为默认配置，且启用双重日志记录缓存的守护进程配置：

- Step 1: Configure Docker daemon

  步骤 1：配置 Docker 守护进程

  ```console
  $ cat /etc/docker/daemon.json
  {
    "log-driver": "splunk",
    "log-opts": {
      ... (options for "splunk" logging driver)
    }
  }
  ```

- Step 2: Start the container

  步骤 2：启动容器

  ```console
  $ docker run -d busybox --name testlog top
  ```

- Step 3: Read the container logs

  步骤 3：读取容器日志

  ```console
  $ docker logs 7d6ac83a89a0
  2019-02-04T19:48:15.423Z [INFO]  core: marked as sealed
  2019-02-04T19:48:15.423Z [INFO]  core: pre-seal teardown starting
  2019-02-04T19:48:15.423Z [INFO]  core: stopping cluster listeners
  2019-02-04T19:48:15.423Z [INFO]  core: shutting down forwarding rpc listeners
  2019-02-04T19:48:15.423Z [INFO]  core: forwarding rpc listeners stopped
  2019-02-04T19:48:15.599Z [INFO]  core: rpc listeners successfully shut down
  2019-02-04T19:48:15.599Z [INFO]  core: cluster listeners successfully shut down
  ```

> **Note**
>
> 
>
> For logging drivers that support reading logs, such as the `local`, `json-file` and `journald` drivers, there is no difference in functionality before or after the dual logging capability became available. For these drivers, Logs can be read using `docker logs` in both scenarios.
>
> ​	对于支持读取日志的日志驱动（如 `local`、`json-file` 和 `journald`），在启用或禁用双重日志记录后，功能没有区别。对于这些驱动，可以在两种场景下使用 `docker logs` 读取日志。

### 配置选项 Configuration options

The dual logging cache accepts the same configuration options as the [`local` logging driver]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}}), but with a `cache-` prefix. These options can be specified per container, and defaults for new containers can be set using the [daemon configuration file](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	双重日志记录缓存接受与[`local` 日志驱动]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}})相同的配置选项，但选项前需添加 `cache-` 前缀。这些选项可以针对每个容器指定，也可以使用[守护进程配置文件](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)为新容器设置默认值。

By default, the cache has log-file rotation enabled, and is limited to a maximum of 5 files of 20MB each (before compression) per container. Use the configuration options described below to customize these defaults.

​	默认情况下，缓存启用日志文件轮转，每个容器最多保存 5 个大小为 20MB（压缩前）的日志文件。可以使用下方描述的配置选项自定义这些默认设置。

| Option           | Default   | Description                                                  |
| :--------------- | :-------- | :----------------------------------------------------------- |
| `cache-disabled` | `"false"` | 禁用本地缓存。布尔值作为字符串传递（`true`、`1`、`0` 或 `false`）。Disable local caching. Boolean value passed as a string (`true`, `1`, `0`, or `false`). |
| `cache-max-size` | `"20m"`   | 缓存旋转前的最大大小。正整数和单位修饰符（`k`、`m` 或 `g`）。The maximum size of the cache before it is rotated. A positive integer plus a modifier representing the unit of measure (`k`, `m`, or `g`). |
| `cache-max-file` | `"5"`     | 最大缓存文件数量。如果旋转日志创建超额文件，最旧的文件会被删除。正整数。The maximum number of cache files that can be present. If rotating the logs creates excess files, the oldest file is removed. A positive integer. |
| `cache-compress` | `"true"`  | 启用或禁用旋转日志文件的压缩。布尔值作为字符串传递（`true`、`1`、`0` 或 `false`）。Enable or disable compression of rotated log files. Boolean value passed as a string (`true`, `1`, `0`, or `false`). |

## 禁用双重日志记录缓存 Disable the dual logging cache

Use the `cache-disabled` option to disable the dual logging cache. Disabling the cache can be useful to save storage space in situations where logs are only read through a remote logging system, and if there is no need to read logs through `docker logs` for debugging purposes.

​	使用 `cache-disabled` 选项可以禁用双重日志记录缓存。在只通过远程日志系统读取日志，且无需通过 `docker logs` 进行调试的情况下，禁用缓存有助于节省存储空间。

Caching can be disabled for individual containers or by default for new containers, when using the [daemon configuration file](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	可以对单个容器禁用缓存，也可以在[守护进程配置文件](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)中为新容器设置默认禁用。

The following example uses the daemon configuration file to use the [`splunk`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Splunkloggingdriver" >}}) logging driver as a default, with caching disabled:

​	以下示例使用守护进程配置文件将 [`splunk`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Splunkloggingdriver" >}}) 日志驱动设置为默认，并禁用缓存：



```console
$ cat /etc/docker/daemon.json
{
  "log-driver": "splunk",
  "log-opts": {
    "cache-disabled": "true",
    ... (options for "splunk" logging driver)
  }
}
```

> **Note**
>
> 
>
> For logging drivers that support reading logs, such as the `local`, `json-file` and `journald` drivers, dual logging isn't used, and disabling the option has no effect.
>
> ​	对于支持读取日志的日志驱动（如 `local`、`json-file` 和 `journald`），不会使用双重日志记录，禁用该选项也无效果。

## 限制 Limitations

- If a container using a logging driver or plugin that sends logs remotely has a network issue, no `write` to the local cache occurs.
  - 如果使用远程日志驱动或插件的容器遇到网络问题，日志不会写入本地缓存。
- If a write to `logdriver` fails for any reason (file system full, write permissions removed), the cache write fails and is logged in the daemon log. The log entry to the cache isn't retried.
  - 如果由于某些原因（如文件系统已满、写权限被移除）导致写入 `logdriver` 失败，缓存写入也会失败，并记录到守护进程日志中。该缓存写入不重试。
- Some logs might be lost from the cache in the default configuration because a ring buffer is used to prevent blocking the stdio of the container in case of slow file writes. An admin must repair these while the daemon is shut down.
  - 在默认配置中，可能会有部分日志从缓存中丢失，因为使用环形缓冲区来防止在文件写入缓慢时阻塞容器的标准输入输出（stdio）。管理员需要在守护进程关闭时进行修复。
