+++
title = "Splunk 日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/drivers/splunk/](https://docs.docker.com/engine/logging/drivers/splunk/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Splunk logging driver - Splunk 日志驱动

The `splunk` logging driver sends container logs to [HTTP Event Collector](https://dev.splunk.com/enterprise/docs/devtools/httpeventcollector/) in Splunk Enterprise and Splunk Cloud.

​	`splunk` 日志驱动将容器日志发送到 Splunk Enterprise 和 Splunk Cloud 中的 [HTTP 事件收集器](https://dev.splunk.com/enterprise/docs/devtools/httpeventcollector/)。

## 用法 Usage

You can configure Docker logging to use the `splunk` driver by default or on a per-container basis.

​	可以配置 Docker 日志以默认使用 `splunk` 驱动，或按每个容器进行配置。

To use the `splunk` driver as the default logging driver, set the keys `log-driver` and `log-opts` to appropriate values in the `daemon.json` configuration file and restart Docker. For example:

​	要将 `splunk` 驱动设置为默认日志驱动，请在 `daemon.json` 配置文件中设置 `log-driver` 和 `log-opts` 键，并重启 Docker。例如：



```json
{
  "log-driver": "splunk",
  "log-opts": {
    "splunk-token": "",
    "splunk-url": "",
    ...
  }
}
```

The daemon.json file is located in `/etc/docker/` on Linux hosts or `C:\ProgramData\docker\config\daemon.json` on Windows Server. For more about configuring Docker using `daemon.json`, see [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	在 Linux 主机上，`daemon.json` 文件位于 `/etc/docker/`，在 Windows Server 上位于 `C:\ProgramData\docker\config\daemon.json`。有关使用 `daemon.json` 配置 Docker 的更多信息，请参见 [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)。

> **Note**
>
> 
>
> `log-opts` configuration options in the `daemon.json` configuration file must be provided as strings. Boolean and numeric values (such as the value for `splunk-gzip` or `splunk-gzip-level`) must therefore be enclosed in quotes (`"`).
>
> ​	`daemon.json` 配置文件中的 `log-opts` 配置选项必须以字符串形式提供。布尔值和数值（如 `splunk-gzip` 或 `splunk-gzip-level` 的值）必须用引号（`"`）括起来。

To use the `splunk` driver for a specific container, use the commandline flags `--log-driver` and `log-opt` with `docker run`:

​	要为特定容器使用 `splunk` 驱动，请使用 `docker run` 的 `--log-driver` 和 `log-opt` 命令行标志：



```console
$ docker run --log-driver=splunk --log-opt splunk-token=VALUE --log-opt splunk-url=VALUE ...
```

## Splunk options

The following properties let you configure the Splunk logging driver.

​	以下属性可用于配置 Splunk 日志驱动：

- To configure the `splunk` driver across the Docker environment, edit `daemon.json` with the key, `"log-opts": {"NAME": "VALUE", ...}`.
  - 要在整个 Docker 环境中配置 `splunk` 驱动，请在 `daemon.json` 中编辑键 `"log-opts": {"NAME": "VALUE", ...}`。

- To configure the `splunk` driver for an individual container, use `docker run` with the flag, `--log-opt NAME=VALUE ...`.
  - 要为单个容器配置 `splunk` 驱动，请使用 `docker run` 的标志 `--log-opt NAME=VALUE ...`。


| Option                      | Required | Description                                                  |
| :-------------------------- | :------- | :----------------------------------------------------------- |
| `splunk-token`              | required | Splunk HTTP 事件收集器令牌。Splunk HTTP Event Collector token. |
| `splunk-url`                | required | Splunk Enterprise、Splunk Cloud 实例或托管集群的路径（包括端口和 HTTP 事件收集器使用的方案）。例如：`https://your_splunk_instance:8088`。Path to your Splunk Enterprise, self-service Splunk Cloud instance, or Splunk Cloud managed cluster (including port and scheme used by HTTP Event Collector) in one of the following formats: `https://your_splunk_instance:8088`, `https://input-prd-p-XXXXXXX.cloud.splunk.com:8088`, or `https://http-inputs-XXXXXXXX.splunkcloud.com`. |
| `splunk-source`             | optional | 事件源。Event source.                                        |
| `splunk-sourcetype`         | optional | 事件源类型。Event source type.                               |
| `splunk-index`              | optional | 事件源类型。Event index.                                     |
| `splunk-capath`             | optional | 根证书的路径。Path to root certificate.                      |
| `splunk-caname`             | optional | 用于验证服务器证书的名称，默认使用 `splunk-url` 的主机名。Name to use for validating server certificate; by default the hostname of the `splunk-url` is used. |
| `splunk-insecureskipverify` | optional | 忽略服务器证书验证。Ignore server certificate validation.    |
| `splunk-format`             | optional | 消息格式。可以为 `inline`、`json` 或 `raw`。默认值为 `inline`。Message format. Can be `inline`, `json` or `raw`. Defaults to `inline`. |
| `splunk-verify-connection`  | optional | 在启动时验证 Docker 是否可以连接到 Splunk 服务器。默认为 true。Verify on start, that Docker can connect to Splunk server. Defaults to true. |
| `splunk-gzip`               | optional | 启用/禁用 gzip 压缩以将事件发送到 Splunk。默认为 false。Enable/disable gzip compression to send events to Splunk Enterprise or Splunk Cloud instance. Defaults to false. |
| `splunk-gzip-level`         | optional | 设置 gzip 的压缩级别。有效值为 -1（默认）、0（无压缩）、1（最快速度）...9（最佳压缩）。Set compression level for gzip. Valid values are -1 (default), 0 (no compression), 1 (best speed) ... 9 (best compression). Defaults to [DefaultCompression](https://golang.org/pkg/compress/gzip/#DefaultCompression). |
| `tag`                       | optional | 指定消息的标签，支持某些标记。默认值为 `{{.ID}}`（容器 ID 的前 12 个字符）。有关日志标签格式的详细信息，请参见 [日志标签选项文档]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Specify tag for message, which interpret some markup. Default value is `{{.ID}}` (12 characters of the container ID). Refer to the [log tag option documentation]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}) for customizing the log tag format. |
| `labels`                    | optional | 逗号分隔的标签键列表。如果这些标签为容器指定，则将包含在消息中。Comma-separated list of keys of labels, which should be included in message, if these labels are specified for container. |
| `labels-regex`              | optional | 类似并兼容 `labels`。用于匹配日志相关标签的正则表达式。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Similar to and compatible with `labels`. A regular expression to match logging-related labels. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). |
| `env`                       | optional | 逗号分隔的环境变量键列表。如果这些变量为容器指定，则将包含在消息中。Comma-separated list of keys of environment variables, which should be included in message, if these variables are specified for container. |
| `env-regex`                 | optional | 类似并兼容 `env`。用于匹配日志相关环境变量的正则表达式。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Similar to and compatible with `env`. A regular expression to match logging-related environment variables. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). |

