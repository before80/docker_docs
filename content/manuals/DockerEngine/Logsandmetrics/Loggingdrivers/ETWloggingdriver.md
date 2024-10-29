+++
title = "ETW 日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/drivers/etwlogs/](https://docs.docker.com/engine/logging/drivers/etwlogs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# ETW logging driver - ETW 日志驱动

The Event Tracing for Windows (ETW) logging driver forwards container logs as ETW events. ETW stands for Event Tracing in Windows, and is the common framework for tracing applications in Windows. Each ETW event contains a message with both the log and its context information. A client can then create an ETW listener to listen to these events.

​	ETW（Event Tracing for Windows）日志驱动将容器日志转发为 ETW 事件。ETW 是 Windows 中用于应用程序跟踪的通用框架。每个 ETW 事件都包含一条包含日志和上下文信息的消息，客户端可以创建 ETW 监听器来接收这些事件。

The ETW provider that this logging driver registers with Windows, has the GUID identifier of: `{a3693192-9ed6-46d2-a981-f8226c8363bd}`. A client creates an ETW listener and registers to listen to events from the logging driver's provider. It doesn't matter the order in which the provider and listener are created. A client can create their ETW listener and start listening for events from the provider, before the provider has been registered with the system.

​	此日志驱动在 Windows 中注册的 ETW 提供程序的 GUID 标识符为 `{a3693192-9ed6-46d2-a981-f8226c8363bd}`。客户端可以创建 ETW 监听器，并注册以监听来自该日志驱动提供程序的事件。提供程序和监听器的创建顺序无关紧要。客户端可以先创建 ETW 监听器并开始监听提供程序的事件，即使该提供程序尚未在系统中注册。

## 用法 Usage

Here is an example of how to listen to these events using the logman utility program included in most installations of Windows:

​	以下是使用大多数 Windows 安装中包含的 `logman` 实用程序来监听这些事件的示例：

1. `logman start -ets DockerContainerLogs -p {a3693192-9ed6-46d2-a981-f8226c8363bd} 0 0 -o trace.etl`
2. Run your container(s) with the etwlogs driver, by adding `--log-driver=etwlogs` to the Docker run command, and generate log messages. 使用 `etwlogs` 驱动运行容器，在 Docker 运行命令中添加 `--log-driver=etwlogs`，并生成日志消息。
3. `logman stop -ets DockerContainerLogs`
4. This generates an etl file that contains the events. One way to convert this file into human-readable form is to run: `tracerpt -y trace.etl`. 这会生成包含事件的 etl 文件。可以通过运行 `tracerpt -y trace.etl` 将此文件转换为人类可读的格式。

Each ETW event contains a structured message string in this format:

​	每个 ETW 事件都包含以下格式的结构化消息字符串：



```text
container_name: %s, image_name: %s, container_id: %s, image_id: %s, source: [stdout | stderr], log: %s
```

Details on each item in the message can be found below:

​	消息中每项的详细信息如下：

| Field            | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `container_name` | 容器启动时的名称。The container name at the time it was started. |
| `image_name`     | 容器镜像的名称。The name of the container's image.           |
| `container_id`   | 完整的 64 字符容器 ID。The full 64-character container ID.   |
| `image_id`       | 容器镜像的完整 ID。The full ID of the container's image.     |
| `source`         | `stdout` 或 `stderr`。`stdout` or `stderr`.                  |
| `log`            | 容器日志消息。The container log message.                     |

Here is an example event message (output formatted for readability):

​	以下是事件消息的示例（输出为便于阅读进行了格式化）：

```yaml
container_name: backstabbing_spence,
image_name: windowsservercore,
container_id: f14bb55aa862d7596b03a33251c1be7dbbec8056bbdead1da8ec5ecebbe29731,
image_id: sha256:2f9e19bd998d3565b4f345ac9aaf6e3fc555406239a4fb1b1ba879673713824b,
source: stdout,
log: Hello world!
```

A client can parse this message string to get both the log message, as well as its context information. The timestamp is also available within the ETW event.

​	客户端可以解析该消息字符串，以获取日志消息及其上下文信息。时间戳也可在 ETW 事件中获取。

> **Note**
>
> 
>
> This ETW provider only emits a message string, and not a specially structured ETW event. Therefore, you don't have to register a manifest file with the system to read and interpret its ETW events.
>
> ​	该 ETW 提供程序仅发送消息字符串，而不是特定结构的 ETW 事件。因此，无需在系统中注册清单文件即可读取和解释其 ETW 事件。
