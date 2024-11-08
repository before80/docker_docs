+++
title = "docker"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/](https://docs.docker.com/reference/cli/docker/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker

| Description | The base command for the Docker CLI. |
| :---------- | ------------------------------------ |
|             |                                      |

## Description

Depending on your Docker system configuration, you may be required to preface each `docker` command with `sudo`. To avoid having to use `sudo` with the `docker` command, your system administrator can create a Unix group called `docker` and add users to it.

​	根据 Docker 系统配置，您可能需要在每个 `docker` 命令前加上 `sudo`。为了避免在 `docker` 命令前使用 `sudo`，系统管理员可以创建一个名为 `docker` 的 Unix 用户组并将用户添加到该组。

For more information about installing Docker or `sudo` configuration, refer to the [installation](https://docs.docker.com/install/) instructions for your operating system.

​	有关安装 Docker 或 `sudo` 配置的更多信息，请参阅您操作系统的 [安装](https://docs.docker.com/install/) 说明。

### Display help text

To list the help on any command just execute the command, followed by the `--help` option.

​	要列出任何命令的帮助内容，只需在命令后添加 `--help` 选项。

```console
$ docker run --help

Usage: docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Create and run a new container from an image

Options:
      --add-host value             Add a custom host-to-IP mapping (host:ip) (default [])
  -a, --attach value               Attach to STDIN, STDOUT or STDERR (default [])
<...>
```

### Environment variables

The following list of environment variables are supported by the `docker` command line:

​	`docker` 命令行支持以下环境变量：

| Variable                      | Description                                                  |
| :---------------------------- | :----------------------------------------------------------- |
| `DOCKER_API_VERSION`          | 覆盖用于调试的协商 API 版本（例如 `1.19`） Override the negotiated API version to use for debugging (e.g. `1.19`) |
| `DOCKER_CERT_PATH`            | 身份验证密钥的位置。此变量同时用于 `docker` CLI 和 [`dockerd` 守护进程]({{< ref "/reference/CLIreference/dockerd" >}})  Location of your authentication keys. This variable is used both by the `docker` CLI and the [`dockerd` daemon]({{< ref "/reference/CLIreference/dockerd" >}}) |
| `DOCKER_CONFIG`               | 客户端配置文件的位置。 The location of your client configuration files. |
| `DOCKER_CONTENT_TRUST_SERVER` | 使用的 Notary 服务器的 URL，默认为与注册表相同的 URL。 The URL of the Notary server to use. Defaults to the same URL as the registry. |
| `DOCKER_CONTENT_TRUST`        | 设置后，Docker 使用 Notary 签名和验证镜像。相当于对 `build`、`create`、`pull`、`push`、`run` 启用 `--disable-content-trust=false`。  When set Docker uses notary to sign and verify images. Equates to `--disable-content-trust=false` for build, create, pull, push, run. |
| `DOCKER_CONTEXT`              | 要使用的 `docker context` 名称（覆盖 `DOCKER_HOST` 环境变量和通过 `docker context use` 设置的默认上下文）  Name of the `docker context` to use (overrides `DOCKER_HOST` env var and default context set with `docker context use`) |
| `DOCKER_CUSTOM_HEADERS`       | （实验性）配置客户端发送的[自定义 HTTP 标头](https://docs.docker.com/reference/cli/docker/#custom-http-headers)。必须以 `name=value` 的逗号分隔列表提供。    - (Experimental) Configure [custom HTTP headers](https://docs.docker.com/reference/cli/docker/#custom-http-headers) to be sent by the client. Headers must be provided as a comma-separated list of `name=value` pairs. This is the equivalent to the `HttpHeaders` field in the configuration file. |
| `DOCKER_DEFAULT_PLATFORM`     | 接受 `--platform` 标志的命令的默认平台。 Default platform for commands that take the `--platform` flag. |
| `DOCKER_HIDE_LEGACY_COMMANDS` | 设置后，Docker 在 `docker help` 输出中隐藏“传统”顶级命令（如 `docker rm` 和 `docker pull`），并仅显示按对象类型分类的“管理命令”（例如，`docker container`）。  When set, Docker hides "legacy" top-level commands (such as `docker rm`, and `docker pull`) in `docker help` output, and only `Management commands` per object-type (e.g., `docker container`) are printed. This may become the default in a future release. |
| `DOCKER_HOST`                 | 要连接的守护进程套接字。 Daemon socket to connect to.        |
| `DOCKER_TLS`                  | 启用 `docker` CLI 的 TLS 连接（等效于 `--tls` 命令行选项）。设置为非空值启用 TLS。如果设置了其他 TLS 选项，则自动启用 TLS。 Enable TLS for connections made by the `docker` CLI (equivalent of the `--tls` command-line option). Set to a non-empty value to enable TLS. Note that TLS is enabled automatically if any of the other TLS options are set. |
| `DOCKER_TLS_VERIFY`           | 设置后，Docker 使用 TLS 并验证远程。此变量同时用于 `docker` CLI 和 [`dockerd` 守护进程]({{< ref "/reference/CLIreference/dockerd" >}})。 When set Docker uses TLS and verifies the remote. This variable is used both by the `docker` CLI and the [`dockerd` daemon]({{< ref "/reference/CLIreference/dockerd" >}}) |
| `BUILDKIT_PROGRESS`           | 设置 [BuildKit 后端]({{< ref "/manuals/DockerBuild/BuildKit" >}}) 构建时的进度输出类型 (`auto`、`plain`、`tty`、`rawjson`)。使用 `plain` 显示容器输出（默认 `auto`）。 Set type of progress output (`auto`, `plain`, `tty`, `rawjson`) when [building](https://docs.docker.com/reference/cli/docker/image/build/) with [BuildKit backend]({{< ref "/manuals/DockerBuild/BuildKit" >}}). Use plain to show container output (default `auto`). |

Because Docker is developed using Go, you can also use any environment variables used by the Go runtime. In particular, you may find these useful:

​	由于 Docker 使用 Go 开发，您还可以使用 Go 运行时使用的任何环境变量。特别有用的包括：

| Variable      | Description                                                  |
| :------------ | :----------------------------------------------------------- |
| `HTTP_PROXY`  | HTTP 请求的代理 URL，除非被 NoProxy 覆盖。 Proxy URL for HTTP requests unless overridden by NoProxy. |
| `HTTPS_PROXY` | HTTPS 请求的代理 URL，除非被 NoProxy 覆盖。 Proxy URL for HTTPS requests unless overridden by NoProxy. |
| `NO_PROXY`    | 以逗号分隔的值，指定应排除在代理之外的主机。 Comma-separated values specifying hosts that should be excluded from proxying. |

See the [Go specification](https://pkg.go.dev/golang.org/x/net/http/httpproxy#Config) for details on these variables.

​	有关这些变量的详细信息，请参见 [Go 规范](https://pkg.go.dev/golang.org/x/net/http/httpproxy#Config)。

### Option types

Single character command line options can be combined, so rather than typing `docker run -i -t --name test busybox sh`, you can write `docker run -it --name test busybox sh`.

​	单字符命令行选项可以组合使用，因此可以将 `docker run -i -t --name test busybox sh` 写为 `docker run -it --name test busybox sh`。

#### Boolean

Boolean options take the form `-d=false`. The value you see in the help text is the default value which is set if you do **not** specify that flag. If you specify a Boolean flag without a value, this will set the flag to `true`, irrespective of the default value.

​	布尔选项以 `-d=false` 形式出现。帮助文本中显示的值是默认值，如果不指定该标志，将设置该默认值。如果指定布尔标志而未赋值，则会将其设置为 `true`，不论默认值是什么。

For example, running `docker run -d` will set the value to `true`, so your container **will** run in "detached" mode, in the background.

​	例如，运行 `docker run -d` 将设置值为 `true`，因此您的容器将**以“分离”模式**在后台运行。

Options which default to `true` (e.g., `docker build --rm=true`) can only be set to the non-default value by explicitly setting them to `false`:

​	默认值为 `true` 的选项（例如 `docker build --rm=true`）只能通过明确设置为 `false` 来设置为非默认值：

```console
$ docker build --rm=false .
```

#### Multi

You can specify options like `-a=[]` multiple times in a single command line, for example in these commands:

​	您可以在单行命令中多次指定类似 `-a=[]` 的选项，例如：

```console
$ docker run -a stdin -a stdout -i -t ubuntu /bin/bash

$ docker run -a stdin -a stdout -a stderr ubuntu /bin/ls
```

Sometimes, multiple options can call for a more complex value string as for `-v`:

​	在某些情况下，多个选项可能需要更复杂的值格式，例如 `-v`：

```console
$ docker run -v /host:/container example/mysql
```

> **Note**
>
> Do not use the `-t` and `-a stderr` options together due to limitations in the `pty` implementation. All `stderr` in `pty` mode simply goes to `stdout`.
>
> ​	由于 `pty` 实现的限制，请不要同时使用 `-t` 和 `-a stderr` 选项。`pty` 模式下的所有 `stderr` 将直接输出到 `stdout`。

#### Strings and Integers

Options like `--name=""` expect a string, and they can only be specified once. Options like `-c=0` expect an integer, and they can only be specified once.

​	类似 `--name=""` 的选项需要字符串，并且只能指定一次。类似 `-c=0` 的选项需要整数，并且只能指定一次。

### Configuration files

By default, the Docker command line stores its configuration files in a directory called `.docker` within your `$HOME` directory.

​	默认情况下，Docker 命令行将其配置文件存储在 `$HOME` 目录中的 `.docker` 文件夹内。

Docker manages most of the files in the configuration directory and you shouldn't modify them. However, you can modify the `config.json` file to control certain aspects of how the `docker` command behaves.

​	Docker 管理配置目录中的大部分文件，因此不应修改它们。然而，可以修改 `config.json` 文件以控制 `docker` 命令的某些行为。

You can modify the `docker` command behavior using environment variables or command-line options. You can also use options within `config.json` to modify some of the same behavior. If an environment variable and the `--config` flag are set, the flag takes precedent over the environment variable. Command line options override environment variables and environment variables override properties you specify in a `config.json` file.

​	您可以使用环境变量或命令行选项来修改 `docker` 命令的行为。也可以在 `config.json` 文件中使用选项来修改某些行为。如果设置了环境变量和 `--config` 标志，则标志优先于环境变量。命令行选项优先于环境变量，环境变量优先于 `config.json` 文件中指定的属性。

#### Change the `.docker` directory

To specify a different directory, use the `DOCKER_CONFIG` environment variable or the `--config` command line option. If both are specified, then the `--config` option overrides the `DOCKER_CONFIG` environment variable. The example below overrides the `docker ps` command using a `config.json` file located in the `~/testconfigs/` directory.

​	要指定其他目录，请使用 `DOCKER_CONFIG` 环境变量或 `--config` 命令行选项。如果两者都指定，则 `--config` 选项会覆盖 `DOCKER_CONFIG` 环境变量。以下示例使用位于 `~/testconfigs/` 目录中的 `config.json` 文件覆盖 `docker ps` 命令：

```console
$ docker --config ~/testconfigs/ ps
```

This flag only applies to whatever command is being ran. For persistent configuration, you can set the `DOCKER_CONFIG` environment variable in your shell (e.g. `~/.profile` or `~/.bashrc`). The example below sets the new directory to be `HOME/newdir/.docker`.

​	此标志仅适用于当前运行的命令。对于持久性配置，可以在 shell 中设置 `DOCKER_CONFIG` 环境变量（例如 `~/.profile` 或 `~/.bashrc`）。以下示例将新目录设置为 `HOME/newdir/.docker`：

```console
$ echo export DOCKER_CONFIG=$HOME/newdir/.docker > ~/.profile
```

### Docker CLI configuration file (`config.json`) properties



Use the Docker CLI configuration to customize settings for the `docker` CLI. The configuration file uses JSON formatting, and properties:

​	使用 Docker CLI 配置自定义 `docker` CLI 的设置。该配置文件使用 JSON 格式，包含以下属性：

By default, configuration file is stored in `~/.docker/config.json`. Refer to the [change the `.docker` directory](https://docs.docker.com/reference/cli/docker/#change-the-docker-directory) section to use a different location.

​	默认情况下，配置文件存储在 `~/.docker/config.json`。如需使用其他位置，请参阅 [更改 `.docker` 目录](https://docs.docker.com/reference/cli/docker/#change-the-docker-directory) 部分。

> **Warning**
>
> The configuration file and other files inside the `~/.docker` configuration directory may contain sensitive information, such as authentication information for proxies or, depending on your credential store, credentials for your image registries. Review your configuration file's content before sharing with others, and prevent committing the file to version control.
>
> ​	`~/.docker` 配置目录中的配置文件和其他文件可能包含敏感信息，例如代理的身份验证信息，或根据您的凭证存储，可能包含镜像注册表的凭据。在与他人共享之前，请检查配置文件的内容，并防止将文件提交到版本控制。

#### 自定义命令的默认输出格式 Customize the default output format for commands

These fields lets you customize the default output format for some commands if no `--format` flag is provided.

​	这些字段允许您在没有 `--format` 标志时自定义某些命令的默认输出格式。

| Property               | Description                                                  |
| :--------------------- | :----------------------------------------------------------- |
| `configFormat`         | `docker config ls` 输出的自定义默认格式。有关支持的格式说明符列表，请参阅 [`docker config ls`](https://docs.docker.com/reference/cli/docker/config/ls/#format)。 Custom default format for `docker config ls` output. See [`docker config ls`](https://docs.docker.com/reference/cli/docker/config/ls/#format) for a list of supported formatting directives. |
| `imagesFormat`         | `docker images` 或 `docker image ls` 输出的自定义默认格式。请参阅 [`docker images`](https://docs.docker.com/reference/cli/docker/image/ls/#format)。 Custom default format for `docker images` / `docker image ls` output. See [`docker images`](https://docs.docker.com/reference/cli/docker/image/ls/#format) for a list of supported formatting directives. |
| `networksFormat`       | `docker network ls` 输出的自定义默认格式。请参阅 [`docker network ls`](https://docs.docker.com/reference/cli/docker/network/ls/#format)。 Custom default format for `docker network ls` output. See [`docker network ls`](https://docs.docker.com/reference/cli/docker/network/ls/#format) for a list of supported formatting directives. |
| `nodesFormat`          | `docker node ls` 输出的自定义默认格式。请参阅 [`docker node ls`](https://docs.docker.com/reference/cli/docker/node/ls/#format)。 Custom default format for `docker node ls` output. See [`docker node ls`](https://docs.docker.com/reference/cli/docker/node/ls/#format) for a list of supported formatting directives. |
| `pluginsFormat`        | `docker plugin ls` 输出的自定义默认格式。请参阅 [`docker plugin ls`](https://docs.docker.com/reference/cli/docker/plugin/ls/#format)。 Custom default format for `docker plugin ls` output. See [`docker plugin ls`](https://docs.docker.com/reference/cli/docker/plugin/ls/#format) for a list of supported formatting directives. |
| `psFormat`             | `docker ps` 或 `docker container ps` 输出的自定义默认格式。请参阅 [`docker ps`](https://docs.docker.com/reference/cli/docker/container/ls/#format)。 Custom default format for `docker ps` / `docker container ps` output. See [`docker ps`](https://docs.docker.com/reference/cli/docker/container/ls/#format) for a list of supported formatting directives. |
| `secretFormat`         | `docker secret ls` 输出的自定义默认格式。请参阅 [`docker secret ls`](https://docs.docker.com/reference/cli/docker/secret/ls/#format)。 Custom default format for `docker secret ls` output. See [`docker secret ls`](https://docs.docker.com/reference/cli/docker/secret/ls/#format) for a list of supported formatting directives. |
| `serviceInspectFormat` | `docker service inspect` 输出的自定义默认格式。请参阅 [`docker service inspect`](https://docs.docker.com/reference/cli/docker/service/inspect/#format)。 Custom default format for `docker service inspect` output. See [`docker service inspect`](https://docs.docker.com/reference/cli/docker/service/inspect/#format) for a list of supported formatting directives. |
| `servicesFormat`       | `docker service ls` 输出的自定义默认格式。请参阅 [`docker service ls`](https://docs.docker.com/reference/cli/docker/service/ls/#format)。 Custom default format for `docker service ls` output. See [`docker service ls`](https://docs.docker.com/reference/cli/docker/service/ls/#format) for a list of supported formatting directives. |
| `statsFormat`          | `docker stats` 输出的自定义默认格式。请参阅 [`docker stats`](https://docs.docker.com/reference/cli/docker/container/stats/#format)。 Custom default format for `docker stats` output. See [`docker stats`](https://docs.docker.com/reference/cli/docker/container/stats/#format) for a list of supported formatting directives. |
| `tasksFormat`          | `docker stack ps` 输出的自定义默认格式。请参阅 [`docker stack ps`](https://docs.docker.com/reference/cli/docker/stack/ps/#format)。 Custom default format for `docker stack ps` output. See [`docker stack ps`](https://docs.docker.com/reference/cli/docker/stack/ps/#format) for a list of supported formatting directives. |
| `volumesFormat`        | `docker volume ls` 输出的自定义默认格式。请参阅 [`docker volume ls`](https://docs.docker.com/reference/cli/docker/volume/ls/#format)。 Custom default format for `docker volume ls` output. See [`docker volume ls`](https://docs.docker.com/reference/cli/docker/volume/ls/#format) for a list of supported formatting directives. |

#### Custom HTTP headers

The property `HttpHeaders` specifies a set of headers to include in all messages sent from the Docker client to the daemon. Docker doesn't try to interpret or understand these headers; it simply puts them into the messages. Docker does not allow these headers to change any headers it sets for itself.

​	属性 `HttpHeaders` 指定一组标头，将其包含在 Docker 客户端发送给守护进程的所有消息中。Docker 不会尝试解释或理解这些标头；它只是将这些标头加入消息中。Docker 不允许这些标头更改它自己设定的标头。

Alternatively, use the `DOCKER_CUSTOM_HEADERS` [environment variable](https://docs.docker.com/reference/cli/docker/#environment-variables), which is available in v27.1 and higher. This environment-variable is experimental, and its exact behavior may change.

​	或者，可以使用在 v27.1 及更高版本中提供的环境变量 `DOCKER_CUSTOM_HEADERS` [环境变量](https://docs.docker.com/reference/cli/docker/#environment-variables)。此环境变量为实验性，其确切行为可能会更改。

#### 凭证存储选项 Credential store options

The property `credsStore` specifies an external binary to serve as the default credential store. When this property is set, `docker login` will attempt to store credentials in the binary specified by `docker-credential-<value>` which is visible on `$PATH`. If this property isn't set, credentials are stored in the `auths` property of the CLI configuration file. For more information, see the [**Credential stores** section in the `docker login` documentation](https://docs.docker.com/reference/cli/docker/login/#credential-stores)

​	属性 `credsStore` 指定一个外部二进制文件作为默认凭证存储。当设置此属性时，`docker login` 将尝试将凭证存储在可见于 `$PATH` 的 `docker-credential-<value>` 二进制文件中。如果未设置此属性，凭证将存储在 CLI 配置文件的 `auths` 属性中。更多信息请参见 [`docker login` 文档中的**凭证存储**部分](https://docs.docker.com/reference/cli/docker/login/#credential-stores)。

The property `credHelpers` specifies a set of credential helpers to use preferentially over `credsStore` or `auths` when storing and retrieving credentials for specific registries. If this property is set, the binary `docker-credential-<value>` will be used when storing or retrieving credentials for a specific registry. For more information, see the [**Credential helpers** section in the `docker login` documentation](https://docs.docker.com/reference/cli/docker/login/#credential-helpers)

​	属性 `credHelpers` 指定用于特定注册表的凭证助手，以优先于 `credsStore` 或 `auths` 用于存储和检索凭证。如果设置此属性，则在存储或检索特定注册表的凭证时，将使用 `docker-credential-<value>` 二进制文件。更多信息请参见 [`docker login` 文档中的**凭证助手**部分](https://docs.docker.com/reference/cli/docker/login/#credential-helpers)。

#### 容器的自动代理配置 Automatic proxy configuration for containers

The property `proxies` specifies proxy environment variables to be automatically set on containers, and set as `--build-arg` on containers used during `docker build`. A `"default"` set of proxies can be configured, and will be used for any Docker daemon that the client connects to, or a configuration per host (Docker daemon), for example, `https://docker-daemon1.example.com`. The following properties can be set for each environment:

​	属性 `proxies` 指定在容器上自动设置的代理环境变量，并在 `docker build` 使用的容器中作为 `--build-arg` 设置。可以配置一组 `"default"` 代理，并用于客户端连接的任何 Docker 守护进程，或为每个主机（Docker 守护进程）配置代理，例如 `https://docker-daemon1.example.com`。每个环境可以设置以下属性：

| Property     | Description                                                  |
| :----------- | :----------------------------------------------------------- |
| `httpProxy`  | 容器中的 `HTTP_PROXY` 和 `http_proxy` 默认值，并在 `docker build` 中作为 `--build-arg` Default value of `HTTP_PROXY` and `http_proxy` for containers, and as `--build-arg` on `docker build` |
| `httpsProxy` | 容器中的 `HTTPS_PROXY` 和 `https_proxy` 默认值，并在 `docker build` 中作为 `--build-arg` Default value of `HTTPS_PROXY` and `https_proxy` for containers, and as `--build-arg` on `docker build` |
| `ftpProxy`   | 容器中的 `FTP_PROXY` 和 `ftp_proxy` 默认值，并在 `docker build` 中作为 `--build-arg` Default value of `FTP_PROXY` and `ftp_proxy` for containers, and as `--build-arg` on `docker build` |
| `noProxy`    | 容器中的 `NO_PROXY` 和 `no_proxy` 默认值，并在 `docker build` 中作为 `--build-arg` Default value of `NO_PROXY` and `no_proxy` for containers, and as `--build-arg` on `docker build` |
| `allProxy`   | 容器中的 `ALL_PROXY` 和 `all_proxy` 默认值，并在 `docker build` 中作为 `--build-arg` Default value of `ALL_PROXY` and `all_proxy` for containers, and as `--build-arg` on `docker build` |

These settings are used to configure proxy settings for containers only, and not used as proxy settings for the `docker` CLI or the `dockerd` daemon. Refer to the [environment variables](https://docs.docker.com/reference/cli/docker/#environment-variables) and [HTTP/HTTPS proxy](https://docs.docker.com/engine/daemon/proxy/#httphttps-proxy) sections for configuring proxy settings for the CLI and daemon.

​	这些设置仅用于配置容器的代理设置，不作为 `docker` CLI 或 `dockerd` 守护进程的代理设置。有关 CLI 和守护进程的代理配置，请参阅[环境变量](https://docs.docker.com/reference/cli/docker/#environment-variables)和 [HTTP/HTTPS 代理](https://docs.docker.com/engine/daemon/proxy/#httphttps-proxy) 部分。

> **Warning**
>
> Proxy settings may contain sensitive information (for example, if the proxy requires authentication). Environment variables are stored as plain text in the container's configuration, and as such can be inspected through the remote API or committed to an image when using `docker commit`.
>
> ​	代理设置可能包含敏感信息（例如，代理需要身份验证时）。环境变量以明文形式存储在容器的配置中，因此在使用 `docker commit` 时可以通过远程 API 检查或提交到镜像中。

#### 默认的容器分离键序列 Default key-sequence to detach from containers

Once attached to a container, users detach from it and leave it running using the using `CTRL-p CTRL-q` key sequence. This detach key sequence is customizable using the `detachKeys` property. Specify a `<sequence>` value for the property. The format of the `<sequence>` is a comma-separated list of either a letter [a-Z], or the `ctrl-` combined with any of the following:

​	连接到容器后，用户可以使用 `CTRL-p CTRL-q` 键序列从中分离并保持其运行状态。可以使用 `detachKeys` 属性自定义该分离键序列。指定一个 `<sequence>` 值，格式为逗号分隔的字母或 `ctrl-` 与以下之一的组合：

- `a-z` (a single lowercase alpha character ) （单个小写字母）
- `@` (at sign) （@ 符号）
- `[` (left bracket) （左方括号）
- `\\` (two backward slashes) （双反斜杠）
- `_` (underscore)（下划线）
- `^` (caret) （插入符号）

Your customization applies to all containers started in with your Docker client. Users can override your custom or the default key sequence on a per-container basis. To do this, the user specifies the `--detach-keys` flag with the `docker attach`, `docker exec`, `docker run` or `docker start` command.

​	此自定义应用于使用 Docker 客户端启动的所有容器。用户可以在每个容器的基础上覆盖您自定义的或默认的键序列。为此，用户可以在 `docker attach`、`docker exec`、`docker run` 或 `docker start` 命令中指定 `--detach-keys` 标志。

#### CLI plugin options

The property `plugins` contains settings specific to CLI plugins. The key is the plugin name, while the value is a further map of options, which are specific to that plugin.

​	属性 `plugins` 包含 CLI 插件的特定设置。键为插件名称，值为进一步的选项映射，特定于该插件。

#### Sample configuration file

Following is a sample `config.json` file to illustrate the format used for various fields:

​	以下是一个 `config.json` 文件的示例，用于展示各种字段的格式：

```json
{
  "HttpHeaders": {
    "MyHeader": "MyValue"
  },
  "psFormat": "table {{.ID}}\\t{{.Image}}\\t{{.Command}}\\t{{.Labels}}",
  "imagesFormat": "table {{.ID}}\\t{{.Repository}}\\t{{.Tag}}\\t{{.CreatedAt}}",
  "pluginsFormat": "table {{.ID}}\t{{.Name}}\t{{.Enabled}}",
  "statsFormat": "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}",
  "servicesFormat": "table {{.ID}}\t{{.Name}}\t{{.Mode}}",
  "secretFormat": "table {{.ID}}\t{{.Name}}\t{{.CreatedAt}}\t{{.UpdatedAt}}",
  "configFormat": "table {{.ID}}\t{{.Name}}\t{{.CreatedAt}}\t{{.UpdatedAt}}",
  "serviceInspectFormat": "pretty",
  "nodesFormat": "table {{.ID}}\t{{.Hostname}}\t{{.Availability}}",
  "detachKeys": "ctrl-e,e",
  "credsStore": "secretservice",
  "credHelpers": {
    "awesomereg.example.org": "hip-star",
    "unicorn.example.com": "vcbait"
  },
  "plugins": {
    "plugin1": {
      "option": "value"
    },
    "plugin2": {
      "anotheroption": "anothervalue",
      "athirdoption": "athirdvalue"
    }
  },
  "proxies": {
    "default": {
      "httpProxy":  "http://user:pass@example.com:3128",
      "httpsProxy": "https://my-proxy.example.com:3129",
      "noProxy":    "intra.mycorp.example.com",
      "ftpProxy":   "http://user:pass@example.com:3128",
      "allProxy":   "socks://example.com:1234"
    },
    "https://manager1.mycorp.example.com:2377": {
      "httpProxy":  "http://user:pass@example.com:3128",
      "httpsProxy": "https://my-proxy.example.com:3129"
    }
  }
}
```

#### Experimental features

Experimental features provide early access to future product functionality. These features are intended for testing and feedback, and they may change between releases without warning or can be removed from a future release.

​	实验性功能提供对未来产品功能的早期访问。这些功能旨在测试和反馈，在发布之间可能会发生变化，或可能从未来版本中移除。

Starting with Docker 20.10, experimental CLI features are enabled by default, and require no configuration to enable them.

​	从 Docker 20.10 开始，实验性 CLI 功能默认启用，无需配置即可启用。

#### Notary

If using your own notary server and a self-signed certificate or an internal Certificate Authority, you need to place the certificate at `tls/<registry_url>/ca.crt` in your Docker config directory.

​	如果使用自有的 Notary 服务器以及自签名证书或内部证书颁发机构，则需将证书放置在 Docker 配置目录的 `tls/<registry_url>/ca.crt` 中。

Alternatively you can trust the certificate globally by adding it to your system's list of root Certificate Authorities.

​	或者，您可以通过将证书添加到系统的根证书颁发机构列表中来全局信任该证书。

## Options

| Option                                                       | Default                  | Description                                                  |
| ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ |
| `--config`                                                   | `/root/.docker`          | 客户端配置文件的位置 Location of client config files         |
| `-c, --context`                                              |                          | 要用于连接守护进程的上下文名称（覆盖 `DOCKER_HOST` 环境变量以及通过 `docker context use` 设置的默认上下文）  Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with `docker context use`) |
| `-D, --debug`                                                |                          | 启用调试模式 Enable debug mode                               |
| [`-H, --host`](https://docs.docker.com/reference/cli/docker/#host) |                          | 要连接的守护进程套接字 Daemon socket to connect to           |
| `-l, --log-level`                                            | `info`                   | 设置日志记录级别（`debug`、`info`、`warn`、`error`、`fatal`） Set the logging level (`debug`, `info`, `warn`, `error`, `fatal`) |
| `--tls`                                                      |                          | 使用 TLS；由 `--tlsverify` 隐式启用 Use TLS; implied by `--tlsverify` |
| `--tlscacert`                                                | `/root/.docker/ca.pem`   | 仅信任由此 CA 签署的证书  Trust certs signed only by this CA |
| `--tlscert`                                                  | `/root/.docker/cert.pem` | TLS 证书文件的路径 Path to TLS certificate file              |
| `--tlskey`                                                   | `/root/.docker/key.pem`  | TLS 密钥文件的路径 Path to TLS key file                      |
| `--tlsverify`                                                |                          | 使用 TLS 并验证远程服务器 Use TLS and verify the remote      |

## Examples

### Specify daemon host (`-H, --host`)

You can use the `-H`, `--host` flag to specify a socket to use when you invoke a `docker` command. You can use the following protocols:

​	可以使用 `-H` 或 `--host` 标志在执行 `docker` 命令时指定要使用的套接字。支持以下协议：

| Scheme                                 | Description                                        | Example                          |
| -------------------------------------- | -------------------------------------------------- | -------------------------------- |
| `unix://[<path>]`                      | Unix 套接字（仅限 Linux） Unix socket (Linux only) | `unix:///var/run/docker.sock`    |
| `tcp://[<IP or host>[:port]]`          | TCP connection                                     | `tcp://174.17.0.1:2376`          |
| `ssh://[username@]<IP or host>[:port]` | SSH connection                                     | `ssh://user@192.168.64.5`        |
| `npipe://[<name>]`                     | 命名管道（仅限 Windows）Named pipe (Windows only)  | `npipe:////./pipe/docker_engine` |

If you don't specify the `-H` flag, and you're not using a custom [context](https://docs.docker.com/engine/context/working-with-contexts), commands use the following default sockets:

​	如果未指定 `-H` 标志且未使用自定义[上下文](https://docs.docker.com/engine/context/working-with-contexts)，命令将使用以下默认套接字：

- `unix:///var/run/docker.sock` on macOS and Linux
- `npipe:////./pipe/docker_engine` on Windows

To achieve a similar effect without having to specify the `-H` flag for every command, you could also [create a context]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextcreate" >}}), or alternatively, use the [`DOCKER_HOST` environment variable](https://docs.docker.com/reference/cli/docker/#environment-variables).

​	要获得类似效果而无需在每个命令中指定 `-H` 标志，还可以[创建上下文]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextcreate" >}})，或使用 [`DOCKER_HOST` 环境变量](https://docs.docker.com/reference/cli/docker/#environment-variables)。

For more information about the `-H` flag, see [Daemon socket option](https://docs.docker.com/reference/cli/dockerd/#daemon-socket-option).

​	有关 `-H` 标志的更多信息，请参见[守护进程套接字选项](https://docs.docker.com/reference/cli/dockerd/#daemon-socket-option)。

#### 使用 TCP 套接字 Using TCP sockets

The following example shows how to invoke `docker ps` over TCP, to a remote daemon with IP address `174.17.0.1`, listening on port `2376`:

​	以下示例展示了如何通过 TCP 调用 `docker ps`，以连接到 IP 地址为 `174.17.0.1`、端口为 `2376` 的远程守护进程：

```console
$ docker -H tcp://174.17.0.1:2376 ps
```

> **Note**
>
> By convention, the Docker daemon uses port `2376` for secure TLS connections, and port `2375` for insecure, non-TLS connections.
>
> ​	通常，Docker 守护进程使用端口 `2376` 进行安全的 TLS 连接，使用端口 `2375` 进行不安全的非 TLS 连接。

#### Using SSH sockets

When you use SSH invoke a command on a remote daemon, the request gets forwarded to the `/var/run/docker.sock` Unix socket on the SSH host.

​	当通过 SSH 使用命令连接到远程守护进程时，请求会被转发到 SSH 主机上的 `/var/run/docker.sock` Unix 套接字。

```console
$ docker -H ssh://user@192.168.64.5 ps
```

You can optionally specify the location of the socket by appending a path component to the end of the SSH address.

​	可以通过在 SSH 地址末尾添加路径组件来指定套接字的位置。

```console
$ docker -H ssh://user@192.168.64.5/var/run/docker.sock ps
```

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker build (legacy builder)`]({{< ref "/reference/CLIreference/docker/dockerbuildlegacybuilder" >}}) | 从 Dockerfile 构建镜像   Build an image from a Dockerfile    |
| [`docker builder`]({{< ref "/reference/CLIreference/docker/dockerbuilder" >}}) | 管理构建 Manage builds                                       |
| [`docker buildx`]({{< ref "/reference/CLIreference/docker/dockerbuildx" >}}) | Docker Buildx                                                |
| [`docker checkpoint`]({{< ref "/reference/CLIreference/docker/dockercheckpoint" >}}) | 管理检查点  Manage checkpoints                               |
| [`docker compose`]({{< ref "/reference/CLIreference/docker/dockercompose" >}}) | Docker Compose                                               |
| [`docker config`]({{< ref "/reference/CLIreference/docker/dockerconfig" >}}) | 管理 Swarm 配置  Manage Swarm configs                        |
| [`docker container`]({{< ref "/reference/CLIreference/docker/dockercontainer" >}}) | 管理容器 Manage containers                                   |
| [`docker context`]({{< ref "/reference/CLIreference/docker/dockercontext" >}}) | 管理上下文 Manage contexts                                   |
| [`docker debug`]({{< ref "/reference/CLIreference/docker/dockerdebug" >}}) | 获取进入任意容器或镜像的 shell，`docker exec` 的调试替代方法  Get a shell into any container or image. An alternative to debugging with `docker exec`. |
| [`docker image`]({{< ref "/reference/CLIreference/docker/dockerimage" >}}) | 管理镜像  Manage images                                      |
| [`docker init`]({{< ref "/reference/CLIreference/docker/dockerinit" >}}) | 为项目创建 Docker 相关的初始文件  Creates Docker-related starter files for your project |
| [`docker inspect`]({{< ref "/reference/CLIreference/docker/dockerinspect" >}}) | 返回 Docker 对象的低级信息  Return low-level information on Docker objects |
| [`docker login`]({{< ref "/reference/CLIreference/docker/dockerlogin" >}}) | 认证到注册表  Authenticate to a registry                     |
| [`docker logout`]({{< ref "/reference/CLIreference/docker/dockerlogout" >}}) | 从注册表注销  Log out from a registry                        |
| [`docker manifest`]({{< ref "/reference/CLIreference/docker/dockermanifest" >}}) | 管理 Docker 镜像清单和清单列表  Manage Docker image manifests and manifest lists |
| [`docker network`]({{< ref "/reference/CLIreference/docker/dockernetwork" >}}) | 管理网络  Manage networks                                    |
| [`docker node`]({{< ref "/reference/CLIreference/docker/dockernode" >}}) | 管理 Swarm 节点 Manage Swarm nodes                           |
| [`docker plugin`]({{< ref "/reference/CLIreference/docker/dockerplugin" >}}) | 管理插件  Manage plugins                                     |
| [`docker scout`]({{< ref "/reference/CLIreference/docker/dockerscout" >}}) | Docker Scout 的命令行工具 Command line tool for Docker Scout |
| [`docker search`]({{< ref "/reference/CLIreference/docker/dockersearch" >}}) | 在 Docker Hub 中搜索镜像 Search Docker Hub for images        |
| [`docker secret`]({{< ref "/reference/CLIreference/docker/dockersecret" >}}) | 管理 Swarm 密钥 Manage Swarm secrets                         |
| [`docker service`]({{< ref "/reference/CLIreference/docker/dockerservice" >}}) | 管理 Swarm 服务 Manage Swarm services                        |
| [`docker stack`]({{< ref "/reference/CLIreference/docker/dockerstack" >}}) | 管理 Swarm 栈  Manage Swarm stacks                           |
| [`docker swarm`]({{< ref "/reference/CLIreference/docker/dockerswarm" >}}) | 管理 Swarm  Manage Swarm                                     |
| [`docker system`]({{< ref "/reference/CLIreference/docker/dockersystem" >}}) | 管理 Docker Manage Docker                                    |
| [`docker trust`]({{< ref "/reference/CLIreference/docker/dockertrust" >}}) | 管理 Docker 镜像的信任 Manage trust on Docker images         |
| [`docker version`]({{< ref "/reference/CLIreference/docker/dockerversion" >}}) | 显示 Docker 版本信息 Show the Docker version information     |
| [`docker volume`]({{< ref "/reference/CLIreference/docker/dockervolume" >}}) | 管理卷 Manage volumes                                        |
