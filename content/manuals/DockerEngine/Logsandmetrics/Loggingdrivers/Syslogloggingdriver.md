+++
title = "Syslog 日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/drivers/syslog/](https://docs.docker.com/engine/logging/drivers/syslog/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Syslog logging driver - Syslog 日志驱动

The `syslog` logging driver routes logs to a `syslog` server. The `syslog` protocol uses a raw string as the log message and supports a limited set of metadata. The syslog message must be formatted in a specific way to be valid. From a valid message, the receiver can extract the following information:

​	`syslog` 日志驱动将日志路由到 `syslog` 服务器。`syslog` 协议使用原始字符串作为日志消息，并支持一组有限的元数据。要使 syslog 消息有效，必须以特定格式编写。通过有效消息，接收方可以提取以下信息：

- Priority: the logging level, such as `debug`, `warning`, `error`, `info`.
  - 优先级：日志级别，如 `debug`、`warning`、`error`、`info`。

- Timestamp: when the event occurred.
  - 时间戳：事件发生的时间。

- Hostname: where the event happened.
  - 主机名：事件发生的地点。

- Facility: which subsystem logged the message, such as `mail` or `kernel`.
  - 设备：记录消息的子系统，如 `mail` 或 `kernel`。

- Process name and process ID (PID): The name and ID of the process that generated the log.
  - 进程名称和进程 ID (PID)：生成日志的进程名称和 ID。


