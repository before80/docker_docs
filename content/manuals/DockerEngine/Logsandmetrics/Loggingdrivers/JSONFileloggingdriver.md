+++
title = "JSON 文件日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/drivers/json-file/](https://docs.docker.com/engine/logging/drivers/json-file/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# JSON File logging driver - JSON 文件日志驱动

By default, Docker captures the standard output (and standard error) of all your containers, and writes them in files using the JSON format. The JSON format annotates each line with its origin (`stdout` or `stderr`) and its timestamp. Each log file contains information about only one container.

​	默认情况下，Docker 捕获所有容器的标准输出（和标准错误）并使用 JSON 格式将其写入文件。JSON 格式为每一行注释其来源（`stdout` 或 `stderr`）以及时间戳。每个日志文件仅包含一个容器的信息。



```json
{
  "log": "Log line is here\n",
  "stream": "stdout",
  "time": "2019-01-01T11:11:11.111111111Z"
}
```

> **Warning**
>
> 
>
> The `json-file` logging driver uses file-based storage. These files are designed to be exclusively accessed by the Docker daemon. Interacting with these files with external tools may interfere with Docker's logging system and result in unexpected behavior, and should be avoided.
>
> ​	`json-file` 日志驱动使用基于文件的存储。这些文件设计为 Docker 守护进程专用访问。使用外部工具与这些文件交互可能会干扰 Docker 的日志系统并导致意外行为，应避免此操作。

## 用法 Usage

To use the `json-file` driver as the default logging driver, set the `log-driver` and `log-opts` keys to appropriate values in the `daemon.json` file, which is located in `/etc/docker/` on Linux hosts or `C:\ProgramData\docker\config\` on Windows Server. If the file does not exist, create it first. For more information about configuring Docker using `daemon.json`, see [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	要将 `json-file` 设置为默认日志驱动，请在 `daemon.json` 文件中设置 `log-driver` 和 `log-opt` 键。该文件位于 Linux 主机的 `/etc/docker/` 或 Windows Server 的 `C:\ProgramData\docker\config\` 中。如果该文件不存在，请先创建它。有关使用 `daemon.json` 配置 Docker 的更多信息，请参见 [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)。

The following example sets the log driver to `json-file` and sets the `max-size` and `max-file` options to enable automatic log-rotation.

​	以下示例将日志驱动设置为 `json-file` 并启用自动日志轮换，将 `max-size` 和 `max-file` 选项分别设置为 10M 和 3。



```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

> **Note**
>
> 
>
> `log-opts` configuration options in the `daemon.json` configuration file must be provided as strings. Boolean and numeric values (such as the value for `max-file` in the example above) must therefore be enclosed in quotes (`"`).
>
> ​	在 `daemon.json` 配置文件中的 `log-opts` 配置选项必须以字符串形式提供。因此，布尔值和数值（例如上例中的 `max-file` 值）必须加上引号（`"`）。

Restart Docker for the changes to take effect for newly created containers. Existing containers don't use the new logging configuration automatically.

​	重启 Docker 以使更改在新创建的容器中生效。现有容器不会自动使用新的日志配置。

You can set the logging driver for a specific container by using the `--log-driver` flag to `docker container create` or `docker run`:

​	可以通过在 `docker container create` 或 `docker run` 命令中使用 `--log-driver` 标志来为特定容器设置日志驱动：



```console
$ docker run \
      --log-driver json-file --log-opt max-size=10m \
      alpine echo hello world
```

### Options

The `json-file` logging driver supports the following logging options:

​	`json-file` 日志驱动支持以下日志选项：

| Option         | Description                                                  | Example value                                     |
| :------------- | :----------------------------------------------------------- | :------------------------------------------------ |
| `max-size`     | 日志滚动前的最大日志大小。一个正整数加上单位表示符（`k`、`m` 或 `g`）。默认值为 -1（无限制）。 The maximum size of the log before it is rolled. A positive integer plus a modifier representing the unit of measure (`k`, `m`, or `g`). Defaults to -1 (unlimited). | `--log-opt max-size=10m`                          |
| `max-file`     | 最多可以存在的日志文件数。如果滚动日志创建的文件超过此数量，最旧的文件将被删除。**仅在 `max-size` 也设置时生效**。正整数。默认值为 1。The maximum number of log files that can be present. If rolling the logs creates excess files, the oldest file is removed. **Only effective when `max-size` is also set.** A positive integer. Defaults to 1. | `--log-opt max-file=3`                            |
| `labels`       | 启动 Docker 守护进程时应用。一个以逗号分隔的日志相关标签列表。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Applies when starting the Docker daemon. A comma-separated list of logging-related labels this daemon accepts. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt labels=production_status,geo`          |
| `labels-regex` | 类似于且与 `labels` 兼容。匹配日志相关标签的正则表达式。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Similar to and compatible with `labels`. A regular expression to match logging-related labels. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt labels-regex=^(production_status|geo)` |
| `env`          | 启动 Docker 守护进程时应用。一个以逗号分隔的日志相关环境变量列表。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Applies when starting the Docker daemon. A comma-separated list of logging-related environment variables this daemon accepts. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt env=os,customer`                       |
| `env-regex`    | 类似于且与 `env` 兼容。匹配日志相关环境变量的正则表达式。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Similar to and compatible with `env`. A regular expression to match logging-related environment variables. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). | `--log-opt env-regex=^(os|customer)`              |
| `compress`     | 切换日志文件是否压缩。默认禁用。Toggles compression for rotated logs. Default is `disabled`. | `--log-opt compress=true`                         |

### Examples

This example starts an `alpine` container which can have a maximum of 3 log files no larger than 10 megabytes each.

​	此示例启动一个 `alpine` 容器，该容器最多可以有 3 个日志文件，每个文件不大于 10MB。

```console
$ docker run -it --log-opt max-size=10m --log-opt max-file=3 alpine ash
```
