+++
title = "Splunk logging driver"
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

# Splunk logging driver

The `splunk` logging driver sends container logs to [HTTP Event Collector](https://dev.splunk.com/enterprise/docs/devtools/httpeventcollector/) in Splunk Enterprise and Splunk Cloud.

## Usage

You can configure Docker logging to use the `splunk` driver by default or on a per-container basis.

To use the `splunk` driver as the default logging driver, set the keys `log-driver` and `log-opts` to appropriate values in the `daemon.json` configuration file and restart Docker. For example:



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

> **Note**
>
> 
>
> `log-opts` configuration options in the `daemon.json` configuration file must be provided as strings. Boolean and numeric values (such as the value for `splunk-gzip` or `splunk-gzip-level`) must therefore be enclosed in quotes (`"`).

To use the `splunk` driver for a specific container, use the commandline flags `--log-driver` and `log-opt` with `docker run`:



```console
$ docker run --log-driver=splunk --log-opt splunk-token=VALUE --log-opt splunk-url=VALUE ...
```

## Splunk options

The following properties let you configure the Splunk logging driver.

- To configure the `splunk` driver across the Docker environment, edit `daemon.json` with the key, `"log-opts": {"NAME": "VALUE", ...}`.
- To configure the `splunk` driver for an individual container, use `docker run` with the flag, `--log-opt NAME=VALUE ...`.

| Option                      | Required | Description                                                  |
| :-------------------------- | :------- | :----------------------------------------------------------- |
| `splunk-token`              | required | Splunk HTTP Event Collector token.                           |
| `splunk-url`                | required | Path to your Splunk Enterprise, self-service Splunk Cloud instance, or Splunk Cloud managed cluster (including port and scheme used by HTTP Event Collector) in one of the following formats: `https://your_splunk_instance:8088`, `https://input-prd-p-XXXXXXX.cloud.splunk.com:8088`, or `https://http-inputs-XXXXXXXX.splunkcloud.com`. |
| `splunk-source`             | optional | Event source.                                                |
| `splunk-sourcetype`         | optional | Event source type.                                           |
| `splunk-index`              | optional | Event index.                                                 |
| `splunk-capath`             | optional | Path to root certificate.                                    |
| `splunk-caname`             | optional | Name to use for validating server certificate; by default the hostname of the `splunk-url` is used. |
| `splunk-insecureskipverify` | optional | Ignore server certificate validation.                        |
| `splunk-format`             | optional | Message format. Can be `inline`, `json` or `raw`. Defaults to `inline`. |
| `splunk-verify-connection`  | optional | Verify on start, that Docker can connect to Splunk server. Defaults to true. |
| `splunk-gzip`               | optional | Enable/disable gzip compression to send events to Splunk Enterprise or Splunk Cloud instance. Defaults to false. |
| `splunk-gzip-level`         | optional | Set compression level for gzip. Valid values are -1 (default), 0 (no compression), 1 (best speed) ... 9 (best compression). Defaults to [DefaultCompression](https://golang.org/pkg/compress/gzip/#DefaultCompression). |
| `tag`                       | optional | Specify tag for message, which interpret some markup. Default value is `{{.ID}}` (12 characters of the container ID). Refer to the [log tag option documentation]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}) for customizing the log tag format. |
| `labels`                    | optional | Comma-separated list of keys of labels, which should be included in message, if these labels are specified for container. |
| `labels-regex`              | optional | Similar to and compatible with `labels`. A regular expression to match logging-related labels. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). |
| `env`                       | optional | Comma-separated list of keys of environment variables, which should be included in message, if these variables are specified for container. |
| `env-regex`                 | optional | Similar to and compatible with `env`. A regular expression to match logging-related environment variables. Used for advanced [log tag options]({{< ref "/manuals/DockerEngine/Logsandmetrics/Customizelogdriveroutput" >}}). |

If there is collision between the `label` and `env` keys, the value of the `env` takes precedence. Both options add additional fields to the attributes of a logging message.

Below is an example of the logging options specified for the Splunk Enterprise instance. The instance is installed locally on the same machine on which the Docker daemon is running.

The path to the root certificate and Common Name is specified using an HTTPS scheme. This is used for verification. The `SplunkServerDefaultCert` is automatically generated by Splunk certificates.



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

### Message formats

There are three logging driver messaging formats: `inline` (default), `json`, and `raw`.

Inline JSON Raw

------

The default format is `inline` where each log message is embedded as a string. For example:



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

------

## Advanced options

The Splunk logging driver lets you configure a few advanced options by setting environment variables for the Docker daemon.

| Environment variable name                        | Default value | Description                                                  |
| :----------------------------------------------- | :------------ | :----------------------------------------------------------- |
| `SPLUNK_LOGGING_DRIVER_POST_MESSAGES_FREQUENCY`  | `5s`          | The time to wait for more messages to batch.                 |
| `SPLUNK_LOGGING_DRIVER_POST_MESSAGES_BATCH_SIZE` | `1000`        | The number of messages that should accumulate before sending them in one batch. |
| `SPLUNK_LOGGING_DRIVER_BUFFER_MAX`               | `10 * 1000`   | The maximum number of messages held in buffer for retries.   |
| `SPLUNK_LOGGING_DRIVER_CHANNEL_SIZE`             | `4 * 1000`    | The maximum number of pending messages that can be in the channel used to send messages to background logger worker, which batches them. |
