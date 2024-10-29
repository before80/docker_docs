+++
title = "Graylog 扩展格式日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/drivers/gelf/](https://docs.docker.com/engine/logging/drivers/gelf/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Graylog Extended Format logging driver - Graylog 扩展格式日志驱动

The `gelf` logging driver is a convenient format that's understood by a number of tools such as [Graylog](https://www.graylog.org/), [Logstash](https://www.elastic.co/products/logstash), and [Fluentd](https://www.fluentd.org/). Many tools use this format.

​	`gelf` 日志驱动是一种便捷的格式，适用于许多工具，例如 [Graylog](https://www.graylog.org/)、[Logstash](https://www.elastic.co/products/logstash) 和 [Fluentd](https://www.fluentd.org/)。许多工具都使用此格式。

In GELF, every log message is a dict with the following fields:

​	在 GELF 中，每条日志消息都是包含以下字段的字典：

- Version

- Host (who sent the message in the first place) （最初发送消息的主机）
- Timestamp
- Short and long version of the message 消息的短版本和长版本
- Any custom fields you configure yourself 消息的短版本和长版本

## 用法 Usage

To use the `gelf` driver as the default logging driver, set the `log-driver` and `log-opt` keys to appropriate values in the `daemon.json` file, which is located in `/etc/docker/` on Linux hosts or `C:\ProgramData\docker\config\daemon.json` on Windows Server. For more about configuring Docker using `daemon.json`, see [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	要将 `gelf` 驱动设置为默认日志驱动，请在 Linux 主机上的 `/etc/docker/` 或 Windows Server 上的 `C:\ProgramData\docker\config\daemon.json` 文件中设置 `log-driver` 和 `log-opt`。有关使用 `daemon.json` 配置 Docker 的更多信息，请参见 [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)。

The following example sets the log driver to `gelf` and sets the `gelf-address` option.

​	以下示例将日志驱动设置为 `gelf` 并设置 `gelf-address` 选项：



```json
{
  "log-driver": "gelf",
  "log-opts": {
    "gelf-address": "udp://1.2.3.4:12201"
  }
}
```

Restart Docker for the changes to take effect.

​	重启 Docker 以使更改生效。

> **Note**
>
> 
>
> `log-opts` configuration options in the `daemon.json` configuration file must be provided as strings. Boolean and numeric values (such as the value for `gelf-tcp-max-reconnect`) must therefore be enclosed in quotes (`"`).
>
> ​	`daemon.json` 配置文件中的 `log-opts` 配置选项必须以字符串形式提供。布尔值和数值（如 `gelf-tcp-max-reconnect` 的值）必须用引号（`"`）括起来。

You can set the logging driver for a specific container by setting the `--log-driver` flag when using `docker container create` or `docker run`:

​	可以在使用 `docker container create` 或 `docker run` 时，通过设置 `--log-driver` 标志为特定容器设置日志驱动：

```console
$ docker run \
      --log-driver gelf --log-opt gelf-address=udp://1.2.3.4:12201 \
      alpine echo hello world
```

### GELF options

The `gelf` logging driver supports the following options:

​	`gelf` 日志驱动支持以下选项：

| Option                     | Required | Description                                                  | Example value                                     |
| :------------------------- | :------- | :----------------------------------------------------------- | :------------------------------------------------ |
| `gelf-address`             | required | GELF 服务器的地址。`tcp` 和 `udp` 是唯一支持的 URI 指定器，必须指定端口。The address of the GELF server. `tcp` and `udp` are the only supported URI specifier and you must specify the port. | `--log-opt gelf-address=udp://192.168.0.42:12201` |
| `gelf-compression-type`    | optional | `仅限 UDP` GELF 驱动用于压缩每条日志消息的压缩类型。可选值为 `gzip`、`zlib` 和 `none`，默认值为 `gzip`。请注意，启用压缩会导致 CPU 使用率过高，因此强烈建议将其设置为 `none`。`UDP Only` The type of compression the GELF driver uses to compress each log message. Allowed values are `gzip`, `zlib` and `none`. The default is `gzip`. Note that enabled compression leads to excessive CPU usage, so it's highly recommended to set this to `none`. | `--log-opt gelf-compression-type=gzip`            |
| `gelf-compression-level`   | optional | `仅限 UDP` 当 `gelf-compression-type` 为 `gzip` 或 `zlib` 时的压缩级别。整数范围为 `-1` 至 `9`（最佳压缩）。默认值为 1（最佳速度）。较高的级别提供更高的压缩但速度较慢。`-1` 或 `0` 禁用压缩。`UDP Only` The level of compression when `gzip` or `zlib` is the `gelf-compression-type`. An integer in the range of `-1` to `9` (BestCompression). Default value is 1 (BestSpeed). Higher levels provide more compression at lower speed. Either `-1` or `0` disables compression. | `--log-opt gelf-compression-level=2`              |
| `gelf-tcp-max-reconnect`   | optional | `仅限 TCP` 连接断开时的最大重连次数。正整数。默认值为 3。`TCP Only` The maximum number of reconnection attempts when the connection drop. An positive integer. Default value is 3. | `--log-opt gelf-tcp-max-reconnect=3`              |
| `gelf-tcp-reconnect-delay` | optional | `仅限 TCP` 重连尝试之间的等待秒数。正整数。默认值为 1。`TCP Only` The number of seconds to wait between reconnection attempts. A positive integer. Default value is 1. | `--log-opt gelf-tcp-reconnect-delay=1`            |
| `tag`                      | optional | 一个字符串，会附加到 `gelf` 消息中的 `APP-NAME`。默认情况下，Docker 使用容器 ID 的前 12 个字符标记日志消息。有关自定义日志标签格式的详细信息，请参见 [日志标签选项文档]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。A string that's appended to the `APP-NAME` in the `gelf` message. By default, Docker uses the first 12 characters of the container ID to tag log messages. Refer to the [log tag option documentation]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}) for customizing the log tag format. | `--log-opt tag=mailer`                            |
| `labels`                   | optional | 启动 Docker 守护进程时应用。逗号分隔的日志相关标签列表，守护进程接受这些标签。将附加键添加到 `extra` 字段中，前缀为下划线（`_`）。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Applies when starting the Docker daemon. A comma-separated list of logging-related labels this daemon accepts. Adds additional key on the `extra` fields, prefixed by an underscore (`_`). Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt labels=production_status,geo`          |
| `labels-regex`             | optional | 类似并兼容 `labels`。用于匹配日志相关标签的正则表达式。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Similar to and compatible with `labels`. A regular expression to match logging-related labels. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt labels-regex=^(production_status|geo)` |
| `env`                      | optional | 启动 Docker 守护进程时应用。守护进程接受的日志相关环境变量的逗号分隔列表。将附加键添加到 `extra` 字段中，前缀为下划线（`_`）。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Applies when starting the Docker daemon. A comma-separated list of logging-related environment variables this daemon accepts. Adds additional key on the `extra` fields, prefixed by an underscore (`_`). Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt env=os,customer`                       |
| `env-regex`                | optional | 类似并兼容 `env`。用于匹配日志相关环境变量的正则表达式。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Similar to and compatible with `env`. A regular expression to match logging-related environment variables. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt env-regex=^(os|customer)`              |

> **Note**
>
> 
>
> The `gelf` driver doesn't support TLS for TCP connections. Messages sent to TLS-protected inputs can silently fail.
>
> ​	`gelf` 驱动不支持 TCP 连接的 TLS 加密。发送到 TLS 保护的输入的消息可能会悄无声息地失败。

### Examples

This example configures the container to use the GELF server running at `192.168.0.42` on port `12201`.

​	此示例将容器配置为使用运行在 `192.168.0.42` 上的 GELF 服务器，端口为 `12201`。



```console
$ docker run -dit \
    --log-driver=gelf \
    --log-opt gelf-address=udp://192.168.0.42:12201 \
    alpine sh
```
