+++
title = "配置日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/configure/](https://docs.docker.com/engine/logging/configure/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Configure logging drivers - 配置日志驱动

Docker includes multiple logging mechanisms to help you get information from running containers and services. These mechanisms are called logging drivers. Each Docker daemon has a default logging driver, which each container uses unless you configure it to use a different logging driver, or log driver for short.

​	Docker 包含多种日志机制，以帮助您从运行的容器和服务中获取信息。这些机制称为日志驱动。每个 Docker 守护进程都有一个默认日志驱动，除非您配置使用其他日志驱动，否则每个容器都会使用此默认日志驱动。

As a default, Docker uses the [`json-file` logging driver]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/JSONFileloggingdriver" >}}), which caches container logs as JSON internally. In addition to using the logging drivers included with Docker, you can also implement and use [logging driver plugins]({{< ref "/manuals/DockerEngine/Logsandmetrics/Usealoggingdriverplugin" >}}).

​	默认情况下，Docker 使用 [`json-file` 日志驱动]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/JSONFileloggingdriver" >}})，该驱动将容器日志作为 JSON 缓存在内部。除了使用 Docker 内置的日志驱动，您还可以实现和使用[日志驱动插件]({{< ref "/manuals/DockerEngine/Logsandmetrics/Usealoggingdriverplugin" >}})。

> **Tip: use the `local` logging driver to prevent disk-exhaustion 提示：使用 `local` 日志驱动防止磁盘空间耗尽**
>
> By default, no log-rotation is performed. As a result, log-files stored by the default [`json-file` logging driver]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/JSONFileloggingdriver" >}}) logging driver can cause a significant amount of disk space to be used for containers that generate much output, which can lead to disk space exhaustion.
>
> ​	默认情况下，不执行日志轮换。因此，默认的 [`json-file` 日志驱动]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/JSONFileloggingdriver" >}})存储的日志文件可能会导致大量磁盘空间被使用，尤其是对于生成大量输出的容器，这可能导致磁盘空间耗尽。
>
> Docker keeps the json-file logging driver (without log-rotation) as a default to remain backward compatibility with older versions of Docker, and for situations where Docker is used as runtime for Kubernetes.
>
> ​	Docker 保留了没有日志轮换功能的 `json-file` 作为默认驱动，以保持与 Docker 旧版本的向后兼容性，并适用于 Docker 作为 Kubernetes 运行时的情况。
>
> For other situations, the `local` logging driver is recommended as it performs log-rotation by default, and uses a more efficient file format. Refer to the [Configure the default logging driver](https://docs.docker.com/engine/logging/configure/#configure-the-default-logging-driver) section below to learn how to configure the `local` logging driver as a default, and the [local file logging driver]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}}) page for more details about the `local` logging driver.
>
> ​	对于其他情况，建议使用 `local` 日志驱动，因为它默认执行日志轮换，并使用更高效的文件格式。请参阅[配置默认日志驱动](https://docs.docker.com/engine/logging/configure/#configure-the-default-logging-driver)部分，了解如何将 `local` 设置为默认日志驱动，并访问[本地文件日志驱动页面]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}})以获取更多有关 `local` 日志驱动的详细信息。

## 配置默认日志驱动 Configure the default logging driver

To configure the Docker daemon to default to a specific logging driver, set the value of `log-driver` to the name of the logging driver in the `daemon.json` configuration file. Refer to the "daemon configuration file" section in the [`dockerd` reference manual](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file) for details.

​	要配置 Docker 守护进程默认使用的日志驱动，请在 `daemon.json` 配置文件中将 `log-driver` 的值设置为日志驱动的名称。详细信息请参阅 [`dockerd` 参考手册](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)中的“守护进程配置文件”部分。

The default logging driver is `json-file`. The following example sets the default logging driver to the [`local` log driver]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}}):

​	默认的日志驱动为 `json-file`。以下示例将默认日志驱动设置为 [`local` 日志驱动]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}})：



```json
{
  "log-driver": "local"
}
```

If the logging driver has configurable options, you can set them in the `daemon.json` file as a JSON object with the key `log-opts`. The following example sets four configurable options on the `json-file` logging driver:

​	如果日志驱动具有可配置选项，您可以在 `daemon.json` 文件中使用键 `log-opts` 设置这些选项，格式为 JSON 对象。以下示例在 `json-file` 日志驱动上设置了四个可配置选项：



```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3",
    "labels": "production_status",
    "env": "os,customer"
  }
}
```

Restart Docker for the changes to take effect for newly created containers. Existing containers don't use the new logging configuration automatically.

​	重新启动 Docker 以使更改对新创建的容器生效。现有容器不会自动使用新的日志配置。

> **Note**
>
> 
>
> `log-opts` configuration options in the `daemon.json` configuration file must be provided as strings. Boolean and numeric values (such as the value for `max-file` in the example above) must therefore be enclosed in quotes (`"`).
>
> ​	`daemon.json` 配置文件中的 `log-opts` 配置选项必须以字符串形式提供。布尔值和数字值（例如上例中的 `max-file` 值）必须加上引号（`"`）。