If there is collision between the `label` and `env` keys, the value of the `env` takes precedence. Both options add additional fields to the attributes of a logging message.

​	如果 `label` 和 `env` 键之间存在冲突，则 `env` 的值优先。两个选项都为日志消息的属性添加附加字段。

Below is an example of the logging options specified for the Splunk Enterprise instance. The instance is installed locally on the same machine on which the Docker daemon is running.

​	以下是指定 Splunk Enterprise 实例的日志选项示例。该实例安装在与 Docker 守护进程运行在同一台机器上。

The path to the root certificate and Common Name is specified using an HTTPS scheme. This is used for verification. The `SplunkServerDefaultCert` is automatically generated by Splunk certificates.

​	通过 HTTPS 方案指定根证书路径和通用名称，用于验证。`SplunkServerDefaultCert` 是 Splunk 证书自动生成的。



```console
$ docker run \
    --log-driver=splunk \
    --log-opt splunk-token=176FCEBF-4CF5-4EDF-91BC-703796522D20 \
    --log-opt splunk-url=https://splunkhost:8088 \
    --log-opt splunk-capath=/path/to/cert/cacert.pem \
    --log-opt splunk-caname=SplunkServerDefaultCert \
    --log-opt tag="{{.Name}}/{{.FullID}}" \
    --log-opt labels=location \
    --log-opt env=TEST \
    --env "TEST=false" \
    --label location=west \
    your/application
```

