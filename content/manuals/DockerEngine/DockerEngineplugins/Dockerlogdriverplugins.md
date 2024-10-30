+++
title = "Docker 日志驱动插件"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/extend/plugins_logging/](https://docs.docker.com/engine/extend/plugins_logging/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker log driver plugins - Docker 日志驱动插件

This document describes logging driver plugins for Docker.

​	本文档介绍了 Docker 的日志驱动插件。

Logging drivers enables users to forward container logs to another service for processing. Docker includes several logging drivers as built-ins, however can never hope to support all use-cases with built-in drivers. Plugins allow Docker to support a wide range of logging services without requiring to embed client libraries for these services in the main Docker codebase. See the [plugin documentation]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}}) for more information.

​	日志驱动使用户能够将容器日志转发到另一个服务进行处理。Docker 包含了一些内置的日志驱动，但无法支持所有的使用场景。插件允许 Docker 支持多种日志服务，而无需将这些服务的客户端库嵌入到 Docker 的主代码库中。更多信息请参阅 [插件文档]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}})。

## 创建日志插件 Create a logging plugin

The main interface for logging plugins uses the same JSON+HTTP RPC protocol used by other plugin types. See the [example](https://github.com/cpuguy83/docker-log-driver-test) plugin for a reference implementation of a logging plugin. The example wraps the built-in `jsonfilelog` log driver.

​	日志插件的主要接口与其他插件类型使用相同的 JSON+HTTP RPC 协议。请参阅 [示例](https://github.com/cpuguy83/docker-log-driver-test) 插件以获取日志插件的参考实现。该示例封装了内置的 `jsonfilelog` 日志驱动。

## LogDriver 协议 LogDriver protocol

Logging plugins must register as a `LogDriver` during plugin activation. Once activated users can specify the plugin as a log driver.

​	日志插件在激活期间必须注册为 `LogDriver`。一旦激活，用户即可将该插件指定为日志驱动。

There are two HTTP endpoints that logging plugins must implement:

​	日志插件必须实现以下两个 HTTP 端点：

### `/LogDriver.StartLogging`

Signals to the plugin that a container is starting that the plugin should start receiving logs for.

​	通知插件某个容器正在启动，插件应开始接收该容器的日志。

Logs will be streamed over the defined file in the request. On Linux this file is a FIFO. Logging plugins are not currently supported on Windows.

​	日志将通过请求中定义的文件进行流传输。在 Linux 上，此文件为 FIFO。当前，Windows 上不支持日志插件。

Request:

​	请求：

```json
{
  "File": "/path/to/file/stream",
  "Info": {
          "ContainerID": "123456"
  }
}
```

`File` is the path to the log stream that needs to be consumed. Each call to `StartLogging` should provide a different file path, even if it's a container that the plugin has already received logs for prior. The file is created by Docker with a randomly generated name.

​	`File` 是要被插件消费的日志流路径。每次调用 `StartLogging` 应提供不同的文件路径，即使是同一个容器的日志。该文件由 Docker 创建，名称随机生成。

`Info` is details about the container that's being logged. This is fairly free-form, but is defined by the following struct definition:

​	`Info` 提供有关被记录容器的详细信息。其定义如下：



```go
type Info struct {
	Config              map[string]string
	ContainerID         string
	ContainerName       string
	ContainerEntrypoint string
	ContainerArgs       []string
	ContainerImageID    string
	ContainerImageName  string
	ContainerCreated    time.Time
	ContainerEnv        []string
	ContainerLabels     map[string]string
	LogPath             string
	DaemonName          string
}
```

`ContainerID` will always be supplied with this struct, but other fields may be empty or missing.

​	在这个结构中，`ContainerID` 总是会提供，但其他字段可能为空或缺失。

Response:

​	响应：

```json
{
  "Err": ""
}
```

If an error occurred during this request, add an error message to the `Err` field in the response. If no error then you can either send an empty response (`{}`) or an empty value for the `Err` field.

​	如果此请求期间发生错误，请在响应的 `Err` 字段中添加错误消息。如果没有错误，则可以发送空响应 (`{}`) 或将 `Err` 字段留空。

The driver should at this point be consuming log messages from the passed in file. If messages are unconsumed, it may cause the container to block while trying to write to its stdio streams.

​	在此时，驱动应开始从传入的文件中消费日志消息。如果消息未被消费，可能会导致容器在尝试写入 stdio 流时阻塞。

Log stream messages are encoded as protocol buffers. The protobuf definitions are in the [moby repository](https://github.com/moby/moby/blob/master/api/types/plugins/logdriver/entry.proto).

​	日志流消息以协议缓冲区（protobuf）编码。相关的 protobuf 定义可在 [moby 仓库](https://github.com/moby/moby/blob/master/api/types/plugins/logdriver/entry.proto) 中找到。

Since protocol buffers are not self-delimited you must decode them from the stream using the following stream format:

​	由于协议缓冲区不是自限定的，您必须使用以下流格式从流中解码它们：



```text
[size][message]
```

Where `size` is a 4-byte big endian binary encoded uint32. `size` in this case defines the size of the next message. `message` is the actual log entry.

​	其中 `size` 为 4 字节的大端编码 uint32，定义下一个消息的大小。`message` 是实际的日志条目。

A reference golang implementation of a stream encoder/decoder can be found [here](https://github.com/docker/docker/blob/master/api/types/plugins/logdriver/io.go)

​	参考的 Golang 流编码/解码器实现可以在[这里](https://github.com/docker/docker/blob/master/api/types/plugins/logdriver/io.go)找到。

### `/LogDriver.StopLogging`

Signals to the plugin to stop collecting logs from the defined file. Once a response is received, the file will be removed by Docker. You must make sure to collect all logs on the stream before responding to this request or risk losing log data.

​	向插件发出信号以停止从定义的文件中收集日志。一旦收到响应，该文件将被 Docker 删除。您必须确保在响应此请求之前收集流中所有日志，否则可能会导致日志数据丢失。

Requests on this endpoint does not mean that the container has been removed only that it has stopped.

​	此端点的请求并不意味着容器已被移除，仅表示其已停止。

Request:

​	请求：

```json
{
  "File": "/path/to/file/stream"
}
```

Response:

​	响应：

```json
{
  "Err": ""
}
```

If an error occurred during this request, add an error message to the `Err` field in the response. If no error then you can either send an empty response (`{}`) or an empty value for the `Err` field.

​	如果在此请求过程中发生错误，请将错误信息添加到响应中的 `Err` 字段。如果没有错误，可以返回一个空响应（`{}`）或在 `Err` 字段中设置为空值。

## 可选端点 Optional endpoints

Logging plugins can implement two extra logging endpoints:

​	日志插件可以实现两个额外的日志端点：

### `/LogDriver.Capabilities`

Defines the capabilities of the log driver. You must implement this endpoint for Docker to be able to take advantage of any of the defined capabilities.

​	定义日志驱动的功能。必须实现此端点，以便 Docker 能够利用任何已定义的功能。

Request:

​	请求：

```json
{}
```

Response:

​	响应：

```json
{
  "ReadLogs": true
}
```

Supported capabilities:

​	支持的功能：

- `ReadLogs` - this tells Docker that the plugin is capable of reading back logs to clients. Plugins that report that they support `ReadLogs` must implement the `/LogDriver.ReadLogs` endpoint
  - `ReadLogs` - 该功能指示 Docker 插件能够向客户端读取日志。报告支持 `ReadLogs` 的插件必须实现 `/LogDriver.ReadLogs` 端点。


### `/LogDriver.ReadLogs`

Reads back logs to the client. This is used when `docker logs <container>` is called.

​	向客户端读取回日志。在调用 `docker logs <container>` 时使用此端点。

In order for Docker to use this endpoint, the plugin must specify as much when `/LogDriver.Capabilities` is called.

​	为了让 Docker 使用此端点，插件必须在 `/LogDriver.Capabilities` 被调用时进行相应声明。

Request:

​	请求：

```json
{
  "ReadConfig": {},
  "Info": {
    "ContainerID": "123456"
  }
}
```

`ReadConfig` is the list of options for reading, it is defined with the following golang struct:

​	`ReadConfig` 是用于读取的选项列表，定义如下的 Golang 结构体：

```go
type ReadConfig struct {
	Since  time.Time
	Tail   int
	Follow bool
}
```

- `Since` defines the oldest log that should be sent.
  - `Since` 定义了应该发送的最早日志时间。

- `Tail` defines the number of lines to read (e.g. like the command `tail -n 10`)
  - `Tail` 定义读取的行数（例如类似于命令 `tail -n 10`）。

- `Follow` signals that the client wants to stay attached to receive new log messages as they come in once the existing logs have been read.
  - `Follow` 指示客户端希望保持连接，以便在现有日志读取完成后接收新的日志消息。


`Info` is the same type defined in `/LogDriver.StartLogging`. It should be used to determine what set of logs to read.

​	`Info` 与在 `/LogDriver.StartLogging` 中定义的类型相同，应用于确定读取的日志集。

Response:

​	响应：

```text
{{ log stream }}
```

The response should be the encoded log message using the same format as the messages that the plugin consumed from Docker.

​	响应应为已编码的日志消息，使用与插件从 Docker 消费的消息相同的格式。