The format is defined in [RFC 5424](https://tools.ietf.org/html/rfc5424) and Docker's syslog driver implements the [ABNF reference](https://tools.ietf.org/html/rfc5424#section-6) in the following way:

​	格式定义在 [RFC 5424](https://tools.ietf.org/html/rfc5424) 中，Docker 的 syslog 驱动按 [ABNF 参考](https://tools.ietf.org/html/rfc5424#section-6) 实现如下：



```none
                TIMESTAMP SP HOSTNAME SP APP-NAME SP PROCID SP MSGID
                    +          +             +           |        +
                    |          |             |           |        |
                    |          |             |           |        |
       +------------+          +----+        |           +----+   +---------+
       v                            v        v                v             v
2017-04-01T17:41:05.616647+08:00 a.vm {taskid:aa,version:} 1787791 {taskid:aa,version:}
```

## 用法 Usage

To use the `syslog` driver as the default logging driver, set the `log-driver` and `log-opt` keys to appropriate values in the `daemon.json` file, which is located in `/etc/docker/` on Linux hosts or `C:\ProgramData\docker\config\daemon.json` on Windows Server. For more about configuring Docker using `daemon.json`, see [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	要将 `syslog` 设置为默认日志驱动，请在 `daemon.json` 文件中设置 `log-driver` 和 `log-opt` 键。该文件位于 Linux 主机的 `/etc/docker/` 或 Windows Server 的 `C:\ProgramData\docker\config\daemon.json`。关于如何使用 `daemon.json` 配置 Docker，参见 [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)。

The following example sets the log driver to `syslog` and sets the `syslog-address` option. The `syslog-address` options supports both UDP and TCP; this example uses UDP.

​	以下示例将日志驱动设置为 `syslog`，并设置 `syslog-address` 选项。`syslog-address` 支持 UDP 和 TCP，本例中使用 UDP。



```json
{
  "log-driver": "syslog",
  "log-opts": {
    "syslog-address": "udp://1.2.3.4:1111"
  }
}
```

Restart Docker for the changes to take effect.

​	重启 Docker 以使更改生效。

> **Note**
>
> 
>
> `log-opts` configuration options in the `daemon.json` configuration file must be provided as strings. Numeric and Boolean values (such as the value for `syslog-tls-skip-verify`) must therefore be enclosed in quotes (`"`).
>
> ​	`daemon.json` 配置文件中的 `log-opts` 选项必须作为字符串提供。数值和布尔值（如 `syslog-tls-skip-verify` 的值）需用引号 (`"`) 包围。

You can set the logging driver for a specific container by using the `--log-driver` flag to `docker container create` or `docker run`:

​	可以使用 `docker container create` 或 `docker run` 中的 `--log-driver` 标志为特定容器设置日志驱动：



```console
$ docker run \
      --log-driver syslog --log-opt syslog-address=udp://1.2.3.4:1111 \
      alpine echo hello world
```

## Options

The following logging options are supported as options for the `syslog` logging driver. They can be set as defaults in the `daemon.json`, by adding them as key-value pairs to the `log-opts` JSON array. They can also be set on a given container by adding a `--log-opt <key>=<value>` flag for each option when starting the container.

​	以下日志选项可以作为 `syslog` 日志驱动的选项设置。可以将它们作为默认值添加到 `daemon.json` 的 `log-opts` JSON 数组中，或在启动容器时通过添加 `--log-opt <key>=<value>` 标志设置。

| Option                   | Description                                                  | Example value                                                |
| :----------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `syslog-address`         | 外部 `syslog` 服务器地址。URI 规范可以是 `[tcpThe address of an external `syslog` server. The URI specifier may be `[tcp|udp|tcp+tls]://host:port`, `unix://path`, or `unixgram://path`. If the transport is `tcp`, `udp`, or `tcp+tls`, the default port is `514`. | `--log-opt syslog-address=tcp+tls://192.168.1.3:514`, `--log-opt syslog-address=unix:///tmp/syslog.sock` |
| `syslog-facility`        | 使用的 `syslog` 设备。可以是任何有效 `syslog` 设备的编号或名称。参见 [syslog 文档](https://tools.ietf.org/html/rfc5424#section-6.2.1)。The `syslog` facility to use. Can be the number or name for any valid `syslog` facility. See the [syslog documentation](https://tools.ietf.org/html/rfc5424#section-6.2.1). | `--log-opt syslog-facility=daemon`                           |
| `syslog-tls-ca-cert`     | CA 签名的信任证书的绝对路径。若地址协议不是 `tcp+tls`，则忽略。The absolute path to the trust certificates signed by the CA. Ignored if the address protocol isn't `tcp+tls`. | `--log-opt syslog-tls-ca-cert=/etc/ca-certificates/custom/ca.pem` |
| `syslog-tls-cert`        | TLS 证书文件的绝对路径。若地址协议不是 `tcp+tls`，则忽略。The absolute path to the TLS certificate file. Ignored if the address protocol isn't `tcp+tls`. | `--log-opt syslog-tls-cert=/etc/ca-certificates/custom/cert.pem` |
| `syslog-tls-key`         | TLS 密钥文件的绝对路径。若地址协议不是 `tcp+tls`，则忽略。The absolute path to the TLS key file. Ignored if the address protocol isn't `tcp+tls`. | `--log-opt syslog-tls-key=/etc/ca-certificates/custom/key.pem` |
| `syslog-tls-skip-verify` | 若设置为 `true`，则在连接到 `syslog` 守护进程时跳过 TLS 验证。默认为 `false`。若地址协议不是 `tcp+tls`，则忽略。If set to `true`, TLS verification is skipped when connecting to the `syslog` daemon. Defaults to `false`. Ignored if the address protocol isn't `tcp+tls`. | `--log-opt syslog-tls-skip-verify=true`                      |
| `tag`                    | 附加到 `syslog` 消息 `APP-NAME` 的字符串。默认情况下，Docker 使用容器 ID 的前 12 个字符作为日志消息的标签。有关自定义日志标签格式的信息，请参考 [日志标签选项文档]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。A string that's appended to the `APP-NAME` in the `syslog` message. By default, Docker uses the first 12 characters of the container ID to tag log messages. Refer to the [log tag option documentation]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}) for customizing the log tag format. | `--log-opt tag=mailer`                                       |
| `syslog-format`          | 使用的 `syslog` 消息格式。若未指定，则使用本地 Unix syslog 格式且无主机名。可以指定 `rfc3164` 为 RFC-3164 兼容格式，`rfc5424` 为 RFC-5424 兼容格式，`rfc5424micro` 为带微秒时间戳的 RFC-5424 兼容格式。The `syslog` message format to use. If not specified the local Unix syslog format is used, without a specified hostname. Specify `rfc3164` for the RFC-3164 compatible format, `rfc5424` for RFC-5424 compatible format, or `rfc5424micro` for RFC-5424 compatible format with microsecond timestamp resolution. | `--log-opt syslog-format=rfc5424micro`                       |
| `labels`                 | 启动 Docker 守护进程时应用的日志相关标签的逗号分隔列表。用于高级[日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Applies when starting the Docker daemon. A comma-separated list of logging-related labels this daemon accepts. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt labels=production_status,geo`                     |
| `labels-regex`           | 启动 Docker 守护进程时应用的日志相关标签的正则表达式。类似于并兼容 `labels`。用于高级[日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Applies when starting the Docker daemon. Similar to and compatible with `labels`. A regular expression to match logging-related labels. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt labels-regex=^(production_status|geo)`            |
| `env`                    | 启动 Docker 守护进程时应用的日志相关环境变量的逗号分隔列表。用于高级[日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Applies when starting the Docker daemon. A comma-separated list of logging-related environment variables this daemon accepts. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt env=os,customer`                                  |
| `env-regex`              | 启动 Docker 守护进程时应用的日志相关环境变量的正则表达式。类似于并兼容 `env`。用于高级[日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Applies when starting the Docker daemon. Similar to and compatible with `env`. A regular expression to match logging-related environment variables. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt env-regex=^(os|customer)`                         |

