+++
title = "Amazon CloudWatch Logs 日志驱动"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/drivers/awslogs/](https://docs.docker.com/engine/logging/drivers/awslogs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Amazon CloudWatch Logs logging driver - Amazon CloudWatch Logs 日志驱动

The `awslogs` logging driver sends container logs to [Amazon CloudWatch Logs](https://aws.amazon.com/cloudwatch/details/#log-monitoring). Log entries can be retrieved through the [AWS Management Console](https://console.aws.amazon.com/cloudwatch/home#logs:) or the [AWS SDKs and Command Line Tools](https://docs.aws.amazon.com/cli/latest/reference/logs/index.html).

​	`awslogs` 日志驱动将容器日志发送到 [Amazon CloudWatch Logs](https://aws.amazon.com/cloudwatch/details/#log-monitoring)。日志条目可以通过 [AWS 管理控制台](https://console.aws.amazon.com/cloudwatch/home#logs:) 或 [AWS SDK 和命令行工具](https://docs.aws.amazon.com/cli/latest/reference/logs/index.html) 来检索。

## 用法 Usage

To use the `awslogs` driver as the default logging driver, set the `log-driver` and `log-opt` keys to appropriate values in the `daemon.json` file, which is located in `/etc/docker/` on Linux hosts or `C:\ProgramData\docker\config\daemon.json` on Windows Server. For more about configuring Docker using `daemon.json`, see [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file). The following example sets the log driver to `awslogs` and sets the `awslogs-region` option.

​	要将 `awslogs` 设为默认日志驱动，请在 `daemon.json` 文件中设置 `log-driver` 和 `log-opt` 键。该文件位于 Linux 主机的 `/etc/docker/` 或 Windows Server 的 `C:\ProgramData\docker\config\daemon.json`。有关如何使用 `daemon.json` 配置 Docker 的更多信息，请参见 [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)。以下示例将日志驱动设置为 `awslogs`，并设置了 `awslogs-region` 选项。



```json
{
  "log-driver": "awslogs",
  "log-opts": {
    "awslogs-region": "us-east-1"
  }
}
```

Restart Docker for the changes to take effect.

​	重启 Docker 以使更改生效。

You can set the logging driver for a specific container by using the `--log-driver` option to `docker run`:

​	可以通过 `docker run` 的 `--log-driver` 选项为特定容器设置日志驱动：



```console
$ docker run --log-driver=awslogs ...
```

If you are using Docker Compose, set `awslogs` using the following declaration example:

​	如果使用 Docker Compose，可以如下声明 `awslogs`：



```yaml
myservice:
  logging:
    driver: awslogs
    options:
      awslogs-region: us-east-1
```

## Amazon CloudWatch Logs 选项 Amazon CloudWatch Logs options

You can add logging options to the `daemon.json` to set Docker-wide defaults, or use the `--log-opt NAME=VALUE` flag to specify Amazon CloudWatch Logs logging driver options when starting a container.

​	可以将日志选项添加到 `daemon.json` 以设置 Docker 全局默认值，也可以在启动容器时使用 `--log-opt NAME=VALUE` 参数指定 Amazon CloudWatch Logs 日志驱动选

### awslogs-region

The `awslogs` logging driver sends your Docker logs to a specific region. Use the `awslogs-region` log option or the `AWS_REGION` environment variable to set the region. By default, if your Docker daemon is running on an EC2 instance and no region is set, the driver uses the instance's region.

​	`awslogs` 日志驱动将 Docker 日志发送到特定区域。使用 `awslogs-region` 日志选项或 `AWS_REGION` 环境变量来设置区域。如果 Docker 守护进程在 EC2 实例上运行且未设置区域，驱动程序将使用实例的区域。



```console
$ docker run --log-driver=awslogs --log-opt awslogs-region=us-east-1 ...
```

### awslogs-endpoint

By default, Docker uses either the `awslogs-region` log option or the detected region to construct the remote CloudWatch Logs API endpoint. Use the `awslogs-endpoint` log option to override the default endpoint with the provided endpoint.

​	默认情况下，Docker 使用 `awslogs-region` 日志选项或检测到的区域来构造远程 CloudWatch Logs API 端点。使用 `awslogs-endpoint` 日志选项可以覆盖默认端点。

> **Note**
>
> 
>
> The `awslogs-region` log option or detected region controls the region used for signing. You may experience signature errors if the endpoint you've specified with `awslogs-endpoint` uses a different region.
>
> ​	`awslogs-region` 日志选项或检测到的区域控制用于签名的区域。如果您指定的 `awslogs-endpoint` 使用不同的区域，可能会遇到签名错误。

### awslogs-group

You must specify a [log group](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) for the `awslogs` logging driver. You can specify the log group with the `awslogs-group` log option:

​	必须为 `awslogs` 日志驱动指定一个 [日志组](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)。可以通过 `awslogs-group` 日志选项指定日志组：



```console
$ docker run --log-driver=awslogs --log-opt awslogs-region=us-east-1 --log-opt awslogs-group=myLogGroup ...
```

### awslogs-stream

To configure which [log stream](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) should be used, you can specify the `awslogs-stream` log option. If not specified, the container ID is used as the log stream.

​	要配置使用哪个 [日志流](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)，可以指定 `awslogs-stream` 日志选项。如果未指定，容器 ID 将用作日志流。

> **Note**
>
> 
>
> Log streams within a given log group should only be used by one container at a time. Using the same log stream for multiple containers concurrently can cause reduced logging performance.
>
> ​	同一日志组内的日志流应仅由一个容器同时使用。多容器同时使用相同的日志流可能会导致日志记录性能下降。

### awslogs-create-group

Log driver returns an error by default if the log group doesn't exist. However, you can set the `awslogs-create-group` to `true` to automatically create the log group as needed. The `awslogs-create-group` option defaults to `false`.

​	默认情况下，如果日志组不存在，日志驱动会返回错误。可以将 `wslogs-create-group` 设置为 `true`，以在需要时自动创建日志组。`awslogs-create-group` 选项默认值为 `false`。



```console
$ docker run \
    --log-driver=awslogs \
    --log-opt awslogs-region=us-east-1 \
    --log-opt awslogs-group=myLogGroup \
    --log-opt awslogs-create-group=true \
    ...
```

> **Note**
>
> 
>
> Your AWS IAM policy must include the `logs:CreateLogGroup` permission before you attempt to use `awslogs-create-group`.
>
> ​	您的 AWS IAM 策略必须包含 `logs:CreateLogGroup` 权限，才能使用 `awslogs-create-group`。

### awslogs-create-stream

By default, the log driver creates the AWS CloudWatch Logs stream used for container log persistence.

​	默认情况下，日志驱动会创建用于容器日志持久化的 AWS CloudWatch Logs 日志流。

Set `awslogs-create-stream` to `false` to disable log stream creation. When disabled, the Docker daemon assumes the log stream already exists. A use case where this is beneficial is when log stream creation is handled by another process avoiding redundant AWS CloudWatch Logs API calls.

​	将 `awslogs-create-stream` 设置为 `false` 可以禁用日志流创建。禁用时，Docker 守护进程假设日志流已存在。这在日志流创建由其他进程处理以避免重复的 AWS CloudWatch Logs API 调用的情况下很有用。

If `awslogs-create-stream` is set to `false` and the log stream does not exist, log persistence to CloudWatch fails during container runtime, resulting in `Failed to put log events` error messages in daemon logs.

​	如果 `awslogs-create-stream` 设置为 `false` 且日志流不存在，则在容器运行时日志无法持久化到 CloudWatch，导致守护进程日志中出现 `Failed to put log events` 错误消息。



```console
$ docker run \
    --log-driver=awslogs \
    --log-opt awslogs-region=us-east-1 \
    --log-opt awslogs-group=myLogGroup \
    --log-opt awslogs-stream=myLogStream \
    --log-opt awslogs-create-stream=false \
    ...
```

### awslogs-datetime-format

The `awslogs-datetime-format` option defines a multi-line start pattern in [Python `strftime` format](https://strftime.org/). A log message consists of a line that matches the pattern and any following lines that don't match the pattern. Thus the matched line is the delimiter between log messages.

​	`awslogs-datetime-format` 选项使用 [Python `strftime` 格式](https://strftime.org/) 定义多行起始模式。日志消息由匹配模式的行和随后不匹配模式的行组成。因此，匹配行是日志消息之间的分隔符。

One example of a use case for using this format is for parsing output such as a stack dump, which might otherwise be logged in multiple entries. The correct pattern allows it to be captured in a single entry.

​	一个使用该格式的示例用例是解析堆栈转储等输出，这可能会以多个条目记录下来。正确的模式可将其捕获为单个条目。

This option always takes precedence if both `awslogs-datetime-format` and `awslogs-multiline-pattern` are configured.

​	如果同时配置了 `awslogs-datetime-format` 和 `awslogs-multiline-pattern`，则此选项始终优先。

> **Note**
>
> 
>
> Multi-line logging performs regular expression parsing and matching of all log messages, which may have a negative impact on logging performance.
>
> ​	多行日志记录会对所有日志消息进行正则表达式解析和匹配，可能会对日志记录性能产生负面影响。

Consider the following log stream, where new log messages start with a timestamp:

​	考虑以下日志流，其中新日志消息以时间戳开始：



```console
[May 01, 2017 19:00:01] A message was logged
[May 01, 2017 19:00:04] Another multi-line message was logged
Some random message
with some random words
[May 01, 2017 19:01:32] Another message was logged
```

The format can be expressed as a `strftime` expression of `[%b %d, %Y %H:%M:%S]`, and the `awslogs-datetime-format` value can be set to that expression:

​	格式可以用 `strftime` 表达式 `[%b %d, %Y %H:%M:%S]` 表示，并将 `awslogs-datetime-format` 值设置为该表达式：



```console
$ docker run \
    --log-driver=awslogs \
    --log-opt awslogs-region=us-east-1 \
    --log-opt awslogs-group=myLogGroup \
    --log-opt awslogs-datetime-format='\[%b %d, %Y %H:%M:%S\]' \
    ...
```

This parses the logs into the following CloudWatch log events:

​	这会将日志解析为以下 CloudWatch 日志事件：



```console
# First event
[May 01, 2017 19:00:01] A message was logged

# Second event
[May 01, 2017 19:00:04] Another multi-line message was logged
Some random message
with some random words

# Third event
[May 01, 2017 19:01:32] Another message was logged
```

The following `strftime` codes are supported:

​	支持的 `strftime` 代码如下：

| Code | Meaning                                                      | Example  |
| :--- | :----------------------------------------------------------- | :------- |
| `%a` | 星期缩写名。Weekday abbreviated name.                        | Mon      |
| `%A` | 星期全称。Weekday full name.                                 | Monday   |
| `%w` | 星期作为十进制数，0 表示星期日，6 表示星期六。Weekday as a decimal number where 0 is Sunday and 6 is Saturday. | 0        |
| `%d` | 日期（以零填充的十进制数）。Day of the month as a zero-padded decimal number. | 08       |
| `%b` | 月份缩写名。Month abbreviated name.                          | Feb      |
| `%B` | 月份全称。Month full name.                                   | February |
| `%m` | 月份（以零填充的十进制数）。Month as a zero-padded decimal number. | 02       |
| `%Y` | 带世纪的年份。Year with century as a decimal number.         | 2008     |
| `%y` | 无世纪的年份（以零填充）。Year without century as a zero-padded decimal number. | 08       |
| `%H` | 小时（24 小时制，以零填充）。Hour (24-hour clock) as a zero-padded decimal number. | 19       |
| `%I` | 小时（12 小时制，以零填充）。Hour (12-hour clock) as a zero-padded decimal number. | 07       |
| `%p` | 上午或下午。AM or PM.                                        | AM       |
| `%M` | 分钟（以零填充）。Minute as a zero-padded decimal number.    | 57       |
| `%S` | 秒数（以零填充）。Second as a zero-padded decimal number.    | 04       |
| `%L` | 毫秒（以零填充）。Milliseconds as a zero-padded decimal number. | .123     |
| `%f` | 微秒（以零填充）。Microseconds as a zero-padded decimal number. | 000345   |
| `%z` | UTC 偏移量（格式为 +HHMM 或 -HHMM）。UTC offset in the form +HHMM or -HHMM. | +1300    |
| `%Z` | 时区名称。Time zone name.                                    | PST      |
| `%j` | 一年中的第几天（以零填充）。Day of the year as a zero-padded decimal number. | 363      |

### awslogs-multiline-pattern

The `awslogs-multiline-pattern` option defines a multi-line start pattern using a regular expression. A log message consists of a line that matches the pattern and any following lines that don't match the pattern. Thus the matched line is the delimiter between log messages.

​	`awslogs-multiline-pattern` 选项使用正则表达式定义多行起始模式。日志消息由匹配该模式的行和随后的不匹配行组成。匹配行为日志消息的分隔符。

This option is ignored if `awslogs-datetime-format` is also configured.

​	如果配置了 `awslogs-datetime-format`，则忽略此选项。

> **Note**
>
> 
>
> Multi-line logging performs regular expression parsing and matching of all log messages. This may have a negative impact on logging performance.
>
> ​	多行日志记录会对所有日志消息进行正则表达式解析和匹配，这可能会对日志性能产生负面影响。

Consider the following log stream, where each log message should start with the pattern `INFO`:

​	考虑以下日志流，其中每条日志消息应以 `INFO` 开头：



```console
INFO A message was logged
INFO Another multi-line message was logged
     Some random message
INFO Another message was logged
```

You can use the regular expression of `^INFO`:

​	可以使用正则表达式 `^INFO`：



```console
$ docker run \
    --log-driver=awslogs \
    --log-opt awslogs-region=us-east-1 \
    --log-opt awslogs-group=myLogGroup \
    --log-opt awslogs-multiline-pattern='^INFO' \
    ...
```

This parses the logs into the following CloudWatch log events:

​	这将日志解析为以下 CloudWatch 日志事件：



```console
# First event
INFO A message was logged

# Second event
INFO Another multi-line message was logged
     Some random message

# Third event
INFO Another message was logged
```

### tag

Specify `tag` as an alternative to the `awslogs-stream` option. `tag` interprets Go template markup, such as `{{.ID}}`, `{{.FullID}}` or `{{.Name}}` `docker.{{.ID}}`. See the [tag option documentation]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}) for details on supported template substitutions.

​	可以使用 `tag` 作为 `awslogs-stream` 选项的替代方法。`tag` 支持 Go 模板标记，例如 `{{.ID}}`、`{{.FullID}}` 或 `{{.Name}}`。详细信息请参阅 [标签选项文档]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}})。

