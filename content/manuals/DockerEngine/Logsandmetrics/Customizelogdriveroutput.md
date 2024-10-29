+++
title = "自定义日志驱动输出"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/log_tags/](https://docs.docker.com/engine/logging/log_tags/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Customize log driver output - 自定义日志驱动输出

The `tag` log option specifies how to format a tag that identifies the container's log messages. By default, the system uses the first 12 characters of the container ID. To override this behavior, specify a `tag` option:

​	`tag` 日志选项用于指定如何格式化标识容器日志消息的标签。默认情况下，系统使用容器 ID 的前 12 个字符。要覆盖此行为，可以指定 `tag` 选项：



```console
$ docker run --log-driver=fluentd --log-opt fluentd-address=myhost.local:24224 --log-opt tag="mailer"
```

Docker supports some special template markup you can use when specifying a tag's value:

​	Docker 支持在指定标签的值时使用一些特殊的模板标记：

| Markup             | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| `{{.ID}}`          | 容器 ID 的前 12 个字符。The first 12 characters of the container ID. |
| `{{.FullID}}`      | 完整的容器 ID。The full container ID.                        |
| `{{.Name}}`        | 容器名称。The container name.                                |
| `{{.ImageID}}`     | 容器镜像 ID 的前 12 个字符。The first 12 characters of the container's image ID. |
| `{{.ImageFullID}}` | 容器镜像的完整 ID。The container's full image ID.            |
| `{{.ImageName}}`   | 容器使用的镜像名称。The name of the image used by the container. |
| `{{.DaemonName}}`  | Docker 程序的名称 (`docker`)。The name of the Docker program (`docker`). |

For example, specifying a `--log-opt tag="{{.ImageName}}/{{.Name}}/{{.ID}}"` value yields `syslog` log lines like:

​	例如，指定 `--log-opt tag="{{.ImageName}}/{{.Name}}/{{.ID}}"` 作为标签值，可以生成如下格式的 `syslog` 日志行：



```none
Aug  7 18:33:19 HOSTNAME hello-world/foobar/5790672ab6a0[9103]: Hello from Docker.
```

At startup time, the system sets the `container_name` field and `{{.Name}}` in the tags. If you use `docker rename` to rename a container, the new name isn't reflected in the log messages. Instead, these messages continue to use the original container name.

​	在启动时，系统设置 `container_name` 字段和标签中的 `{{.Name}}`。如果使用 `docker rename` 重命名容器，日志消息中不会反映新名称，而是继续使用原始容器名称。