The `splunk-url` for Splunk instances hosted on Splunk Cloud is in a format like `https://http-inputs-XXXXXXXX.splunkcloud.com` and does not include a port specifier.

​	对于托管在 Splunk Cloud 上的 Splunk 实例，`splunk-url` 格式为 `https://http-inputs-XXXXXXXX.splunkcloud.com`，且不包括端口说明。

### 消息格式 Message formats

There are three logging driver messaging formats: `inline` (default), `json`, and `raw`.

​	日志驱动支持三种消息格式：`inline`（默认）、`json` 和 `raw`。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Inline " %}}

The default format is `inline` where each log message is embedded as a string. For example:

​	默认的 `inline` 格式将每条日志消息嵌入为字符串。例如：

```json
{
  "attrs": {
    "env1": "val1",
    "label1": "label1"
  },
  "tag": "MyImage/MyContainer",
  "source": "stdout",
  "line": "my message"
}
```



```json
{
  "attrs": {
    "env1": "val1",
    "label1": "label1"
  },
  "tag": "MyImage/MyContainer",
  "source": "stdout",
  "line": "{\"foo\": \"bar\"}"
}
```



{{% /tab  %}}

{{% tab header="JSON " %}}

To format messages as `json` objects, set `--log-opt splunk-format=json`. The driver attempts to parse every line as a JSON object and send it as an embedded object. If it can't parse the message, it's sent `inline`. For example:

​	要将消息格式化为 `json` 对象，设置 `--log-opt splunk-format=json`。驱动程序会尝试将每行解析为 JSON 对象并作为嵌入对象发送。如果无法解析消息，则会以内联方式发送。例如：

```json
{
  "attrs": {
    "env1": "val1",
    "label1": "label1"
  },
  "tag": "MyImage/MyContainer",
  "source": "stdout",
  "line": "my message"
}
```



```json
{
  "attrs": {
    "env1": "val1",
    "label1": "label1"
  },
  "tag": "MyImage/MyContainer",
  "source": "stdout",
  "line": {
    "foo": "bar"
  }
}
```

{{% /tab  %}}

{{% tab header="Raw" %}}

To format messages as `raw`, set `--log-opt splunk-format=raw`. Attributes (environment variables and labels) and tags are prefixed to the message. For example:

​	要将消息格式化为 `raw`，设置 `--log-opt splunk-format=raw`。属性（环境变量和标签）和标签将添加为消息前缀。例如：

```console
MyImage/MyContainer env1=val1 label1=label1 my message
MyImage/MyContainer env1=val1 label1=label1 {"foo": "bar"}
```

{{% /tab  %}}

{{< /tabpane >}}



------

## 高级选项 Advanced options

The Splunk logging driver lets you configure a few advanced options by setting environment variables for the Docker daemon.

​	Splunk 日志驱动允许通过为 Docker 守护进程设置环境变量来配置一些高级选项。

| Environment variable name                        | Default value | Description                                                  |
| :----------------------------------------------- | :------------ | :----------------------------------------------------------- |
| `SPLUNK_LOGGING_DRIVER_POST_MESSAGES_FREQUENCY`  | `5s`          | 等待更多消息以便批处理的时间。The time to wait for more messages to batch. |
| `SPLUNK_LOGGING_DRIVER_POST_MESSAGES_BATCH_SIZE` | `1000`        | 累积的消息数，以便一次性发送。The number of messages that should accumulate before sending them in one batch. |
| `SPLUNK_LOGGING_DRIVER_BUFFER_MAX`               | `10 * 1000`   | 重试时缓冲区中保持的最大消息数。The maximum number of messages held in buffer for retries. |
| `SPLUNK_LOGGING_DRIVER_CHANNEL_SIZE`             | `4 * 1000`    | 发送消息到后台记录器工作器所使用的通道中可以包含的最大挂起消息数，后台工作器会将消息批处理。The maximum number of pending messages that can be in the channel used to send messages to background logger worker, which batches them. |
