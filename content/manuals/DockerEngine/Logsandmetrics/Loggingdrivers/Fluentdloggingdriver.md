+++
title = "Fluentd 日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/drivers/fluentd/](https://docs.docker.com/engine/logging/drivers/fluentd/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Fluentd logging driver - Fluentd 日志驱动

The `fluentd` logging driver sends container logs to the [Fluentd](https://www.fluentd.org/) collector as structured log data. Then, users can use any of the [various output plugins of Fluentd](https://www.fluentd.org/plugins) to write these logs to various destinations.

​	`fluentd` 日志驱动将容器日志作为结构化日志数据发送到 [Fluentd](https://www.fluentd.org/) 收集器，然后用户可以使用 Fluentd 的[各种输出插件](https://www.fluentd.org/plugins)将这些日志写入不同的目的地。

In addition to the log message itself, the `fluentd` log driver sends the following metadata in the structured log message:

​	除了日志消息本身，`fluentd` 日志驱动还会在结构化日志消息中发送以下元数据：

| Field            | Description                                                  |
| :--------------- | :----------------------------------------------------------- |
| `container_id`   | 完整的 64 字符容器 ID。The full 64-character container ID.   |
| `container_name` | 容器启动时的名称。如果您使用 `docker rename` 重命名容器，新名称不会反映在日志条目中。The container name at the time it was started. If you use `docker rename` to rename a container, the new name isn't reflected in the journal entries. |
| `source`         | `stdout` or `stderr`                                         |
| `log`            | 容器日志 The container log                                   |

## 用法 Usage

Some options are supported by specifying `--log-opt` as many times as needed:

​	可以通过多次指定 `--log-opt` 设置一些选项：

- `fluentd-address`: specify a socket address to connect to the Fluentd daemon, ex `fluentdhost:24224` or `unix:///path/to/fluentd.sock`.
  - `fluentd-address`：指定与 Fluentd 守护进程的连接地址，例如 `fluentdhost:24224` 或 `unix:///path/to/fluentd.sock`。

- `tag`: specify a tag for Fluentd messages. Supports some Go template markup, ex `{{.ID}}`, `{{.FullID}}` or `{{.Name}}` `docker.{{.ID}}`.
  - `tag`：为 Fluentd 消息指定标签，支持一些 Go 模板标记，例如 `{{.ID}}`、`{{.FullID}}` 或 `{{.Name}}` 等 `docker.{{.ID}}`。


To use the `fluentd` driver as the default logging driver, set the `log-driver` and `log-opt` keys to appropriate values in the `daemon.json` file, which is located in `/etc/docker/` on Linux hosts or `C:\ProgramData\docker\config\daemon.json` on Windows Server. For more about configuring Docker using `daemon.json`, see [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	要将 `fluentd` 驱动程序设置为默认日志驱动，请在 Linux 主机上的 `/etc/docker/` 或 Windows Server 上的 `C:\ProgramData\docker\config\daemon.json` 中的 `daemon.json` 文件中设置 `log-driver` 和 `log-opt`。有关使用 `daemon.json` 配置 Docker 的更多信息，请参见 [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)。

The following example sets the log driver to `fluentd` and sets the `fluentd-address` option.

​	以下示例将日志驱动设置为 `fluentd` 并设置 `fluentd-address` 选项：



```json
{
  "log-driver": "fluentd",
  "log-opts": {
    "fluentd-address": "fluentdhost:24224"
  }
}
```

Restart Docker for the changes to take effect.

​	重启 Docker 以使更改生效。

> **Note**
>
> 
>
> `log-opts` configuration options in the `daemon.json` configuration file must be provided as strings. Boolean and numeric values (such as the value for `fluentd-async` or `fluentd-max-retries`) must therefore be enclosed in quotes (`"`).
>
> ​	`daemon.json` 配置文件中的 `log-opts` 配置选项必须以字符串形式提供。布尔值和数值（如 `fluentd-async` 或 `fluentd-max-retries` 的值）必须用引号（`"`）括起来。

To set the logging driver for a specific container, pass the `--log-driver` option to `docker run`:

​	可以使用 `--log-driver` 选项为特定容器设置日志驱动：

```console
$ docker run --log-driver=fluentd ...
```

Before using this logging driver, launch a Fluentd daemon. The logging driver connects to this daemon through `localhost:24224` by default. Use the `fluentd-address` option to connect to a different address.

​	使用此日志驱动之前，先启动一个 Fluentd 守护进程。默认情况下，日志驱动通过 `localhost:24224` 连接到该守护进程。使用 `fluentd-address` 选项连接到其他地址：



```console
$ docker run --log-driver=fluentd --log-opt fluentd-address=fluentdhost:24224
```

If container cannot connect to the Fluentd daemon, the container stops immediately unless the `fluentd-async` option is used.

​	如果容器无法连接到 Fluentd 守护进程，除非使用了 `fluentd-async` 选项，否则容器会立即停止。

## Options

Users can use the `--log-opt NAME=VALUE` flag to specify additional Fluentd logging driver options.

​	用户可以使用 `--log-opt NAME=VALUE` 标志指定其他 Fluentd 日志驱动选项。

### fluentd-address

By default, the logging driver connects to `localhost:24224`. Supply the `fluentd-address` option to connect to a different address. `tcp`(default) and `unix` sockets are supported.

​	默认情况下，日志驱动连接到 `localhost:24224`。使用 `fluentd-address` 选项连接到其他地址。支持 `tcp`（默认）和 `unix` 套接字。



```console
$ docker run --log-driver=fluentd --log-opt fluentd-address=fluentdhost:24224
$ docker run --log-driver=fluentd --log-opt fluentd-address=tcp://fluentdhost:24224
$ docker run --log-driver=fluentd --log-opt fluentd-address=unix:///path/to/fluentd.sock
```

Two of the above specify the same address, because `tcp` is default.

​	其中两个命令指定相同的地址，因为 `tcp` 是默认协议。

### tag

By default, Docker uses the first 12 characters of the container ID to tag log messages. Refer to the [log tag option documentation]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}) for customizing the log tag format.

​	默认情况下，Docker 使用容器 ID 的前 12 个字符标记日志消息。有关自定义日志标签格式的详细信息，请参阅 [日志标签选项文档]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。

### labels, labels-regex, env, and env-regex

The `labels` and `env` options each take a comma-separated list of keys. If there is collision between `label` and `env` keys, the value of the `env` takes precedence. Both options add additional fields to the extra attributes of a logging message.

​	`labels` 和 `env` 选项各接受一个以逗号分隔的键列表。如果 `label` 和 `env` 键之间发生冲突，`env` 的值具有优先权。两个选项都会为日志消息的额外属性添加字段。

The `env-regex` and `labels-regex` options are similar to and compatible with respectively `env` and `labels`. Their values are regular expressions to match logging-related environment variables and labels. It is used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}).

