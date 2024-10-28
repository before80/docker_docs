+++
title = "Docker CLI 的 OpenTelemetry 支持"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/cli/otel/](https://docs.docker.com/engine/cli/otel/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# OpenTelemetry for the Docker CLI - Docker CLI 的 OpenTelemetry 支持

Introduced in Docker Engine version 26.1.0

​	引入于 Docker 引擎版本 26.1.0

The Docker CLI supports [OpenTelemetry](https://opentelemetry.io/docs/) instrumentation for emitting metrics about command invocations. This is disabled by default. You can configure the CLI to start emitting metrics to the endpoint that you specify. This allows you to capture information about your `docker` command invocations for more insight into your Docker usage.

​	Docker CLI 支持 [OpenTelemetry](https://opentelemetry.io/docs/) 仪表化，用于发出关于命令调用的指标。默认情况下这是禁用的。您可以配置 CLI 开始向指定的端点发出指标，从而捕获有关 `docker` 命令调用的信息，以更深入地了解您的 Docker 使用情况。

Exporting metrics is opt-in, and you control where data is being sent by specifying the destination address of the metrics collector.

​	导出指标是自愿加入的，您通过指定指标收集器的目标地址来控制数据发送位置。

## 什么是 OpenTelemetry？ What is OpenTelemetry?

OpenTelemetry, or OTel for short, is an open observability framework for creating and managing telemetry data, such as traces, metrics, and logs. OpenTelemetry is vendor- and tool-agnostic, meaning that it can be used with a broad variety of Observability backends.

​	OpenTelemetry，简称 OTel，是一个用于创建和管理遥测数据（如跟踪、指标和日志）的开放可观察性框架。OpenTelemetry 是供应商和工具无关的，这意味着它可以与各种可观察性后端一起使用。

Support for OpenTelemetry instrumentation in the Docker CLI means that the CLI can emit information about events that take place, using the protocols and conventions defined in the Open Telemetry specification.

​	Docker CLI 对 OpenTelemetry 的支持意味着 CLI 可以根据 OpenTelemetry 规范发出关于事件的信息。

## 工作原理 How it works

The Docker CLI doesn't emit telemetry data by default. Only if you've set an environment variable on your system will Docker CLI attempt to emit OpenTelemetry metrics, to the endpoint that you specify.

​	默认情况下，Docker CLI 不会发出遥测数据。只有在系统上设置了环境变量后，Docker CLI 才会尝试将 OpenTelemetry 指标发送到指定的端点。

```bash
DOCKER_CLI_OTEL_EXPORTER_OTLP_ENDPOINT=<endpoint>
```

The variable specifies the endpoint of an OpenTelemetry collector, where telemetry data about `docker` CLI invocation should be sent. To capture the data, you'll need an OpenTelemetry collector listening on that endpoint.

​	该变量指定 OpenTelemetry 收集器的端点，Docker CLI 调用的遥测数据将被发送至该端点。要捕获这些数据，您需要一个监听该端点的 OpenTelemetry 收集器。

The purpose of a collector is to receive the telemetry data, process it, and exports it to a backend. The backend is where the telemetry data gets stored. You can choose from a number of different backends, such as Prometheus or InfluxDB.

​	收集器的作用是接收遥测数据，对其进行处理并导出到后端，后端用于存储这些遥测数据。您可以选择 Prometheus 或 InfluxDB 等后端。

Some backends provide tools for visualizing the metrics directly. Alternatively, you can also run a dedicated frontend with support for generating more useful graphs, such as Grafana.

​	一些后端提供直接可视化指标的工具。或者，您也可以使用支持生成图形的独立前端工具，例如 Grafana。

## Setup

To get started capturing telemetry data for the Docker CLI, you'll need to:

​	要开始捕获 Docker CLI 的遥测数据，您需要：

- Set the `DOCKER_CLI_OTEL_EXPORTER_OTLP_ENDPOINT` environment variable to point to an OpenTelemetry collector endpoint
  - 将 `DOCKER_CLI_OTEL_EXPORTER_OTLP_ENDPOINT` 环境变量设置为 OpenTelemetry 收集器的端点。

- Run an OpenTelemetry collector that receives the signals from CLI command invocations
  - 运行一个接收 CLI 命令调用信号的 OpenTelemetry 收集器。

- Run a backend for storing the data received from the collector
  - 运行一个后端以存储从收集器接收到的数据。


The following Docker Compose file bootstraps a set of services to get started with OpenTelemetry. It includes an OpenTelemetry collector that the CLI can send metrics to, and a Prometheus backend that scrapes the metrics off the collector.

​	以下 Docker Compose 文件启动了一组用于开始 OpenTelemetry 的服务，包含一个 CLI 可发送指标的 OpenTelemetry 收集器和一个从收集器中抓取指标的 Prometheus 后端。

compose.yml



```yaml
name: cli-otel
services:
  prometheus:
    image: prom/prometheus
    command:
      - "--config.file=/etc/prometheus/prom.yml"
    ports:
      # Publish the Prometheus frontend on localhost:9091
      - 9091:9090
    restart: always
    volumes:
      # Store Prometheus data in a volume:
      - prom_data:/prometheus
      # Mount the prom.yml config file
      - ./prom.yml:/etc/prometheus/prom.yml
  otelcol:
    image: otel/opentelemetry-collector
    restart: always
    depends_on:
      - prometheus
    ports:
      - 4317:4317
    volumes:
      # Mount the otelcol.yml config file
      - ./otelcol.yml:/etc/otelcol/config.yaml

volumes:
  prom_data:
```

This service assumes that the following two configuration files exist alongside `compose.yml`:

​	该服务假设 `compose.yml` 文件旁存在以下两个配置文件：

- otelcol.yml

  ```yaml
  # Receive signals over gRPC and HTTP
  receivers:
    otlp:
      protocols:
        grpc:
        http:
  
  # Establish an endpoint for Prometheus to scrape from
  exporters:
    prometheus:
      endpoint: "0.0.0.0:8889"
  
  service:
    pipelines:
      metrics:
        receivers: [otlp]
        exporters: [prometheus]
  ```
  
- prom.yml

  ```yaml
  # Configure Prometheus to scrape the OpenTelemetry collector endpoint
  scrape_configs:
    - job_name: "otel-collector"
      scrape_interval: 1s
      static_configs:
        - targets: ["otelcol:8889"]
  ```

With these files in place:

​	在这些文件到位后：

1. Start the Docker Compose services:

   启动 Docker Compose 服务：

   ```console
   $ docker compose up
   ```

2. Configure Docker CLI to export telemetry to the OpenTelemetry collector.

   配置 Docker CLI 将遥测数据导出到 OpenTelemetry 收集器。

   ```console
   $ export DOCKER_CLI_OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4317
   ```

3. Run a `docker` command to trigger the CLI into sending a metric signal to the OpenTelemetry collector.

   运行 `docker` 命令以触发 CLI 发送指标信号到 OpenTelemetry 收集器。

   ```console
   $ docker version
   ```

4. To view telemetry metrics created by the CLI, open the Prometheus expression browser by going to http://localhost:9091/graph. 要查看 CLI 生成的遥测指标，请打开 Prometheus 表达式浏览器并访问 http://localhost:9091/graph。

5. In the **Query** field, enter `command_time_milliseconds_total`, and execute the query to see the telemetry data. 在 **Query** 字段中输入 `command_time_milliseconds_total`，并执行查询以查看遥测数据。

## 可用指标 Available metrics

Docker CLI currently exports a single metric, `command.time`, which measures the execution duration of a command in milliseconds. This metric has the following attributes:

​	Docker CLI 当前导出单一指标 `command.time`，用于测量命令执行的持续时间（单位为毫秒）。该指标具有以下属性：

- `command.name`: the name of the command
  - `command.name`：命令名称
- `command.status.code`: the exit code of the command
  - `command.status.code`：命令的退出代码
- `command.stderr.isatty`: true if stderr is attached to a TTY
  - `command.stderr.isatty`：如果 stderr 连接到 TTY，则为 true
- `command.stdin.isatty`: true if stdin is attached to a TTY
  - `command.stdin.isatty`：如果 stdin 连接到 TTY，则为 true
- `command.stdout.isatty`: true if stdout is attached to a TTY
  - `command.stdout.isatty`：如果 stdout 连接到 TTY，则为 true
