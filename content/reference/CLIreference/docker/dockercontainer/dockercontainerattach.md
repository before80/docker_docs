+++
title = "docker container attach"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/attach/](https://docs.docker.com/reference/cli/docker/container/attach/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container attach

| Description | Attach local standard input, output, and error streams to a running container |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container attach [OPTIONS] CONTAINER`                |
| Aliases     | `docker attach`                                              |

## Description

Use `docker attach` to attach your terminal's standard input, output, and error (or any combination of the three) to a running container using the container's ID or name. This lets you view its output or control it interactively, as though the commands were running directly in your terminal.

​	使用 `docker attach` 将终端的标准输入、输出和错误（或这三者的任意组合）附加到正在运行的容器，使用容器的 ID 或名称。这让你能够查看其输出或交互式地控制它，就像命令直接在你的终端中运行一样。

> **Note**
>
> The `attach` command displays the output of the container's `ENTRYPOINT` and `CMD` process. This can appear as if the attach command is hung when in fact the process may simply not be writing any output at that time.
>
> ​	`attach` 命令显示容器的 `ENTRYPOINT` 和 `CMD` 进程的输出。这可能会导致 `attach` 命令看起来似乎卡住，但实际上该进程可能只是此时没有写入任何输出。

You can attach to the same contained process multiple times simultaneously, from different sessions on the Docker host.

​	你可以同时从 Docker 主机的不同会话附加到同一个容器进程。

To stop a container, use `CTRL-c`. This key sequence sends `SIGKILL` to the container. If `--sig-proxy` is true (the default),`CTRL-c` sends a `SIGINT` to the container. If the container was run with `-i` and `-t`, you can detach from a container and leave it running using the `CTRL-p CTRL-q` key sequence.

​	要停止容器，请使用 `CTRL-c`。此键序列向容器发送 `SIGKILL`。如果 `--sig-proxy` 为 true（默认值），则 `CTRL-c` 向容器发送 `SIGINT`。如果容器是以 `-i` 和 `-t` 启动的，你可以使用 `CTRL-p CTRL-q` 键序列从容器中分离并保持其运行。

> **Note**
>
> A process running as PID 1 inside a container is treated specially by Linux: it ignores any signal with the default action. So, the process doesn't terminate on `SIGINT` or `SIGTERM` unless it's coded to do so.
>
> ​	在容器内以 PID 1 运行的进程被 Linux 特殊对待：它忽略任何信号的默认操作。因此，除非该进程被编码为这样，否则在收到 `SIGINT` 或 `SIGTERM` 时不会终止。

You can't redirect the standard input of a `docker attach` command while attaching to a TTY-enabled container (using the `-i` and `-t` options).

​	在附加到启用 TTY 的容器时（使用 `-i` 和 `-t` 选项），你无法重定向 `docker attach` 命令的标准输入。

While a client is connected to container's `stdio` using `docker attach`, Docker uses a ~1MB memory buffer to maximize the throughput of the application. Once this buffer is full, the speed of the API connection is affected, and so this impacts the output process' writing speed. This is similar to other applications like SSH. Because of this, it isn't recommended to run performance-critical applications that generate a lot of output in the foreground over a slow client connection. Instead, use the `docker logs` command to get access to the logs.

​	当客户端使用 `docker attach` 连接到容器的 `stdio` 时，Docker 使用约 1MB 的内存缓冲区来最大化应用程序的吞吐量。一旦该缓冲区满了，API 连接的速度就会受到影响，从而影响输出进程的写入速度。这类似于其他应用程序，如 SSH。因此，不建议在较慢的客户端连接上运行生成大量输出的性能关键型应用程序。相反，使用 `docker logs` 命令获取日志。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--detach-keys`](https://docs.docker.com/reference/cli/docker/container/attach/#detach-keys) |         | 覆盖分离容器的键序列 Override the key sequence for detaching a container |
| `--no-stdin`                                                 |         | 不附加 STDIN - Do not attach STDIN                           |
| `--sig-proxy`                                                | `true`  | 将所有接收到的信号代理到进程 Proxy all received signals to the process |

## Examples

### 附加到并从运行中的容器中分离 Attach to and detach from a running container

The following example starts an Alpine container running `top` in detached mode, then attaches to the container;

​	以下示例启动一个在分离模式下运行 `top` 的 Alpine 容器，然后附加到该容器；



```console
$ docker run -d --name topdemo alpine top -b

$ docker attach topdemo

Mem: 2395856K used, 5638884K free, 2328K shrd, 61904K buff, 1524264K cached
CPU:   0% usr   0% sys   0% nic  99% idle   0% io   0% irq   0% sirq
Load average: 0.15 0.06 0.01 1/567 6
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     R     1700   0%   3   0% top -b
```

As the container was started without the `-i`, and `-t` options, signals are forwarded to the attached process, which means that the default `CTRL-p CTRL-q` detach key sequence produces no effect, but pressing `CTRL-c` terminates the container:

​	由于容器是以没有 `-i` 和 `-t` 选项的方式启动的，信号被转发到附加的进程，这意味着默认的 `CTRL-p CTRL-q` 分离键序列无效，但按下 `CTRL-c` 会终止容器：



```console
<...>
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     R     1700   0%   7   0% top -b
^P^Q
^C

$ docker ps -a --filter name=topdemo

CONTAINER ID   IMAGE     COMMAND    CREATED          STATUS                       PORTS     NAMES
96254a235bd6   alpine    "top -b"   44 seconds ago   Exited (130) 8 seconds ago             topdemo
```

Repeating the example above, but this time with the `-i` and `-t` options set;

​	重复上述示例，但这次设置 `-i` 和 `-t` 选项；



```console
$ docker run -dit --name topdemo2 alpine /usr/bin/top -b
```

Now, when attaching to the container, and pressing the `CTRL-p CTRL-q` ("read escape sequence"), the Docker CLI is handling the detach sequence, and the `attach` command is detached from the container. Checking the container's status with `docker ps` shows that the container is still running in the background:

​	现在，当附加到容器并按下 `CTRL-p CTRL-q`（“读取转义序列”）时，Docker CLI 正在处理分离序列，并且 `attach` 命令从容器中分离。使用 `docker ps` 检查容器的状态显示该容器仍在后台运行：

```console
$ docker attach topdemo2

Mem: 2405344K used, 5629396K free, 2512K shrd, 65100K buff, 1524952K cached
CPU:   0% usr   0% sys   0% nic  99% idle   0% io   0% irq   0% sirq
Load average: 0.12 0.12 0.05 1/594 6
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     R     1700   0%   3   0% top -b
read escape sequence

$ docker ps -a --filter name=topdemo2

CONTAINER ID   IMAGE     COMMAND    CREATED          STATUS          PORTS     NAMES
fde88b83c2c2   alpine    "top -b"   22 seconds ago   Up 21 seconds             topdemo2
```

### 获取容器命令的退出代码 Get the exit code of the container's command

And in this second example, you can see the exit code returned by the `bash` process is returned by the `docker attach` command to its caller too:

​	在这个第二个示例中，你可以看到 `bash` 进程返回的退出代码也通过 `docker attach` 命令返回给调用者：



```console
$ docker run --name test -dit alpine
275c44472aebd77c926d4527885bb09f2f6db21d878c75f0a1c212c03d3bcfab

$ docker attach test
/# exit 13

$ echo $?
13

$ docker ps -a --filter name=test

CONTAINER ID   IMAGE     COMMAND     CREATED              STATUS                       PORTS     NAMES
a2fe3fd886db   alpine    "/bin/sh"   About a minute ago   Exited (13) 40 seconds ago             test
```

### 覆盖分离序列 (`--detach-keys`) Override the detach sequence (`--detach-keys`)

Use the `--detach-keys` option to override the Docker key sequence for detach. This is useful if the Docker default sequence conflicts with key sequence you use for other applications. There are two ways to define your own detach key sequence, as a per-container override or as a configuration property on your entire configuration.

​	使用 `--detach-keys` 选项来覆盖 Docker 的分离键序列。如果 Docker 的默认序列与你在其他应用中使用的键序列冲突，这非常有用。可以通过每个容器的覆盖或作为整个配置的配置属性来定义自己的分离键序列。

To override the sequence for an individual container, use the `--detach-keys="<sequence>"` flag with the `docker attach` command. The format of the `<sequence>` is either a letter [a-Z], or the `ctrl-` combined with any of the following:

​	要覆盖单个容器的序列，请在 `docker attach` 命令中使用 `--detach-keys="<sequence>"` 标志。`<sequence>` 的格式可以是字母 [a-Z]，或 `ctrl-` 结合以下任意一个：

- `a-z` (a single lowercase alpha character )
  - `a-z`（一个小写字母）

- `@` (at sign)
  - `@`（at 符号）

- `[` (left bracket)
  - `[`（左方括号）

- `\\` (two backward slashes)
  - `\\`（两个反斜杠）

- `_` (underscore)
  - `_`（下划线）

- `^` (caret)
  - `^`（插入符）


These `a`, `ctrl-a`, `X`, or `ctrl-\\` values are all examples of valid key sequences. To configure a different configuration default key sequence for all containers, see [**Configuration file** section](https://docs.docker.com/reference/cli/docker/#configuration-files).

​	这些 `a`、`ctrl-a`、`X` 或 `ctrl-\\` 的值都是有效的键序列示例。要为所有容器配置不同的配置默认键序列，请参见 [**配置文件**部分](https://docs.docker.com/reference/cli/docker/#configuration-files)。
