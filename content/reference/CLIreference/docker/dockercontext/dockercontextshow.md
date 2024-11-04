+++
title = "docker context show"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/context/show/](https://docs.docker.com/reference/cli/docker/context/show/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker context show

| Description | Print the name of the current context |
| :---------- | ------------------------------------- |
| Usage       | `docker context show`                 |

## Description

Print the name of the current context, possibly set by `DOCKER_CONTEXT` environment variable or `--context` global option.

​	打印当前上下文的名称，可能由 `DOCKER_CONTEXT` 环境变量或 `--context` 全局选项设置。

## Examples

### 打印当前上下文 Print the current context

The following example prints the currently used [`docker context`]({{< ref "/reference/CLIreference/docker/dockercontext" >}}):

​	以下示例打印当前使用的 `docker context`：

```console
$ docker context show'
default
```

As an example, this output can be used to dynamically change your shell prompt to indicate your active context. The example below illustrates how this output could be used when using Bash as your shell.

​	例如，可以使用该输出动态更改 shell 提示以指示活动上下文。下面的示例展示了在使用 Bash 作为 shell 时如何实现。

Declare a function to obtain the current context in your `~/.bashrc`, and set this command as your `PROMPT_COMMAND`

​	在你的 `~/.bashrc` 中声明一个获取当前上下文的函数，并将该命令设置为 `PROMPT_COMMAND`：

```console
function docker_context_prompt() {
        PS1="context: $(docker context show)> "
}

PROMPT_COMMAND=docker_context_prompt
```

After reloading the `~/.bashrc`, the prompt now shows the currently selected `docker context`:

​	重新加载 `~/.bashrc` 后，提示符现在会显示当前选择的 `docker context`。

```console
$ source ~/.bashrc
context: default> docker context create --docker host=unix:///var/run/docker.sock my-context
my-context
Successfully created context "my-context"
context: default> docker context use my-context
my-context
Current context is now "my-context"
context: my-context> docker context use default
default
Current context is now "default"
context: default>
```