If you don't specify a logging driver, the default is `json-file`. To find the current default logging driver for the Docker daemon, run `docker info` and search for `Logging Driver`. You can use the following command on Linux, macOS, or PowerShell on Windows:

​	如果未指定日志驱动，默认使用 `json-file`。要查找 Docker 守护进程的当前默认日志驱动，请运行 `docker info` 并搜索 `Logging Driver`。在 Linux、macOS 或 Windows 的 PowerShell 上，可以使用以下命令：



```console
$ docker info --format '{{.LoggingDriver}}'

json-file
```

> **Note**
>
> 
>
> Changing the default logging driver or logging driver options in the daemon configuration only affects containers that are created after the configuration is changed. Existing containers retain the logging driver options that were used when they were created. To update the logging driver for a container, the container has to be re-created with the desired options. Refer to the [configure the logging driver for a container](https://docs.docker.com/engine/logging/configure/#configure-the-logging-driver-for-a-container) section below to learn how to find the logging-driver configuration of a container.
>
> ​	更改守护进程配置中的默认日志驱动或日志驱动选项仅影响更改配置后创建的容器。现有容器保留其创建时的日志驱动选项。要更新容器的日志驱动，必须使用所需的选项重新创建容器。请参阅[为容器配置日志驱动](https://docs.docker.com/engine/logging/configure/#configure-the-logging-driver-for-a-container)部分，了解如何查看容器的日志驱动配置。

## 为容器配置日志驱动 Configure the logging driver for a container

When you start a container, you can configure it to use a different logging driver than the Docker daemon's default, using the `--log-driver` flag. If the logging driver has configurable options, you can set them using one or more instances of the `--log-opt <NAME>=<VALUE>` flag. Even if the container uses the default logging driver, it can use different configurable options.

​	启动容器时，可以使用 `--log-driver` 标志将其配置为使用与 Docker 守护进程默认值不同的日志驱动。如果日志驱动具有可配置选项，可以使用一个或多个 `--log-opt <NAME>=<VALUE>` 标志设置它们。即使容器使用默认日志驱动，它也可以使用不同的可配置选项。

The following example starts an Alpine container with the `none` logging driver.

​	以下示例启动了一个使用 `none` 日志驱动的 Alpine 容器。



```console
$ docker run -it --log-driver none alpine ash
```

To find the current logging driver for a running container, if the daemon is using the `json-file` logging driver, run the following `docker inspect` command, substituting the container name or ID for `<CONTAINER>`:

​	要查找正在运行容器的当前日志驱动，如果守护进程使用的是 `json-file` 日志驱动，可以运行以下 `docker inspect` 命令，将 `<CONTAINER>` 替换为容器名称或 ID：

```console
$ docker inspect -f '{{.HostConfig.LogConfig.Type}}' CONTAINER

json-file
```

## 配置从容器到日志驱动的日志消息传递模式 Configure the delivery mode of log messages from container to log driver

Docker provides two modes for delivering messages from the container to the log driver:

​	Docker 提供了两种日志消息传递模式：

- (default) direct, blocking delivery from container to driver
  - 默认的直接阻塞传递

- non-blocking delivery that stores log messages in an intermediate per-container buffer for consumption by driver
  - 非阻塞传递，将日志消息存储在中间的每容器缓冲区中供驱动程序使用


The `non-blocking` message delivery mode prevents applications from blocking due to logging back pressure. Applications are likely to fail in unexpected ways when STDERR or STDOUT streams block.

​	`non-blocking` 消息传递模式可防止应用程序因日志写入压力而阻塞。STDERR 或 STDOUT 流阻塞时，应用程序可能会以意外方式失败。

> **Warning**
>
> 
>
> When the buffer is full, new messages will not be enqueued. Dropping messages is often preferred to blocking the log-writing process of an application.
>
> ​	当缓冲区已满时，新消息将不会排队。丢弃消息通常比阻塞应用程序的日志写入过程更可取。

The `mode` log option controls whether to use the `blocking` (default) or `non-blocking` message delivery.

​	`mode` 日志选项控制是否使用 `blocking`（默认）或 `non-blocking` 消息传递。

The `max-buffer-size` controls the size of the buffer used for intermediate message storage when `mode` is set to `non-blocking`. The default is `1m` meaning 1 MB (1 million bytes). See [function `FromHumanSize()` in the `go-units` package](https://pkg.go.dev/github.com/docker/go-units#FromHumanSize) for the allowed format strings, some examples are `1KiB` for 1024 bytes, `2g` for 2 billion bytes.

​	`max-buffer-size` 控制中间消息存储缓冲区的大小，当 `mode` 设置为 `non-blocking` 时使用。默认值为 `1m`（即 1 MB，1 百万字节）。有关允许的格式字符串，请参见 [`go-units` 包中的 `FromHumanSize()` 函数](https://pkg.go.dev/github.com/docker/go-units#FromHumanSize)，示例包括 `1KiB` 表示 1024 字节，`2g` 表示 2 亿字节。

The following example starts an Alpine container with log output in non-blocking mode and a 4 megabyte buffer:

​	以下示例启动了一个在非阻塞模式下输出日志并带有 4 MB 缓冲区的 Alpine 容器：

```console
$ docker run -it --log-opt mode=non-blocking --log-opt max-buffer-size=4m alpine ping 127.0.0.1
```

### 使用环境变量或标签与日志驱动 Use environment variables or labels with logging drivers

Some logging drivers add the value of a container's `--env|-e` or `--label` flags to the container's logs. This example starts a container using the Docker daemon's default logging driver (in the following example, `json-file`) but sets the environment variable `os=ubuntu`.

​	一些日志驱动添加容器的 `--env|-e` 或 `--label` 标志的值到容器日志中。此示例启动了一个使用 Docker 守护进程默认日志驱动的容器（以下示例为 `json-file`），但设置了环境变量 `os=ubuntu`。

```console
$ docker run -dit --label production_status=testing -e os=ubuntu alpine sh
```

If the logging driver supports it, this adds additional fields to the logging output. The following output is generated by the `json-file` logging driver:

​	如果日志驱动支持此功能，它会将其他字段添加到日志输出中。以下输出由 `json-file` 日志驱动生成：

```json
"attrs":{"production_status":"testing","os":"ubuntu"}
```

## 支持的日志驱动 Supported logging drivers

The following logging drivers are supported. See the link to each driver's documentation for its configurable options, if applicable. If you are using [logging driver plugins]({{< ref "/manuals/DockerEngine/Logsandmetrics/Usealoggingdriverplugin" >}}), you may see more options.

​	以下是支持的日志驱动。有关其可配置选项的详情，请参阅每个驱动的文档链接。如果您使用[日志驱动插件]({{< ref "/manuals/DockerEngine/Logsandmetrics/Usealoggingdriverplugin" >}})，您可能会看到更多选项。

| Driver                                                       | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `none`                                                       | 容器不可用日志，`docker logs` 不会返回任何输出。 No logs are available for the container and `docker logs` does not return any output. |
| [`local`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}}) | 以专门设计的格式存储日志，最小化开销。 Logs are stored in a custom format designed for minimal overhead. |
| [`json-file`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/JSONFileloggingdriver" >}}) | 日志格式为 JSON。Docker 的默认日志驱动。 The logs are formatted as JSON. The default logging driver for Docker. |
| [`syslog`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Syslogloggingdriver" >}}) | 将日志写入 `syslog` 服务。`syslog` 守护进程必须在主机上运行。 Writes logging messages to the `syslog` facility. The `syslog` daemon must be running on the host machine. |
| [`journald`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Journaldloggingdriver" >}}) | 将日志写入 `journald`。`journald` 守护进程必须在主机上运行。Writes log messages to `journald`. The `journald` daemon must be running on the host machine. |
| [`gelf`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/GraylogExtendedFormatloggingdriver" >}}) | 将日志写入 Graylog Extended Log Format (GELF) 端点，例如 Graylog 或 Logstash。 Writes log messages to a Graylog Extended Log Format (GELF) endpoint such as Graylog or Logstash. |
| [`fluentd`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Fluentdloggingdriver" >}}) | 将日志写入 `fluentd`（转发输入）。`fluentd` 守护进程必须在主机上运行。Writes log messages to `fluentd` (forward input). The `fluentd` daemon must be running on the host machine. |
| [`awslogs`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/AmazonCloudWatchLogsloggingdriver" >}}) | 将日志写入 Amazon CloudWatch Logs。Writes log messages to Amazon CloudWatch Logs. |
| [`splunk`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Splunkloggingdriver" >}}) | 使用 HTTP 事件收集器将日志写入 `splunk`。Writes log messages to `splunk` using the HTTP Event Collector. |
| [`etwlogs`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/ETWloggingdriver" >}}) | 将日志作为 Windows 平台的事件跟踪（ETW）事件写入，仅适用于 Windows 平台。Writes log messages as Event Tracing for Windows (ETW) events. Only available on Windows platforms. |
| [`gcplogs`]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/GoogleCloudLoggingdriver" >}}) | 将日志写入 Google Cloud Platform (GCP) 日志。Writes log messages to Google Cloud Platform (GCP) Logging. |

## 日志驱动的限制 Limitations of logging drivers

- Reading log information requires decompressing rotated log files, which causes a temporary increase in disk usage (until the log entries from the rotated files are read) and an increased CPU usage while decompressing.
  - 读取日志信息需要解压已轮换的日志文件，这会暂时增加磁盘使用量（直到读取完轮换文件中的日志条目）并在解压缩时增加 CPU 使用率。
- The capacity of the host storage where the Docker data directory resides determines the maximum size of the log file information.
  - Docker 数据目录所在的主机存储容量决定了日志文件信息的最大大小。