When both `awslogs-stream` and `tag` are specified, the value supplied for `awslogs-stream` overrides the template specified with `tag`.

​	当同时指定了 `awslogs-stream` 和 `tag` 时，`awslogs-stream` 的值会覆盖 `tag` 指定的模板。

If not specified, the container ID is used as the log stream.

​	如果未指定，容器 ID 将用作日志流。

> **Note**
>
> 
>
> The CloudWatch log API doesn't support `:` in the log name. This can cause some issues when using the `{{ .ImageName }}` as a tag, since a Docker image has a format of `IMAGE:TAG`, such as `alpine:latest`. Template markup can be used to get the proper format. To get the image name and the first 12 characters of the container ID, you can use:
>
> ​	CloudWatch 日志 API 不支持在日志名称中使用 `:`。当使用 `{{ .ImageName }}` 作为标签时，可能会遇到问题，因为 Docker 镜像的格式为 `IMAGE:TAG`（例如 `alpine:latest`）。可以使用模板标记来格式化。例如，获取镜像名称和容器 ID 的前 12 个字符：
>
> 
>
> ```bash
> --log-opt tag='{{ with split .ImageName ":" }}{{join . "_"}}{{end}}-{{.ID}}'
> ```
>
> the output is something like: `alpine_latest-bf0072049c76`
>
> ​	输出格式类似于：`alpine_latest-bf0072049c76`