​	`env-regex` 和 `labels-regex` 选项分别与 `env` 和 `labels` 兼容。它们的值是用于匹配与日志记录相关的环境变量和标签的正则表达式，用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。

### fluentd-async

Docker connects to Fluentd in the background. Messages are buffered until the connection is established. Defaults to `false`.

​	Docker 在后台连接到 Fluentd。消息会被缓冲，直到建立连接。默认为 `false`。

### fluentd-async-reconnect-interval

When `fluentd-async` is enabled, the `fluentd-async-reconnect-interval` option defines the interval, in milliseconds, at which the connection to `fluentd-address` is re-established. This option is useful if the address resolves to one or more IP addresses, for example a Consul service address.

​	当启用 `fluentd-async` 时，`fluentd-async-reconnect-interval` 选项定义重新连接到 `fluentd-address` 的间隔时间（毫秒）。如果地址解析为一个或多个 IP 地址，例如 Consul 服务地址，此选项很有用。

### fluentd-buffer-limit

Sets the number of events buffered on the memory. Records will be stored in memory up to this number. If the buffer is full, the call to record logs will fail. The default is 1048576. ( https://github.com/fluent/fluent-logger-golang/tree/master#bufferlimit)

​	设置内存中缓冲的事件数量。记录将在内存中存储到此数量为止。如果缓冲区已满，记录日志的调用将失败。默认值为 1048576。详见 https://github.com/fluent/fluent-logger-golang/tree/master#bufferlimit

### fluentd-retry-wait

How long to wait between retries. Defaults to 1 second.

​	重试之间的等待时间。默认为 1 秒。

### 

### fluentd-max-retries

The maximum number of retries. Defaults to `4294967295` (2**32 - 1).

​	最大重试次数。默认为 `4294967295` (2**32 - 1)。

### fluentd-sub-second-precision

Generates event logs in nanosecond resolution. Defaults to `false`.

​	以纳秒分辨率生成事件日志。默认为 `false`。

## 使用 Docker 管理 Fluentd 守护进程 Fluentd daemon management with Docker

About `Fluentd` itself, see [the project webpage](https://www.fluentd.org/) and [its documents](https://docs.fluentd.org/).

​	有关 `Fluentd` 本身的详细信息，请参见 [项目网页](https://www.fluentd.org/) 和[其文档](https://docs.fluentd.org/)。

To use this logging driver, start the `fluentd` daemon on a host. We recommend that you use [the Fluentd docker image](https://hub.docker.com/r/fluent/fluentd/). This image is especially useful if you want to aggregate multiple container logs on each host then, later, transfer the logs to another Fluentd node to create an aggregate store.

​	要使用此日志驱动，请在主机上启动 `fluentd` 守护进程。建议使用 [Fluentd Docker 镜像](https://hub.docker.com/r/fluent/fluentd/)。该镜像特别适用于在每个主机上聚合多个容器日志，然后将日志传输到另一个 Fluentd 节点以创建聚合存储。

### Test container loggers

1. Write a configuration file (`test.conf`) to dump input logs:

   编写配置文件（`test.conf`）以输出输入日志：

   ```text
   <source>
     @type forward
   </source>
   
   <match *>
     @type stdout
   </match>
   ```

2. Launch Fluentd container with this configuration file:

   使用该配置文件启动 Fluentd 容器：

   ```console
   $ docker run -it -p 24224:24224 -v /path/to/conf/test.conf:/fluentd/etc/test.conf -e FLUENTD_CONF=test.conf fluent/fluentd:latest
   ```

3. Start one or more containers with the `fluentd` logging driver:

   启动一个或多个使用 `fluentd` 日志驱动的容器：

   ```console
   $ docker run --log-driver=fluentd your/application
   ```
