+++
title = "预定义环境变量"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/environment-variables/envvars/](https://docs.docker.com/compose/how-tos/environment-variables/envvars/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Set or change pre-defined environment variables in Docker Compose - 在 Docker Compose 中设置或更改预定义环境变量

Compose already comes with pre-defined environment variables. It also inherits common Docker CLI environment variables, such as `DOCKER_HOST` and `DOCKER_CONTEXT`. See [Docker CLI environment variable reference](https://docs.docker.com/reference/cli/docker/#environment-variables) for details.

​	Compose 已经包含一些预定义的环境变量，并继承了一些常用的 Docker CLI 环境变量，如 `DOCKER_HOST` 和 `DOCKER_CONTEXT`。详细信息参见 [Docker CLI 环境变量参考](https://docs.docker.com/reference/cli/docker/#environment-variables)。

This page contains information on how you can set or change the following pre-defined environment variables if you need to:

​	以下是如何设置或更改预定义环境变量的说明：

- `COMPOSE_CONVERT_WINDOWS_PATHS`

- `COMPOSE_FILE`
- `COMPOSE_PROFILES`
- `COMPOSE_PROJECT_NAME`
- `DOCKER_CERT_PATH`
- `COMPOSE_PARALLEL_LIMIT`
- `COMPOSE_IGNORE_ORPHANS`
- `COMPOSE_REMOVE_ORPHANS`
- `COMPOSE_PATH_SEPARATOR`
- `COMPOSE_ANSI`
- `COMPOSE_STATUS_STDOUT`
- `COMPOSE_ENV_FILES`
- `COMPOSE_MENU`
- `COMPOSE_EXPERIMENTAL`

## 覆盖方法 Methods to override

You can set or change the pre-defined environment variables:

​	可以通过以下方式设置或更改预定义环境变量：

- With an [`.env` file located in your working director]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Interpolation" >}})
  - 使用[工作目录中的 `.env` 文件]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Interpolation" >}})

- From the command line
  - 从命令行设置

- From your [shell](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#substitute-from-the-shell)
  - 从[Shell 环境](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#substitute-from-the-shell)设置


When changing or setting any environment variables, be aware of [Environment variable precedence]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Environmentvariablesprecedence" >}}).

​	在更改或设置任何环境变量时，请注意[环境变量优先级]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Environmentvariablesprecedence" >}})。

## 配置 Configure

### COMPOSE_PROJECT_NAME

Sets the project name. This value is prepended along with the service name to the container's name on startup.

​	设置项目名称。该值会与服务名称一起添加到容器的名称中。

For example, if your project name is `myapp` and it includes two services `db` and `web`, then Compose starts containers named `myapp-db-1` and `myapp-web-1` respectively.

​	例如，如果项目名称为 `myapp`，包含两个服务 `db` 和 `web`，则 Compose 启动的容器分别命名为 `myapp-db-1` 和 `myapp-web-1`。

Compose can set the project name in different ways. The level of precedence (from highest to lowest) for each method is as follows:

​	Compose 可以通过不同方式设置项目名称，优先级（从高到低）如下：

1. The `-p` command line flag 命令行标志 `-p`
2. `COMPOSE_PROJECT_NAME`
3. The top level `name:` variable from the config file (or the last `name:` from a series of config files specified using `-f`) 配置文件中的顶级变量 `name:`（或使用 `-f` 指定的多个配置文件中的最后一个 `name:`）
4. The `basename` of the project directory containing the config file (or containing the first config file specified using `-f`) 包含配置文件的项目目录的 `basename`（或使用 `-f` 指定的第一个配置文件）
5. The `basename` of the current directory if no config file is specified 如果未指定配置文件，则为当前目录的 `basename`

Project names must contain only lowercase letters, decimal digits, dashes, and underscores, and must begin with a lowercase letter or decimal digit. If the `basename` of the project directory or current directory violates this constraint, you must use one of the other mechanisms.

​	项目名称只能包含小写字母、数字、短划线和下划线，并且必须以小写字母或数字开头。如果项目目录的 `basename` 或当前目录不符合该限制，则必须使用其他机制。

See also the [command-line options overview](https://docs.docker.com/reference/cli/docker/compose/#command-options-overview-and-help) and [using `-p` to specify a project name](https://docs.docker.com/reference/cli/docker/compose/#use--p-to-specify-a-project-name).

​	另请参阅[命令行选项概述](https://docs.docker.com/reference/cli/docker/compose/#command-options-overview-and-help)和[使用 `-p` 指定项目名称](https://docs.docker.com/reference/cli/docker/compose/#use--p-to-specify-a-project-name)。

### COMPOSE_FILE

Specifies the path to a Compose file. Specifying multiple Compose files is supported.

​	指定 Compose 文件的路径，支持多个 Compose 文件。

- Default behavior: If not provided, Compose looks for a file named `compose.yaml` or `docker-compose.yaml` in the current directory and, if not found, then Compose searches each parent directory recursively until a file by that name is found.
  - 默认行为：如果未提供路径，Compose 会在当前目录中查找名为 `compose.yaml` 或 `docker-compose.yaml` 的文件，如果未找到，则递归查找每个父目录，直到找到该文件。

- Default separator: When specifying multiple Compose files, the path separators are, by default, on: 默认分隔符：指定多个 Compose 文件时，路径分隔符为：
  - Mac and Linux: `:` (colon),
  - Windows: `;` (semicolon).

The path separator can also be customized using `COMPOSE_PATH_SEPARATOR`.

​	可以使用 `COMPOSE_PATH_SEPARATOR` 自定义路径分隔符。

Example: `COMPOSE_FILE=docker-compose.yml:docker-compose.prod.yml`.

​	示例：`COMPOSE_FILE=docker-compose.yml:docker-compose.prod.yml`。

See also the [command-line options overview](https://docs.docker.com/reference/cli/docker/compose/#command-options-overview-and-help) and [using `-f` to specify name and path of one or more Compose files](https://docs.docker.com/reference/cli/docker/compose/#use--f-to-specify-name-and-path-of-one-or-more-compose-files).

​	另请参阅[命令行选项概述](https://docs.docker.com/reference/cli/docker/compose/#command-options-overview-and-help)和[使用 `-f` 指定一个或多个 Compose 文件的名称和路径](https://docs.docker.com/reference/cli/docker/compose/#use--f-to-specify-name-and-path-of-one-or-more-compose-files)。

### COMPOSE_PROFILES

Specifies one or more profiles to be enabled on `compose up` execution. Services with matching profiles are started as well as any services for which no profile has been defined.

​	指定在执行 `compose up` 时启用的一个或多个配置文件。匹配配置文件的服务将启动，未定义配置文件的服务也将启动。

For example, calling `docker compose up`with `COMPOSE_PROFILES=frontend` selects services with the `frontend` profile as well as any services without a profile specified.

​	例如，使用 `COMPOSE_PROFILES=frontend` 执行 `docker compose up` 时，将启动具有 `frontend` 配置文件的服务以及没有指定配置文件的服务。

- Default separator: specify a list of profiles using a comma as separator.
  - 默认分隔符：使用逗号分隔配置文件列表。

Example: `COMPOSE_PROFILES=frontend,debug`
This example enables all services matching both the `frontend` and `debug` profiles and services without a profile.

​	此示例启用与 `frontend` 和 `debug` 配置文件匹配的所有服务以及没有配置文件的服务。

See also [Using profiles with Compose]({{< ref "/manuals/DockerCompose/How-tos/Useserviceprofiles" >}}) and the [`--profile` command-line option](https://docs.docker.com/reference/cli/docker/compose/#use---profile-to-specify-one-or-more-active-profiles).

​	另请参阅[在 Compose 中使用配置文件]({{< ref "/manuals/DockerCompose/How-tos/Useserviceprofiles" >}})和[`--profile` 命令行选项](https://docs.docker.com/reference/cli/docker/compose/#use---profile-to-specify-one-or-more-active-profiles)。

### COMPOSE_CONVERT_WINDOWS_PATHS

When enabled, Compose performs path conversion from Windows-style to Unix-style in volume definitions.

​	启用时，Compose 会在卷定义中将 Windows 路径转换为 Unix 路径。

- Supported values:
  - `true` or `1`, to enable,  `true` 或 `1` 启用，
  - `false` or `0`, to disable. `false` 或 `0` 禁用。
- Defaults to: `0`. 默认为：`0`。

### COMPOSE_PATH_SEPARATOR

Specifies a different path separator for items listed in `COMPOSE_FILE`.

​	指定 `COMPOSE_FILE` 中项目列表的路径分隔符。

- Defaults to: 默认值：
  - On macOS and Linux to `:`, macOS 和 Linux 为 `:`，
  - On Windows to`;`.  Windows 为 `;`。

### COMPOSE_IGNORE_ORPHANS

When enabled, Compose doesn't try to detect orphaned containers for the project.

​	启用时，Compose 不会尝试检测项目的孤立容器。

- Supported values: 
  - `true` or `1`, to enable, `true` 或 `1` 启用，
  - `false` or `0`, to disable. `false` 或 `0` 禁用。
- Defaults to: `0`. 默认为：`0`。

### COMPOSE_PARALLEL_LIMIT

Specifies the maximum level of parallelism for concurrent engine calls.

​	指定并行引擎调用的最大并发级别。

### COMPOSE_ANSI

Specifies when to print ANSI control characters.

​	指定何时打印 ANSI 控制字符。

- Supported values:
  - `auto`, Compose detects if TTY mode can be used. Otherwise, use plain text mode. Compose 检测是否可以使用 TTY 模式，否则使用纯文本模式。
  - `never`, use plain text mode. 使用纯文本模式。
  - `always` or `0`, use TTY mode. 使用 TTY 模式。
- Defaults to: `auto`.

### COMPOSE_STATUS_STDOUT

When enabled, Compose writes its internal status and progress messages to `stdout` instead of `stderr`. The default value is false to clearly separate the output streams between Compose messages and your container's logs.

​	启用时，Compose 将其内部状态和进度消息写入 `stdout` 而不是 `stderr`。默认值为 false，以便清晰地区分 Compose 消息和容器日志的输出流。

- Supported values:
  - `true` or `1`, to enable, `true` 或 `1` 启用，
  - `false` or `0`, to disable. `false` 或 `0` 禁用。
- Defaults to: `0`. 默认为：`0`。

### COMPOSE_ENV_FILES

Lets you specify which environment files Compose should use if `--env-file` isn't used.

​	如果未使用 `--env-file`，此变量指定 Compose 使用的环境文件。

When using multiple environment files, use a comma as a separator. For example,

​	使用多个环境文件时，使用逗号作为分隔符。例如：

```console
COMPOSE_ENV_FILES=.env.envfile1, .env.envfile2
```

If `COMPOSE_ENV_FILES` is not set, and you don't provide `--env-file` in the CLI, Docker Compose uses the default behavior, which is to look for an `.env` file in the project directory.

​	如果未设置 `COMPOSE_ENV_FILES`，且未在 CLI 中提供 `--env-file`，则 Docker Compose 使用默认行为，即在项目目录中查找 `.env` 文件。

### COMPOSE_MENU

> Available in Docker Compose version [2.26.0](https://docs.docker.com/compose/releases/release-notes/#2260) and later, and Docker Desktop version 4.29 and later.
>
> ​	Docker Compose 版本 [2.26.0](https://docs.docker.com/compose/releases/release-notes/#2260) 及更高版本和 Docker Desktop 版本 4.29 及更高版本中可用。

When enabled, Compose displays a navigation menu where you can choose to open the Compose stack in Docker Desktop, switch on [`watch` mode]({{< ref "/manuals/DockerCompose/How-tos/UseComposeWatch" >}}), or use [Docker Debug]({{< ref "/reference/CLIreference/docker/dockerdebug" >}}).

​	启用时，Compose 会显示导航菜单，您可以选择在 Docker Desktop 中打开 Compose 堆栈、启用 [`watch` 模式]({{< ref "/manuals/DockerCompose/How-tos/UseComposeWatch" >}})，或使用 [Docker Debug]({{< ref "/reference/CLIreference/docker/dockerdebug" >}})。

- Supported values:
  - `true` or `1`, to enable, `true` 或 `1` 启用，
  - `false` or `0`, to disable. `false` 或 `0` 禁用。
- Defaults to: `1` if you obtained Docker Compose through Docker Desktop, otherwise default is `0`.
  - 默认为：如果通过 Docker Desktop 获得 Docker Compose，则默认值为 `1`，否则默认为 `0`。


### COMPOSE_EXPERIMENTAL

> Available in Docker Compose version [2.26.0](https://docs.docker.com/compose/releases/release-notes/#2260) and later, and Docker Desktop version 4.29 and later.
>
> ​	Docker Compose 版本 [2.26.0](https://docs.docker.com/compose/releases/release-notes/#2260) 及更高版本和 Docker Desktop 版本 4.29 及更高版本中可用。

This is an opt-out variable. When turned off it deactivates the experimental features such as the navigation menu or [Synchronized file shares]({{< ref "/manuals/DockerDesktop/Synchronizedfileshares" >}}).

​	此变量为选择性关闭变量。关闭时会停用实验功能，如导航菜单或[同步文件共享]({{< ref "/manuals/DockerDesktop/Synchronizedfileshares" >}})。

- Supported values:
  - `true` or `1`, to enable, `true` 或 `1` 启用，
  - `false` or `0`, to disable. `false` 或 `0` 禁用。
- Defaults to: `1`.

## Compose V2 不支持的变量 Unsupported in Compose V2

The following environment variables have no effect in Compose V2. For more information, see [Migrate to Compose V2]({{< ref "/manuals/DockerCompose/Releases/MigratetoComposeV2" >}}).

​	以下环境变量在 Compose V2 中无效。有关更多信息，请参阅[迁移到 Compose V2]({{< ref "/manuals/DockerCompose/Releases/MigratetoComposeV2" >}})。

- `COMPOSE_API_VERSION` By default the API version is negotiated with the server. Use `DOCKER_API_VERSION`.
  See the [Docker CLI environment variable reference](https://docs.docker.com/reference/cli/docker/#environment-variables) page.
  - `COMPOSE_API_VERSION` 默认情况下，API 版本会与服务器进行协商。使用 `DOCKER_API_VERSION`。 请参阅 [Docker CLI 环境变量参考](https://docs.docker.com/reference/cli/docker/#environment-variables) 页面。
- `COMPOSE_HTTP_TIMEOUT`
- `COMPOSE_TLS_VERSION`
- `COMPOSE_FORCE_WINDOWS_HOST`
- `COMPOSE_INTERACTIVE_NO_CLI`
- `COMPOSE_DOCKER_CLI_BUILD` Use `DOCKER_BUILDKIT` to select between BuildKit and the classic builder. If `DOCKER_BUILDKIT=0` then `docker compose build` uses the classic builder to build images.
  - `COMPOSE_DOCKER_CLI_BUILD` 使用 `DOCKER_BUILDKIT` 在 BuildKit 和经典构建器之间选择。如果 `DOCKER_BUILDKIT=0`，则 `docker compose build` 使用经典构建器构建镜像。