### awslogs-force-flush-interval-seconds

The `awslogs` driver periodically flushes logs to CloudWatch.

​	`awslogs` 驱动会定期将日志刷新到 CloudWatch。

The `awslogs-force-flush-interval-seconds` option changes log flush interval seconds. Default is 5 seconds.

​	`awslogs-force-flush-interval-seconds` 选项可更改日志刷新间隔，默认值为 5 秒。

### awslogs-max-buffered-events

The `awslogs` driver buffers logs.

​	`awslogs` 驱动会缓存日志。

The `awslogs-max-buffered-events` option changes log buffer size. Default is 4K.

​	`awslogs-max-buffered-events` 选项可更改日志缓冲区大小，默认值为 4K。

## 凭证 Credentials

You must provide AWS credentials to the Docker daemon to use the `awslogs` logging driver. You can provide these credentials with the `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN` environment variables, the default AWS shared credentials file (`~/.aws/credentials` of the root user), or if you are running the Docker daemon on an Amazon EC2 instance, the Amazon EC2 instance profile.

​	必须向 Docker 守护进程提供 AWS 凭证才能使用 `awslogs` 日志驱动。可以通过环境变量 `AWS_ACCESS_KEY_ID`、`AWS_SECRET_ACCESS_KEY` 和 `AWS_SESSION_TOKEN`，默认的 AWS 共享凭证文件（根用户的 `~/.aws/credentials`），或 Amazon EC2 实例上的 EC2 实例配置文件来提供这些凭证。

Credentials must have a policy applied that allows the `logs:CreateLogStream` and `logs:PutLogEvents` actions, as shown in the following example.

​	凭证必须有允许执行 `logs:CreateLogStream` 和 `logs:PutLogEvents` 操作的策略，如以下示例所示。



```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": ["logs:CreateLogStream", "logs:PutLogEvents"],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```
