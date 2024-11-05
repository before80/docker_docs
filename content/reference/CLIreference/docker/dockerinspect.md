+++
title = "docker inspect"
date = 2024-10-23T14:54:43+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/inspect/](https://docs.docker.com/reference/cli/docker/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker inspect

| Description | Return low-level information on Docker objects  |
| :---------- | ----------------------------------------------- |
| Usage       | `docker inspect [OPTIONS] NAME|ID [NAME|ID...]` |

## Description

Docker inspect provides detailed information on constructs controlled by Docker.

​	`docker inspect` 提供 Docker 控制的构造详细信息。

By default, `docker inspect` will render results in a JSON array.

​	默认情况下，`docker inspect` 会以 JSON 数组的形式呈现结果。

### Format the output (`--format`)

If a format is specified, the given template will be executed for each result.

​	如果指定了格式，将对每个结果执行给定的模板。

Go's [text/template](https://before80.github.io/go_docs/stdlib/text/template) package describes all the details of the format.

​	Go 的 [text/template](https://before80.github.io/go_docs/stdlib/text/template) 包描述了格式的所有详细信息。

### 指定目标类型 (`--type`) Specify target type (`--type`)

```
--type container|image|node|network|secret|service|volume|task|plugin
```

The `docker inspect` command matches any type of object by either ID or name. In some cases multiple type of objects (for example, a container and a volume) exist with the same name, making the result ambiguous.

​	`docker inspect` 命令通过 ID 或名称匹配任何类型的对象。在某些情况下，可能会存在多个类型的对象（例如，容器和卷）具有相同的名称，导致结果不明确。

To restrict `docker inspect` to a specific type of object, use the `--type` option.

​	要将 `docker inspect` 限制为特定类型的对象，可以使用 `--type` 选项。

The following example inspects a volume named `myvolume`.

​	以下示例检查名为 `myvolume` 的卷。



```console
$ docker inspect --type=volume myvolume
```

### 检查容器的大小 (`-s, --size`) Inspect the size of a container (`-s, --size`)

The `--size`, or short-form `-s`, option adds two additional fields to the `docker inspect` output. This option only works for containers. The container doesn't have to be running, it also works for stopped containers.

​	`--size` 或短格式 `-s` 选项为 `docker inspect` 输出添加了两个额外字段。此选项仅适用于容器。容器不必正在运行，也适用于已停止的容器。



```console
$ docker inspect --size mycontainer
```

The output includes the full output of a regular `docker inspect` command, with the following additional fields:

​	输出包括常规 `docker inspect` 命令的完整输出，并包含以下额外字段：

- `SizeRootFs`: the total size of all the files in the container, in bytes.
  - `SizeRootFs`：容器中所有文件的总大小，以字节为单位。

- `SizeRw`: the size of the files that have been created or changed in the container, compared to it's image, in bytes.
  - `SizeRw`：与镜像相比，容器中已创建或更改的文件大小，以字节为单位。



```console
$ docker run --name database -d redis
3b2cbf074c99db4a0cad35966a9e24d7bc277f5565c17233386589029b7db273
$ docker inspect --size database -f '{{ .SizeRootFs }}'
123125760
$ docker inspect --size database -f '{{ .SizeRw }}'
8192
$ docker exec database fallocate -l 1000 /newfile
$ docker inspect --size database -f '{{ .SizeRw }}'
12288
```

## Options

| Option         | Default | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| `-f, --format` |         | 使用自定义模板格式化输出：'json': 以 JSON 格式输出；'TEMPLATE': 使用给定的 Go 模板格式化输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-s, --size`   |         | 如果类型为容器，则显示总文件大小  Display total file sizes if the type is container |
| `--type`       |         | 为指定类型返回 JSON  Return JSON for specified type          |

## Examples

### 获取实例的 IP 地址 Get an instance's IP address

For the most part, you can pick out any field from the JSON in a fairly straightforward manner.

​	通常，您可以以相对简单的方式从 JSON 中提取任何字段。



```console
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $INSTANCE_ID
```

### 获取实例的 MAC 地址 Get an instance's MAC address



```console
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.MacAddress}}{{end}}' $INSTANCE_ID
```

### 获取实例的日志路径 Get an instance's log path



```console
$ docker inspect --format='{{.LogPath}}' $INSTANCE_ID
```

### 获取实例的镜像名称 Get an instance's image name



```console
$ docker inspect --format='{{.Config.Image}}' $INSTANCE_ID
```

### 列出所有端口绑定 List all port bindings

You can loop over arrays and maps in the results to produce simple text output:

​	您可以在结果中循环数组和映射以生成简单的文本输出：



```console
$ docker inspect --format='{{range $p, $conf := .NetworkSettings.Ports}} {{$p}} -> {{(index $conf 0).HostPort}} {{end}}' $INSTANCE_ID
```

### 查找特定的端口映射 Find a specific port mapping

The `.Field` syntax doesn't work when the field name begins with a number, but the template language's `index` function does. The `.NetworkSettings.Ports` section contains a map of the internal port mappings to a list of external address/port objects. To grab just the numeric public port, you use `index` to find the specific port map, and then `index` 0 contains the first object inside of that. Then, specify the `HostPort` field to get the public address.

​	当字段名称以数字开头时，`.Field` 语法不起作用，但可以使用模板语言的 `index` 函数。在 `.NetworkSettings.Ports` 部分中，包含内部端口映射到外部地址/端口对象列表的映射。要仅获取数字的公共端口，可以使用 `index` 查找特定端口映射，然后 `index` 0 包含该字段的第一个对象。然后，指定 `HostPort` 字段以获取公共地址。



```console
$ docker inspect --format='{{(index (index .NetworkSettings.Ports "8787/tcp") 0).HostPort}}' $INSTANCE_ID
```

### 以 JSON 格式获取子部分 Get a subsection in JSON format

If you request a field which is itself a structure containing other fields, by default you get a Go-style dump of the inner values. Docker adds a template function, `json`, which can be applied to get results in JSON format.

​	如果请求的字段本身包含其他字段的结构，默认情况下您会获得内部值的 Go 样式转储。Docker 添加了一个模板函数 `json`，可以应用它来获取 JSON 格式的结果。



```console
$ docker inspect --format='{{json .Config}}' $INSTANCE_ID
```
