+++
title = "Journald 日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/logging/drivers/journald/](https://docs.docker.com/engine/logging/drivers/journald/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Journald logging driver - Journald 日志驱动

The `journald` logging driver sends container logs to the [`systemd` journal](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html). Log entries can be retrieved using the `journalctl` command, through use of the `journal` API, or using the `docker logs` command.

​	`journald` 日志驱动将容器日志发送到 [`systemd` 日志](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html)。可以使用 `journalctl` 命令、通过 `journal` API 或使用 `docker logs` 命令检索日志条目。

In addition to the text of the log message itself, the `journald` log driver stores the following metadata in the journal with each message:

​	除了日志消息本身的文本外，`journald` 日志驱动还会将以下元数据与每条消息一起存储在日志中：

| Field                                | Description                                                  |
| :----------------------------------- | :----------------------------------------------------------- |
| `CONTAINER_ID`                       | 截取到 12 个字符的容器 ID。The container ID truncated to 12 characters. |
| `CONTAINER_ID_FULL`                  | 完整的 64 字符容器 ID。The full 64-character container ID.   |
| `CONTAINER_NAME`                     | 容器启动时的名称。如果使用 `docker rename` 重命名容器，新名称不会反映在日志条目中。The container name at the time it was started. If you use `docker rename` to rename a container, the new name isn't reflected in the journal entries. |
| `CONTAINER_TAG`, `SYSLOG_IDENTIFIER` | 容器标签 ([日志标签选项文档]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}))。The container tag ( [log tag option documentation]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})). |
| `CONTAINER_PARTIAL_MESSAGE`          | 用于标记日志完整性的字段，改善长日志行的记录。A field that flags log integrity. Improve logging of long log lines. |
| `IMAGE_NAME`                         | 容器镜像的名称。The name of the container image.             |

## 用法 Usage

To use the `journald` driver as the default logging driver, set the `log-driver` and `log-opts` keys to appropriate values in the `daemon.json` file, which is located in `/etc/docker/` on Linux hosts or `C:\ProgramData\docker\config\daemon.json` on Windows Server. For more about configuring Docker using `daemon.json`, see [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	要将 `journald` 作为默认日志驱动，请在 `daemon.json` 文件中设置 `log-driver` 和 `log-opts` 键。该文件位于 Linux 主机的 `/etc/docker/` 或 Windows Server 的 `C:\ProgramData\docker\config\daemon.json`。有关使用 `daemon.json` 配置 Docker 的详细信息，请参见 [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)。

The following example sets the log driver to `journald`:

​	以下示例将日志驱动设置为 `journald`：



```json
{
  "log-driver": "journald"
}
```

Restart Docker for the changes to take effect.

​	重启 Docker 使更改生效。

To configure the logging driver for a specific container, use the `--log-driver` flag on the `docker run` command.

​	要为特定容器配置日志驱动，请在 `docker run` 命令中使用 `--log-driver` 标志。



```console
$ docker run --log-driver=journald ...
```

## Options

Use the `--log-opt NAME=VALUE` flag to specify additional `journald` logging driver options.

​	使用 `--log-opt NAME=VALUE` 标志指定其他 `journald` 日志驱动选项。

| Option         | Required                                                     | Description                                                  |
| :------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `tag`          | optional                                                     | 指定模板以设置 journald 日志中的 `CONTAINER_TAG` 和 `SYSLOG_IDENTIFIER` 值。参考[日志标签选项文档]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})自定义日志标签格式。Specify template to set `CONTAINER_TAG` and `SYSLOG_IDENTIFIER` value in journald logs. Refer to [log tag option documentation]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}) to customize the log tag format. |
| `labels`       | optional                                                     | 以逗号分隔的标签键列表，如果为容器指定了这些标签，则应将其包含在消息中。Comma-separated list of keys of labels, which should be included in message, if these labels are specified for the container. |
| `labels-regex` | optional                                                     | 类似于并兼容 `labels`，匹配与日志相关的标签的正则表达式。用于高级[日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Similar to and compatible with labels. A regular expression to match logging-related labels. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). |
| `env`          | optional                                                     | 以逗号分隔的环境变量键列表，如果为容器指定了这些变量，则应将其包含在消息中。Comma-separated list of keys of environment variables, which should be included in message, if these variables are specified for the container. |
| `env-regex`    | xxxxxxxxxx1 1$ docker run -it --log-opt max-size=10m --log-opt max-file=3 alpine ashconsole | 类似于并兼容 `env`，匹配与日志相关的环境变量的正则表达式。用于高级[日志标签选项]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。Similar to and compatible with `env`. A regular expression to match logging-related environment variables. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). |

