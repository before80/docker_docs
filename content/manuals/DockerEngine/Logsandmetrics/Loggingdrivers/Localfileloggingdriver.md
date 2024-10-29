+++
title = "本地文件日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/drivers/local/](https://docs.docker.com/engine/logging/drivers/local/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Local file logging driver - 本地文件日志驱动

The `local` logging driver captures output from container's stdout/stderr and writes them to an internal storage that's optimized for performance and disk use.

​	`local` 日志驱动捕获容器的 stdout/stderr 输出，并将其写入一个内部存储，该存储经过优化以提高性能和减少磁盘使用。

By default, the `local` driver preserves 100MB of log messages per container and uses automatic compression to reduce the size on disk. The 100MB default value is based on a 20M default size for each file and a default count of 5 for the number of such files (to account for log rotation).

​	默认情况下，`local` 驱动每个容器保留 100MB 的日志消息，并使用自动压缩来减少磁盘上的大小。100MB 的默认值基于每个文件的默认大小为 20M 和日志轮换的默认文件数为 5。

> **Warning**
>
> 
>
> The `local` logging driver uses file-based storage. These files are designed to be exclusively accessed by the Docker daemon. Interacting with these files with external tools may interfere with Docker's logging system and result in unexpected behavior, and should be avoided.
>
> ​	`local` 日志驱动使用基于文件的存储。这些文件被设计为仅由 Docker 守护进程专用访问。使用外部工具与这些文件交互可能会干扰 Docker 的日志系统并导致意外行为，应避免此操作。

## 用法 Usage

To use the `local` driver as the default logging driver, set the `log-driver` and `log-opt` keys to appropriate values in the `daemon.json` file, which is located in `/etc/docker/` on Linux hosts or `C:\ProgramData\docker\config\daemon.json` on Windows Server. For more about configuring Docker using `daemon.json`, see [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	要将 `local` 驱动设置为默认日志驱动，请在 `daemon.json` 文件中设置 `log-driver` 和 `log-opt` 键，该文件位于 Linux 主机的 `/etc/docker/` 或 Windows Server 的 `C:\ProgramData\docker\config\daemon.json`。有关使用 `daemon.json` 配置 Docker 的详细信息，请参见 [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)。

The following example sets the log driver to `local` and sets the `max-size` option.

​	以下示例将日志驱动设置为 `local` 并设置 `max-size` 选项。

```json
{
  "log-driver": "local",
  "log-opts": {
    "max-size": "10m"
  }
}
```

Restart Docker for the changes to take effect for newly created containers. Existing containers don't use the new logging configuration automatically.

​	重启 Docker 以使更改在新创建的容器中生效。现有容器不会自动使用新的日志配置。

You can set the logging driver for a specific container by using the `--log-driver` flag to `docker container create` or `docker run`:

​	可以通过在 `docker container create` 或 `docker run` 命令中使用 `--log-driver` 标志来为特定容器设置日志驱动：



```console
$ docker run \
      --log-driver local --log-opt max-size=10m \
      alpine echo hello world
```

Note that `local` is a bash reserved keyword, so you may need to quote it in scripts.

​	注意：`local` 是 bash 中的保留关键字，因此在脚本中可能需要将其用引号括起来。

### Options

The `local` logging driver supports the following logging options:

​	`local` 日志驱动支持以下日志选项：

| Option     | Description                                                  | Example value              |
| :--------- | :----------------------------------------------------------- | :------------------------- |
| `max-size` | 日志滚动前的最大日志大小。一个正整数加上单位表示符（`k`、`m` 或 `g`）。默认为 20m。The maximum size of the log before it's rolled. A positive integer plus a modifier representing the unit of measure (`k`, `m`, or `g`). Defaults to 20m. | `--log-opt max-size=10m`   |
| `max-file` | 日志文件的最大数量。如果滚动日志创建的文件超过此数量，最旧的文件将被删除。正整数。默认为 5。The maximum number of log files that can be present. If rolling the logs creates excess files, the oldest file is removed. A positive integer. Defaults to 5. | `--log-opt max-file=3`     |
| `compress` | 切换是否压缩滚动的日志文件。默认启用。Toggle compression of rotated log files. Enabled by default. | `--log-opt compress=false` |

### Examples

This example starts an `alpine` container which can have a maximum of 3 log files no larger than 10 megabytes each.

​	此示例启动一个 `alpine` 容器，该容器最多可以有 3 个日志文件，每个文件不大于 10MB。

```console
$ docker run -it --log-driver local --log-opt max-size=10m --log-opt max-file=3 alpine ash
```
