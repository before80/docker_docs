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

## Examples

### Print the current context

The following example prints the currently used [`docker context`]({{< ref "/reference/CLIreference/docker/dockercontext" >}}):



```console
$ docker context show'
default
```

As an example, this output can be used to dynamically change your shell prompt to indicate your active context. The example below illustrates how this output could be used when using Bash as your shell.

Declare a function to obtain the current context in your `~/.bashrc`, and set this command as your `PROMPT_COMMAND`



```console
function docker_context_prompt() {
        PS1="context: $(docker context show)> "
}

PROMPT_COMMAND=docker_context_prompt
```

After reloading the `~/.bashrc`, the prompt now shows the currently selected `docker context`:



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