If a collision occurs between `label` and `env` options, the value of the `env` takes precedence. Each option adds additional fields to the attributes of a logging message.

​	如果 `label` 和 `env` 选项之间发生冲突，则 `env` 的值优先。每个选项会为日志消息的属性添加额外字段。

The following is an example of the logging options required to log to journald.

​	以下是将日志记录到 journald 所需的日志选项示例。



```console
$ docker run \
    --log-driver=journald \
    --log-opt labels=location \
    --log-opt env=TEST \
    --env "TEST=false" \
    --label location=west \
    your/application
```

This configuration also directs the driver to include in the payload the label location, and the environment variable `TEST`. If the `--env "TEST=false"` or `--label location=west` arguments were omitted, the corresponding key would not be set in the journald log.

​	此配置还指示驱动程序在日志负载中包含标签 `location` 和环境变量 `TEST`。如果省略了 `--env "TEST=false"` 或 `--label location=west` 参数，则相应的键不会在 journald 日志中设置。

## 关于容器名称的说明 Note regarding container names

The value logged in the `CONTAINER_NAME` field is the name of the container that was set at startup. If you use `docker rename` to rename a container, the new name isn't reflected in the journal entries. Journal entries continue to use the original name.

​	`CONTAINER_NAME` 字段中记录的值是启动时设置的容器名称。如果使用 `docker rename` 重命名容器，新名称不会反映在日志条目中。日志条目继续使用原始名称。

## 使用 `journalctl` 检索日志消息 Retrieve log messages with `journalctl`

Use the `journalctl` command to retrieve log messages. You can apply filter expressions to limit the retrieved messages to those associated with a specific container:

​	使用 `journalctl` 命令检索日志消息。可以应用过滤表达式，将检索到的消息限制为与特定容器关联的消息：



```console
$ sudo journalctl CONTAINER_NAME=webserver
```

You can use additional filters to further limit the messages retrieved. The `-b` flag only retrieves messages generated since the last system boot:

​	可以使用其他过滤器进一步限制检索到的消息。`-b` 标志仅检索自上次系统启动以来生成的消息：



```console
$ sudo journalctl -b CONTAINER_NAME=webserver
```

The `-o` flag specifies the format for the retrieved log messages. Use `-o json` to return the log messages in JSON format.

​	`-o` 标志指定检索到的日志消息的格式。使用 `-o json` 以 JSON 格式返回日志消息。



```console
$ sudo journalctl -o json CONTAINER_NAME=webserver
```

### 查看启用 TTY 的容器日志 View logs for a container with a TTY enabled

If TTY is enabled on a container you may see `[10B blob data]` in the output when retrieving log messages. The reason for that is that `\r` is appended to the end of the line and `journalctl` doesn't strip it automatically unless `--all` is set:

​	如果容器启用了 TTY，检索日志消息时可能会看到 `[10B blob data]`。这是因为 `\r` 被附加到行尾，`journalctl` 不会自动剥离它，除非设置 `--all`：



```console
$ sudo journalctl -b CONTAINER_NAME=webserver --all
```

## 使用 `journal` API 检索日志消息 Retrieve log messages with the `journal` API

This example uses the `systemd` Python module to retrie container logs:

​	以下示例使用 `systemd` Python 模块检索容器日志：



```python
import systemd.journal

reader = systemd.journal.Reader()
reader.add_match('CONTAINER_NAME=web')

for msg in reader:
    print '{CONTAINER_ID_FULL}: {MESSAGE}'.format(**msg)
```
