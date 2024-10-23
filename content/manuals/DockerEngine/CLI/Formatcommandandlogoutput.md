+++
title = "Format command and log output"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/engine/cli/formatting/](https://docs.docker.com/engine/cli/formatting/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Format command and log output

Docker supports [Go templates](https://golang.org/pkg/text/template/) which you can use to manipulate the output format of certain commands and log drivers.

Docker provides a set of basic functions to manipulate template elements. All of these examples use the `docker inspect` command, but many other CLI commands have a `--format` flag, and many of the CLI command references include examples of customizing the output format.

> **Note**
>
> 
>
> When using the `--format` flag, you need observe your shell environment. In a POSIX shell, you can run the following with a single quote:
>
> 
>
> ```console
> $ docker inspect --format '{{join .Args " , "}}'
> ```
>
> Otherwise, in a Windows shell (for example, PowerShell), you need to use single quotes, but escape the double quotes inside the parameters as follows:
>
> 
>
> ```console
> $ docker inspect --format '{{join .Args \" , \"}}'
> ```

## [join](https://docs.docker.com/engine/cli/formatting/#join)

`join` concatenates a list of strings to create a single string. It puts a separator between each element in the list.



```console
$ docker inspect --format '{{join .Args " , "}}' container
```

## [table](https://docs.docker.com/engine/cli/formatting/#table)

`table` specifies which fields you want to see its output.



```console
$ docker image list --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}\t{{.Size}}"
```

## [json](https://docs.docker.com/engine/cli/formatting/#json)

`json` encodes an element as a json string.



```console
$ docker inspect --format '{{json .Mounts}}' container
```

## [lower](https://docs.docker.com/engine/cli/formatting/#lower)

`lower` transforms a string into its lowercase representation.



```console
$ docker inspect --format "{{lower .Name}}" container
```

## [split](https://docs.docker.com/engine/cli/formatting/#split)

`split` slices a string into a list of strings separated by a separator.



```console
$ docker inspect --format '{{split .Image ":"}}' container
```

## [title](https://docs.docker.com/engine/cli/formatting/#title)

`title` capitalizes the first character of a string.



```console
$ docker inspect --format "{{title .Name}}" container
```

## [upper](https://docs.docker.com/engine/cli/formatting/#upper)

`upper` transforms a string into its uppercase representation.



```console
$ docker inspect --format "{{upper .Name}}" container
```

## [println](https://docs.docker.com/engine/cli/formatting/#println)

`println` prints each value on a new line.



```console
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{println .IPAddress}}{{end}}' container
```

## [Hint](https://docs.docker.com/engine/cli/formatting/#hint)

To find out what data can be printed, show all content as json:



```console
$ docker container ls --format='{{json .}}'
```
