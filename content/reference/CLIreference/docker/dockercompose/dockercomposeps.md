+++
title = "docker compose ps"
date = 2024-10-23T14:54:43+08:00
weight = 140
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/ps/](https://docs.docker.com/reference/cli/docker/compose/ps/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose ps

| Description | List containers                            |
| :---------- | ------------------------------------------ |
| Usage       | `docker compose ps [OPTIONS] [SERVICE...]` |

## Description

Lists containers for a Compose project, with current status and exposed ports.

​	列出 Compose 项目的容器，显示当前状态和暴露的端口。



```console
$ docker compose ps
NAME            IMAGE     COMMAND           SERVICE    CREATED         STATUS          PORTS
example-foo-1   alpine    "/entrypoint.…"   foo        4 seconds ago   Up 2 seconds    0.0.0.0:8080->80/tcp
```

By default, only running containers are shown. `--all` flag can be used to include stopped containers.

​	默认情况下，仅显示正在运行的容器。可以使用 `--all` 标志包括已停止的容器。



```console
$ docker compose ps --all
NAME            IMAGE     COMMAND           SERVICE    CREATED         STATUS          PORTS
example-foo-1   alpine    "/entrypoint.…"   foo        4 seconds ago   Up 2 seconds    0.0.0.0:8080->80/tcp
example-bar-1   alpine    "/entrypoint.…"   bar        4 seconds ago   exited (0)
```

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `-a, --all`                                                  |         | 显示所有已停止的容器（包括由 run 命令创建的容器） Show all stopped containers (including those created by the run command) |
| [`--filter`](https://docs.docker.com/reference/cli/docker/compose/ps/#filter) |         | 按属性过滤服务（支持的过滤器：状态） Filter services by a property (supported filters: status) |
| [`--format`](https://docs.docker.com/reference/cli/docker/compose/ps/#format) | `table` | 使用自定义模板格式化输出： 'table': 以表格格式打印输出（默认） 'table TEMPLATE': 使用给定的 Go 模板以表格格式打印输出 'json': 以 JSON 格式打印 'TEMPLATE': 使用给定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/  - Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `--no-trunc`                                                 |         | 不截断输出 Don't truncate output                             |
| `--orphans`                                                  | `true`  | 包括孤立的服务（项目中未声明的） Include orphaned services (not declared by project) |
| `-q, --quiet`                                                |         | 仅显示 ID - Only display IDs                                 |
| `--services`                                                 |         | 显示服务 Display services                                    |
| [`--status`](https://docs.docker.com/reference/cli/docker/compose/ps/#status) |         | 按状态过滤服务。可选值：[paused \| restarting \| removing \| running \| dead \| created \| exited]  - Filter services by status. Values: [paused \| restarting \| removing \| running \| dead \| created \| exited] |

## Examples

### 格式化输出（`--format`） Format the output (`--format`)

By default, the `docker compose ps` command uses a table ("pretty") format to show the containers. The `--format` flag allows you to specify alternative presentations for the output. Currently, supported options are `pretty` (default), and `json`, which outputs information about the containers as a JSON array:

​	默认情况下，`docker compose ps` 命令使用表格（“美化”）格式显示容器。`--format` 标志允许您指定输出的其他展示形式。目前支持的选项是 `pretty`（默认）和 `json`，以 JSON 数组格式输出有关容器的信息：



```console
$ docker compose ps --format json
[{"ID":"1553b0236cf4d2715845f053a4ee97042c4f9a2ef655731ee34f1f7940eaa41a","Name":"example-bar-1","Command":"/docker-entrypoint.sh nginx -g 'daemon off;'","Project":"example","Service":"bar","State":"exited","Health":"","ExitCode":0,"Publishers":null},{"ID":"f02a4efaabb67416e1ff127d51c4b5578634a0ad5743bd65225ff7d1909a3fa0","Name":"example-foo-1","Command":"/docker-entrypoint.sh nginx -g 'daemon off;'","Project":"example","Service":"foo","State":"running","Health":"","ExitCode":0,"Publishers":[{"URL":"0.0.0.0","TargetPort":80,"PublishedPort":8080,"Protocol":"tcp"}]}]
```

The JSON output allows you to use the information in other tools for further processing, for example, using the [`jq` utility](https://stedolan.github.io/jq/) to pretty-print the JSON:

​	JSON 输出允许您在其他工具中使用这些信息进行进一步处理，例如，使用 [`jq` 工具](https://stedolan.github.io/jq/) 美化打印 JSON：



```console
$ docker compose ps --format json | jq .
[
  {
    "ID": "1553b0236cf4d2715845f053a4ee97042c4f9a2ef655731ee34f1f7940eaa41a",
    "Name": "example-bar-1",
    "Command": "/docker-entrypoint.sh nginx -g 'daemon off;'",
    "Project": "example",
    "Service": "bar",
    "State": "exited",
    "Health": "",
    "ExitCode": 0,
    "Publishers": null
  },
  {
    "ID": "f02a4efaabb67416e1ff127d51c4b5578634a0ad5743bd65225ff7d1909a3fa0",
    "Name": "example-foo-1",
    "Command": "/docker-entrypoint.sh nginx -g 'daemon off;'",
    "Project": "example",
    "Service": "foo",
    "State": "running",
    "Health": "",
    "ExitCode": 0,
    "Publishers": [
      {
        "URL": "0.0.0.0",
        "TargetPort": 80,
        "PublishedPort": 8080,
        "Protocol": "tcp"
      }
    ]
  }
]
```

### 按状态过滤容器（`--status`） Filter containers by status (`--status`)

Use the `--status` flag to filter the list of containers by status. For example, to show only containers that are running or only containers that have exited:

​	使用 `--status` 标志按状态过滤容器列表。例如，仅显示正在运行的容器或仅显示已退出的容器：



```console
$ docker compose ps --status=running
NAME            IMAGE     COMMAND           SERVICE    CREATED         STATUS          PORTS
example-foo-1   alpine    "/entrypoint.…"   foo        4 seconds ago   Up 2 seconds    0.0.0.0:8080->80/tcp

$ docker compose ps --status=exited
NAME            IMAGE     COMMAND           SERVICE    CREATED         STATUS          PORTS
example-bar-1   alpine    "/entrypoint.…"   bar        4 seconds ago   exited (0)
```

### 按状态过滤容器（`--filter`） Filter containers by status (`--filter`)

The [`--status` flag](https://docs.docker.com/reference/cli/docker/compose/ps/#status) is a convenient shorthand for the `--filter status=<status>` flag. The example below is the equivalent to the example from the previous section, this time using the `--filter` flag:

​	[`--status` 标志](https://docs.docker.com/reference/cli/docker/compose/ps/#status) 是 `--filter status=<status>` 标志的便捷表示法。下面的示例与前一节的示例等效，这次使用 `--filter` 标志：



```console
$ docker compose ps --filter status=running
NAME            IMAGE     COMMAND           SERVICE    CREATED         STATUS          PORTS
example-foo-1   alpine    "/entrypoint.…"   foo        4 seconds ago   Up 2 seconds    0.0.0.0:8080->80/tcp
```

The `docker compose ps` command currently only supports the `--filter status=<status>` option, but additional filter options may be added in the future.

​	`docker compose ps` 命令当前仅支持 `--filter status=<status>` 选项，但未来可能会添加其他过滤选项。
