+++
title = "Google Cloud 日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/drivers/gcplogs/](https://docs.docker.com/engine/logging/drivers/gcplogs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Google Cloud Logging driver - Google Cloud 日志驱动

The Google Cloud Logging driver sends container logs to [Google Cloud Logging](https://cloud.google.com/logging/docs/) Logging.

​	Google Cloud 日志驱动将容器日志发送到 [Google Cloud Logging](https://cloud.google.com/logging/docs/) 日志服务。

## 用法 Usage

To use the `gcplogs` driver as the default logging driver, set the `log-driver` and `log-opt` keys to appropriate values in the `daemon.json` file, which is located in `/etc/docker/` on Linux hosts or `C:\ProgramData\docker\config\daemon.json` on Windows Server. For more about configuring Docker using `daemon.json`, see [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	要将 `gcplogs` 设置为默认日志驱动，请在 `daemon.json` 文件中设置 `log-driver` 和 `log-opt` 键。该文件位于 Linux 主机的 `/etc/docker/` 或 Windows Server 的 `C:\ProgramData\docker\config\daemon.json`。有关使用 `daemon.json` 配置 Docker 的更多信息，请参见 [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)。

The following example sets the log driver to `gcplogs` and sets the `gcp-meta-name` option.

​	以下示例将日志驱动设置为 `gcplogs` 并设置了 `gcp-meta-name` 选项。



```json
{
  "log-driver": "gcplogs",
  "log-opts": {
    "gcp-meta-name": "example-instance-12345"
  }
}
```

Restart Docker for the changes to take effect.

​	重启 Docker 以使更改生效。

You can set the logging driver for a specific container by using the `--log-drive option to `docker run`:

​	可以通过 `docker run` 的 `--log-driver` 选项为特定容器设置日志驱动：



```console
$ docker run --log-driver=gcplogs ...
```

If Docker detects that it's running in a Google Cloud Project, it discovers configuration from the [instance metadata service](https://cloud.google.com/compute/docs/metadata). Otherwise, the user must specify which project to log to using the `--gcp-project` log option and Docker attempts to obtain credentials from the [Google Application Default Credential](https://developers.google.com/identity/protocols/application-default-credentials). The `--gcp-project` flag takes precedence over information discovered from the metadata server, so a Docker daemon running in a Google Cloud project can be overridden to log to a different project using `--gcp-project`.

​	如果 Docker 检测到它运行在 Google Cloud 项目中，它会从 [实例元数据服务](https://cloud.google.com/compute/docs/metadata)中发现配置。否则，用户必须使用 `--gcp-project` 日志选项指定要记录的项目，Docker 将尝试从 [Google 应用默认凭证](https://developers.google.com/identity/protocols/application-default-credentials)中获取凭证。`--gcp-project` 参数优先于从元数据服务器发现的信息，因此运行在 Google Cloud 项目中的 Docker 守护进程可以使用 `--gcp-project` 参数覆盖以记录到其他项目中。

Docker fetches the values for zone, instance name and instance ID from Google Cloud metadata server. Those values can be provided via options if metadata server isn't available. They don't override the values from metadata server.

​	Docker 从 Google Cloud 元数据服务器获取 zone、实例名称和实例 ID 的值。如果元数据服务器不可用，可以通过选项提供这些值。它们不会覆盖来自元数据服务器的值。

## gcplogs options

You can use the `--log-opt NAME=VALUE` flag to specify these additional Google Cloud Logging driver options:

​	可以使用 `--log-opt NAME=VALUE` 参数来指定这些 Google Cloud 日志驱动的其他选项：

| Option          | Required | Description                                                  |
| :-------------- | :------- | :----------------------------------------------------------- |
| `gcp-project`   | optional | 要记录的 Google Cloud 项目。默认从 Google Cloud 元数据服务器中发现该值。Which Google Cloud project to log to. Defaults to discovering this value from the Google Cloud metadata server. |
| `gcp-log-cmd`   | optional | 是否记录启动容器时的命令。默认值为 false。Whether to log the command that the container was started with. Defaults to false. |
| `labels`        | optional | 以逗号分隔的标签键列表，若容器指定了这些标签则将其包含在消息中。Comma-separated list of keys of labels, which should be included in message, if these labels are specified for the container. |
| `labels-regex`  | optional | 与 `labels` 类似且兼容。用于匹配日志相关标签的正则表达式。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Similar to and compatible with `labels`. A regular expression to match logging-related labels. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). |
| `env`           | optional | 以逗号分隔的环境变量键列表，若容器指定了这些变量则将其包含在消息中。Comma-separated list of keys of environment variables, which should be included in message, if these variables are specified for the container. |
| `env-regex`     | optional | 与 `env` 类似且兼容。用于匹配日志相关环境变量的正则表达式。用于高级 [日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Similar to and compatible with `env`. A regular expression to match logging-related environment variables. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). |
| `gcp-meta-zone` | optional | 实例的区域名称。Zone name for the instance.                  |
| `gcp-meta-name` | optional | 实例名称。Instance name.                                     |
| `gcp-meta-id`   | optional | 实例 ID。Instance ID.                                        |

If there is collision between `label` and `env` keys, the value of the `env` takes precedence. Both options add additional fields to the attributes of a logging message.

​	如果 `label` 和 `env` 键之间发生冲突，则 `env` 的值优先。两个选项都为日志消息的属性添加附加字段。

The following is an example of the logging options required to log to the default logging destination which is discovered by querying the Google Cloud metadata server.

​	以下是记录到默认日志目标（通过查询 Google Cloud 元数据服务器发现的目标）所需的日志选项示例：



```console
$ docker run \
    --log-driver=gcplogs \
    --log-opt labels=location \
    --log-opt env=TEST \
    --log-opt gcp-log-cmd=true \
    --env "TEST=false" \
    --label location=west \
    your/application
```

This configuration also directs the driver to include in the payload the label `location`, the environment variable `ENV`, and the command used to start the container.

​	该配置还指示驱动程序在日志中包含标签 `location`、环境变量 `TEST` 以及用于启动容器的命令。

The following example shows logging options for running outside of Google Cloud. The `GOOGLE_APPLICATION_CREDENTIALS` environment variable must be set for the daemon, for example via systemd:

​	以下示例展示了在 Google Cloud 之外运行时的日志选项。在守护进程中必须设置 `GOOGLE_APPLICATION_CREDENTIALS` 环境变量，例如通过 systemd：



```ini
[Service]
Environment="GOOGLE_APPLICATION_CREDENTIALS=uQWVCPkMTI34bpssr1HI"
```



```console
$ docker run \
    --log-driver=gcplogs \
    --log-opt gcp-project=test-project \
    --log-opt gcp-meta-zone=west1 \
    --log-opt gcp-meta-name=`hostname` \
    your/application
```
