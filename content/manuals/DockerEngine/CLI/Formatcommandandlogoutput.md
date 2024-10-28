+++
title = "格式化命令和日志输出"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/cli/formatting/](https://docs.docker.com/engine/cli/formatting/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Format command and log output - 格式化命令和日志输出

Docker supports [Go templates](https://golang.org/pkg/text/template/) which you can use to manipulate the output format of certain commands and log drivers.

​	Docker 支持 [Go 模板](https://golang.org/pkg/text/template/)，可以用来调整特定命令和日志驱动程序的输出格式。

Docker provides a set of basic functions to manipulate template elements. All of these examples use the `docker inspect` command, but many other CLI commands have a `--format` flag, and many of the CLI command references include examples of customizing the output format.

​	Docker 提供了一些基本的函数来操作模板元素。所有这些示例都使用 `docker inspect` 命令，但许多其他 CLI 命令都带有 `--format` 标志，并且许多 CLI 命令参考文档中也包含了自定义输出格式的示例。

> **Note**
>
> 
>
> When using the `--format` flag, you need observe your shell environment. In a POSIX shell, you can run the following with a single quote:
>
> ​	使用 `--format` 标志时需要留意您的 shell 环境。在 POSIX shell 中，您可以使用单引号运行以下命令：
>
> 
>
> ```console
> $ docker inspect --format '{{join .Args " , "}}'
> ```
>
> Otherwise, in a Windows shell (for example, PowerShell), you need to use single quotes, but escape the double quotes inside the parameters as follows:
>
> ​	在 Windows shell（例如 PowerShell）中，您需要使用单引号并在参数内转义双引号：
>
> 
>
> ```console
> $ docker inspect --format '{{join .Args \" , \"}}'
> ```

## join

`join` concatenates a list of strings to create a single string. It puts a separator between each element in the list.

​	`join` 将字符串列表连接成一个字符串，在每个元素之间插入分隔符。



```console
$ docker inspect --format '{{join .Args " , "}}' container
```

## table

`table` specifies which fields you want to see its output.

​	`table` 指定要在输出中显示的字段。



```console
$ docker image list --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}\t{{.Size}}"
```

## json

`json` encodes an element as a json string.

​	`json` 将元素编码为 json 字符串。



```console
$ docker inspect --format '{{json .Mounts}}' container
```

## lower

`lower` transforms a string into its lowercase representation.

​	`lower` 将字符串转换为小写。



```console
$ docker inspect --format "{{lower .Name}}" container
```

## split

`split` slices a string into a list of strings separated by a separator.

​	`split` 根据分隔符将字符串分割成字符串列表。



```console
$ docker inspect --format '{{split .Image ":"}}' container
```

## title

`title` capitalizes the first character of a string.

​	`title` 将字符串的第一个字符大写。



```console
$ docker inspect --format "{{title .Name}}" container
```

## upper

`upper` transforms a string into its uppercase representation.

​	`upper` 将字符串转换为大写。



```console
$ docker inspect --format "{{upper .Name}}" container
```

## println

`println` prints each value on a new line.

​	`println` 将每个值打印在新行上。



```console
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{println .IPAddress}}{{end}}' container
```

## Hint

To find out what data can be printed, show all content asson:

​	要查看可以输出的数据，请将所有内容显示为 json：



```console
$ docker container ls --format='{{json .}}'
```
