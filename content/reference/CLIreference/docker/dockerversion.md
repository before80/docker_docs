+++
title = "docker version"
date = 2024-10-23T14:54:43+08:00
weight = 260
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/version/](https://docs.docker.com/reference/cli/docker/version/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker version

| Description | Show the Docker version information |
| :---------- | ----------------------------------- |
| Usage       | `docker version [OPTIONS]`          |

## Description

The version command prints the current version number for all independently versioned Docker components. Use the [`--format`](https://docs.docker.com/reference/cli/docker/version/#format) option to customize the output.

​	version 命令打印所有独立版本控制的 Docker 组件的当前版本号。使用 [`--format`](https://docs.docker.com/reference/cli/docker/version/#format) 选项可自定义输出。

The version command (`docker version`) outputs the version numbers of Docker components, while the `--version` flag (`docker --version`) outputs the version number of the Docker CLI you are using.

​	version 命令（`docker version`）输出 Docker 组件的版本号，而 `--version` 标志（`docker --version`）输出所使用的 Docker CLI 的版本号。

### Default output

The default output renders all version information divided into two sections; the `Client` section contains information about the Docker CLI and client components, and the `Server` section contains information about the Docker Engine and components used by the Docker Engine, such as the containerd and runc OCI Runtimes.

​	默认输出会分为两个部分显示所有版本信息；`Client` 部分包含有关 Docker CLI 和客户端组件的信息，`Server` 部分包含有关 Docker 引擎和 Docker 引擎使用的组件（如 containerd 和 runc OCI 运行时）的信息。

The information shown may differ depending on how you installed Docker and what components are in use. The following example shows the output on a macOS machine running Docker Desktop:

​	显示的信息可能会因 Docker 的安装方式和正在使用的组件而异。以下示例显示在运行 Docker Desktop 的 macOS 机器上的输出：



```console
$ docker version

Client: Docker Engine - Community
 Version:           23.0.3
 API version:       1.42
 Go version:        go1.19.7
 Git commit:        3e7cbfd
 Built:             Tue Apr  4 22:05:41 2023
 OS/Arch:           darwin/amd64
 Context:           default

Server: Docker Desktop 4.19.0 (12345)
 Engine:
  Version:          23.0.3
  API version:      1.42 (minimum version 1.12)
  Go version:       go1.19.7
  Git commit:       59118bf
  Built:            Tue Apr  4 22:05:41 2023
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.20
  GitCommit:        2806fc1057397dbaeefbea0e4e17bddfbd388f38
 runc:
  Version:          1.1.5
  GitCommit:        v1.1.5-0-gf19387a
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

### 客户端和服务器版本 Client and server versions

Docker uses a client/server architecture, which allows you to use the Docker CLI on your local machine to control a Docker Engine running on a remote machine, which can be (for example) a machine running in the cloud or inside a virtual machine.

​	Docker 使用客户端/服务器架构，这允许您在本地机器上使用 Docker CLI 控制在远程机器上运行的 Docker 引擎，该远程机器可以是在云中或虚拟机中的机器。

The following example switches the Docker CLI to use a [context]({{< ref "/reference/CLIreference/docker/dockercontext" >}}) named `remote-test-server`, which runs an older version of the Docker Engine on a Linux server:

​	以下示例将 Docker CLI 切换到名为 `remote-test-server` 的 [上下文]({{< ref "/reference/CLIreference/docker/dockercontext" >}})，该上下文在 Linux 服务器上运行较旧版本的 Docker 引擎：



```console
$ docker context use remote-test-server
remote-test-server

$ docker version

Client: Docker Engine - Community
 Version:           23.0.3
 API version:       1.40 (downgraded from 1.42)
 Go version:        go1.19.7
 Git commit:        3e7cbfd
 Built:             Tue Apr  4 22:05:41 2023
 OS/Arch:           darwin/amd64
 Context:           remote-test-server

Server: Docker Engine - Community
 Engine:
  Version:          19.03.8
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       afacb8b
  Built:            Wed Mar 11 01:29:16 2020
  OS/Arch:          linux/amd64
 containerd:
  Version:          v1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

### API 版本和版本协商 API version and version negotiation

The API version used by the client depends on the Docker Engine that the Docker CLI is connecting with. When connecting with the Docker Engine, the Docker CLI and Docker Engine perform API version negotiation, and select the highest API version that is supported by both the Docker CLI and the Docker Engine.

​	客户端使用的 API 版本取决于 Docker CLI 连接的 Docker 引擎。当连接 Docker 引擎时，Docker CLI 和 Docker 引擎会进行 API 版本协商，并选择双方支持的最高 API 版本。

For example, if the CLI is connecting with Docker Engine version 19.03, it downgrades to API version 1.40 (refer to the [API version matrix](https://docs.docker.com/reference/api/engine/#api-version-matrix) to learn about the supported API versions for Docker Engine):

​	例如，如果 CLI 正在连接 Docker 引擎版本 19.03，它会降级到 API 版本 1.40（请参考 [API 版本矩阵](https://docs.docker.com/reference/api/engine/#api-version-matrix) 了解 Docker 引擎支持的 API 版本）：



```console
$ docker version --format '{{.Client.APIVersion}}'

1.40
```

Be aware that API version can also be overridden using the `DOCKER_API_VERSION` environment variable, which can be useful for debugging purposes, and disables API version negotiation. The following example illustrates an environment where the `DOCKER_API_VERSION` environment variable is set. Unsetting the environment variable removes the override, and re-enables API version negotiation:

​	请注意，API 版本也可以使用 `DOCKER_API_VERSION` 环境变量覆盖，这在调试时可能有用，并且会禁用 API 版本协商。以下示例展示了设置 `DOCKER_API_VERSION` 环境变量的环境。取消设置该环境变量会移除覆盖并重新启用 API 版本协商：

```console
$ env | grep DOCKER_API_VERSION
DOCKER_API_VERSION=1.39

$ docker version --format '{{.Client.APIVersion}}'
1.39

$ unset DOCKER_API_VERSION
$ docker version --format '{{.Client.APIVersion}}'
1.42
```

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --format`](https://docs.docker.com/reference/cli/docker/version/#format) |         | 使用自定义模板格式化输出：'json'：以 JSON 格式打印输出 'TEMPLATE'：使用指定的 Go 模板格式化输出。有关使用模板格式化输出的更多信息，请参考 https://docs.docker.com/go/formatting/  Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |

## Examples

### Format the output (`--format`)

The formatting option (`--format`) pretty-prints the output using a Go template, which allows you to customize the output format, or to obtain specific information from the output. Refer to the [format command and log output](https://docs.docker.com/config/formatting/) page for details of the format.

​	格式化选项（`--format`）使用 Go 模板美化输出，这使您可以自定义输出格式或从输出中获取特定信息。详细内容请参考 [格式命令和日志输出](https://docs.docker.com/config/formatting/) 页面。

### 获取服务器版本 Get the server version



```console
$ docker version --format '{{.Server.Version}}'

23.0.3
```

### 获取客户端 API 版本 Get the client API version

The following example prints the API version that is used by the client:

​	以下示例打印客户端使用的 API 版本：

```console
$ docker version --format '{{.Client.APIVersion}}'

1.42
```

The version shown is the API version that is negotiated between the client and the Docker Engine. Refer to [API version and version negotiation](https://docs.docker.com/reference/cli/docker/version/#api-version-and-version-negotiation) above for more information.

​	显示的版本是客户端和 Docker 引擎之间协商的 API 版本。更多信息请参考上面的 [API 版本和版本协商](https://docs.docker.com/reference/cli/docker/version/#api-version-and-version-negotiation)。

### 转储原始 JSON 数据 Dump raw JSON data



```console
$ docker version --format '{{json .}}'

{"Client":"Version":"23.0.3","ApiVersion":"1.42", ...}
```
