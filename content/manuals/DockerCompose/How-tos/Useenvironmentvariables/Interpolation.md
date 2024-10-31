+++
title = "插值"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Set, use, and manage variables in a Compose file with interpolation - 在 Compose 文件中使用插值设置、使用和管理变量

A Compose file can use variables to offer more flexibility. If you want to quickly switch between image tags to test multiple versions, or want to adjust a volume source to your local environment, you don't need to edit the Compose file each time, you can just set variables that insert values into your Compose file at run time.

​	Compose 文件可以使用变量以增加灵活性。如果你希望在多个版本的镜像标签之间快速切换进行测试，或希望根据本地环境调整卷源位置，则无需每次编辑 Compose 文件，只需设置变量，在运行时将值插入到 Compose 文件中。

Interpolation can also be used to insert values into your Compose file at run time, which is then used to pass variables into your container's environment

​	插值还可以在运行时将值插入到 Compose 文件中，以将变量传递给容器的环境。

Below is a simple example:

​	以下是一个简单示例：

```console
$ cat .env
TAG=v1.5
$ cat compose.yml
services:
  web:
    image: "webapp:${TAG}"
```

When you run `docker compose up`, the `web` service defined in the Compose file [interpolates]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Interpolation" >}}) in the image `webapp:v1.5` which was set in the `.env` file. You can verify this with the [config command]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeconfig" >}}), which prints your resolved application config to the terminal:

​	当你运行 `docker compose up` 时，Compose 文件中定义的 `web` 服务将会[插值]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Interpolation" >}})为镜像 `webapp:v1.5`，该值在 `.env` 文件中设置。你可以使用 [config 命令]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeconfig" >}})在终端中打印出已解析的应用配置以进行验证：



```console
$ docker compose config
services:
  web:
    image: 'webapp:v1.5'
```

## 插值语法 Interpolation syntax

Interpolation is applied for unquoted and double-quoted values. Both braced (`${VAR}`) and unbraced (`$VAR`) expressions are supported.

​	插值可用于未加引号和双引号的值。支持使用括号的 (`${VAR}`) 和不使用括号的 (`$VAR`) 表达式。

For braced expressions, the following formats are supported:

​	对于带括号的表达式，支持以下格式：

- Direct substitution 直接替换
  - `${VAR}` -> value of `VAR`
- Default value 默认值
  - `${VAR:-default}` -> value of `VAR` if set and non-empty, otherwise `default `如果 `VAR` 设置且非空则使用其值，否则为 `default`
  - `${VAR-default}` -> value of `VAR` if set, otherwise `default` 如果 `VAR` 设置则使用其值，否则为 `default`
- Required value 必须的值
  - `${VAR:?error}` -> value of `VAR` if set and non-empty, otherwise exit with error 如果 `VAR` 设置且非空则使用其值，否则退出并报错
  - `${VAR?error}` -> value of `VAR` if set, otherwise exit with error 如果 `VAR` 设置则使用其值，否则退出并报错
- Alternative value 替代值
  - `${VAR:+replacement}` -> `replacement` if `VAR` is set and non-empty, otherwise empty 如果 `VAR` 设置且非空则使用 `replacement`，否则为空
  - `${VAR+replacement}` -> `replacement` if `VAR` is set, otherwise empty 如果 `VAR` 设置则使用 `replacement`，否则为空

For more information, see [Interpolation]({{< ref "/reference/Composefilereference/Interpolation" >}}) in the Compose Specification.

​	更多信息请参见 Compose 规范中的 [插值]({{< ref "/reference/Composefilereference/Interpolation" >}})。

## 使用插值设置变量的方式 Ways to set variables with interpolation

Docker Compose can interpolate variables into your Compose file from multiple sources.

​	Docker Compose 可以从多个来源插入变量到 Compose 文件中。

Note that when the same variable is declared by multiple sources, precedence applies:

​	注意，当同一变量由多个来源声明时，会应用优先级：

1. Variables from your shell environment Shell 环境中的变量
2. If `--env-file` is not set, variables set by an `.env` file in local working directory (`PWD`) 如果未设置 `--env-file`，则使用本地工作目录 (`PWD`) 中 `.env` 文件中的变量
3. Variables from a file set by `--env-file` or an `.env` file in project directory `--env-file` 设置的文件或项目目录中的 `.env` 文件中的变量

You can check variables and values used by Compose to interpolate the Compose model by running `docker compose config --environment`.

​	你可以运行 `docker compose config --environment` 查看 Compose 用来插值 Compose 模型的变量及其值。

### `.env` file

An `.env` file in Docker Compose is a text file used to define variables that should be made available for interpolation when running `docker compose up`. This file typically contains key-value pairs of variables, and it lets you centralize and manage configuration in one place. The `.env` file is useful if you have multiple variables you need to store.

​	在 Docker Compose 中，`.env` 文件是一个文本文件，用于定义在运行 `docker compose up` 时进行插值的变量。该文件通常包含变量的键值对，可以让你集中管理配置。对于需要存储的多个变量，`.env` 文件非常有用。

The `.env` file is the default method for setting variables. The `.env` file should be placed at the root of the project directory next to your `compose.yaml` file. For more information on formatting an environment file, see [Syntax for environment files](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#env-file-syntax).

​	`.env` 文件是设置变量的默认方法，通常应放在项目目录根目录下，与 `compose.yaml` 文件位于同一位置。有关环境文件的格式详细信息，请参见 [环境文件的语法](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#env-file-syntax)。

Basic example:

​	基本示例：

```console
$ cat .env
## define COMPOSE_DEBUG based on DEV_MODE, defaults to false
## 根据 DEV_MODE 定义 COMPOSE_DEBUG，默认为 false
COMPOSE_DEBUG=${DEV_MODE:-false}

$ cat compose.yaml 
  services:
    webapp:
      image: my-webapp-image
      environment:
        - DEBUG=${COMPOSE_DEBUG}

$ DEV_MODE=true docker compose config
services:
  webapp:
    environment:
      DEBUG: "true"
```

#### 其他信息 Additional information

- If you define a variable in your `.env` file, you can reference it directly in your `compose.yml` with the [`environment` attribute](https://docs.docker.com/reference/compose-file/services/#environment). For example, if your `.env` file contains the environment variable `DEBUG=1` and your `compose.yml` file looks like this:

  如果你在 `.env` 文件中定义了变量，可以直接在 `compose.yml` 中通过 [`environment` 属性](https://docs.docker.com/reference/compose-file/services/#environment)引用它。例如，如果你的 `.env` 文件包含环境变量 `DEBUG=1`，并且 `compose.yml` 文件如下所示：

  ```yaml
   services:
     webapp:
       image: my-webapp-image
       environment:
         - DEBUG=${DEBUG}
  ```

  Docker Compose replaces `${DEBUG}` with the value from the `.env` file

  ​	Docker Compose 会将 `${DEBUG}` 替换为 `.env` 文件中的值

  > **Important**
  >
  > 
  >
  > Be aware of [Environment variables precedence]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Environmentvariablesprecedence" >}}) when using variables in an `.env` file that as environment variables in your container's environment.
  >
  > ​	使用 `.env` 文件中的变量作为容器环境中的环境变量时，请注意[环境变量的优先级]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Environmentvariablesprecedence" >}})。

- You can place your `.env` file in a location other than the root of your project's directory, and then use the [`--env-file` option in the CLI](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#substitute-with---env-file) so Compose can navigate to it.

  - 可以将 `.env` 文件放在项目目录根目录以外的位置，然后在 CLI 中使用 [`--env-file` 选项](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#substitute-with---env-file)让 Compose 能够找到它。

- Your `.env` file can be overridden by another `.env` if it is [substituted with `--env-file`](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#substitute-with---env-file).

  - `.env` 文件可以被另一个 `.env` 文件覆盖，如果它被 [`--env-file` 替代](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#substitute-with---env-file)。


> **Important**
>
> 
>
> Substitution from `.env` files is a Docker Compose CLI feature.
>
> ​	`.env` 文件的替代是 Docker Compose CLI 的一个特性。
>
> It is not supported by Swarm when running `docker stack deploy`.
>
> ​	它在使用 `docker stack deploy` 运行 Swarm 时不受支持。

#### `.env` file syntax

The following syntax rules apply to environment files:

​	环境文件的语法规则如下

- Lines beginning with `#` are processed as comments and ignored.
  - 以 `#` 开头的行被处理为注释并忽略。

- Blank lines are ignored.
  - 空行被忽略。

- Unquoted and double-quoted (`"`) values have interpolation applied.
  - 未加引号和双引号 (`"`) 的值会进行插值。

- Each line represents a key-value pair. Values can optionally be quoted.
  - 每行代表一个键值对。值可以选择性地加引号。
  - `VAR=VAL` -> `VAL`
  - `VAR="VAL"` -> `VAL`
  - `VAR='VAL'` -> `VAL`

- Inline comments for unquoted values must be preceded with a space.
  - 未加引号的值中，内联注释前必须有空格。
  - `VAR=VAL # comment` -> `VAL`
  - `VAR=VAL# not a comment` -> `VAL# not a comment`

- Inline comments for quoted values must follow the closing quote.
  - 对于加引号的值，内联注释必须紧跟在闭合引号之后。
  - `VAR="VAL # not a comment"` -> `VAL # not a comment`
  - `VAR="VAL" # comment` -> `VAL`

- Single-quoted (`'`) values are used literally.  `'`的值按字面量使用。

  - `VAR='$OTHER'` -> `$OTHER`
  - `VAR='${OTHER}'` -> `${OTHER}`
- Quotes can be escaped with `\`. 可以用`\`来转义引号。

  - `VAR='Let\'s go!'` -> `Let's go!`
  - `VAR="{\"hello\": \"json\"}"` -> `{"hello": "json"}`
- Common shell escape sequences including `\n`, `\r`, `\t`, and `\\` are supported in double-quoted values. 在双引号的值中支持常见的 shell 转义序列 `\n`, `\r`, `\t`和`\\`。

  - `VAR="some\tvalue"` -> `some value`
  - `VAR='some\tvalue'` -> `some\tvalue`
  - `VAR=some\tvalue` -> `some\tvalue`

### 使用 `--env-file` 进行替代 Substitute with `--env-file`

You can set default values for multiple environment variables, in an `.env` file and then pass the file as an argument in the CLI.

​	你可以在 `.env` 文件中为多个环境变量设置默认值，然后在 CLI 中将该文件作为参数传递。

The advantage of this method is that you can store the file anywhere and name it appropriately, for example, This file path is relative to the current working directory where the Docker Compose command is executed. Passing the file path is done using the `--env-file` option:

​	这样可以将文件存储在任何位置，并根据需要命名。例如，此文件路径相对于执行 Docker Compose 命令的当前工作目录。传递文件路径使用 `--env-file` 选项：

```console
$ docker compose --env-file ./config/.env.dev up
```

#### 其他信息 Additional information

- This method is useful if you want to temporarily override an `.env` file that is already referenced in your `compose.yml` file. For example you may have different `.env` files for production ( `.env.prod`) and testing (`.env.test`). In the following example, there are two environment files, `.env` and `.env.dev`. Both have different values set for `TAG`.

  此方法在临时覆盖已在 `compose.yml` 文件中引用的 `.env` 文件时很有用。例如，可以为生产环境 (`.env.prod`) 和测试环境 (`.env.test`) 使用不同的 `.env` 文件。以下示例中有两个环境文件，`.env` 和 `.env.dev`，两者为 `TAG` 设置了不同的值。

  ```console
  $ cat .env
  TAG=v1.5
$ cat ./config/.env.dev
  TAG=v1.6
$ cat compose.yml
  services:
  web:
      image: "webapp:${TAG}"
```
  
  If the `--env-file` is not used in the command line, the `.env` file is loaded by default:
  
如果命令行中未使用 `--env-file` 参数，则默认加载 `.env` 文件：
  
```console
  $ docker compose config
services:
    web:
    image: 'webapp:v1.5'
  ```
  
  Passing the `--env-file` argument overrides the default file path:

  传递 `--env-file` 参数会覆盖默认文件路径：

  ```console
$ docker compose --env-file ./config/.env.dev config
  services:
  web:
      image: 'webapp:v1.6'
  ```
  
When an invalid file path is being passed as an `--env-file` argument, Compose returns an error:
  
当传递无效的文件路径作为 `--env-file` 参数时，Compose 会返回错误：
  
  ```console
  $ docker compose --env-file ./doesnotexist/.env.dev  config
ERROR: Couldn't find env file: /home/user/./doesnotexist/.env.dev
  ```

- You can use multiple `--env-file` options to specify multiple environment files, and Docker Compose reads them in order. Later files can override variables from earlier files.

  可以使用多个 `--env-file` 选项来指定多个环境文件，Docker Compose 会按顺序读取这些文件。后面的文件可以覆盖前面文件中的变量。

  ```console
  $ docker compose --env-file .env --env-file .env.override up
  ```

- You can override specific environment variables from the command line when starting containers.

  启动容器时，可以从命令行覆盖特定的环境变量。

  ```console
  $ docker compose --env-file .env.dev up -e DATABASE_URL=mysql://new_user:new_password@new_db:3306/new_database
  ```

### 本地 `.env` 文件与 `.env` 文件 local `.env` file versus `.env` file

An `.env` file can also be used to declare [pre-defined environment variables]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Pre-definedenvironmentvariables" >}}) used to control Compose behavior and files to be loaded.

​	`.env` 文件还可以用于声明[预定义的环境变量]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Pre-definedenvironmentvariables" >}})，这些变量用于控制 Compose 的行为和加载的文件。

When executed without an explicit `--env-file` flag, Compose searches for an `.env` file in your working directory ( [PWD](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html#index-PWD)) and loads values both for self-configuration and interpolation. If the values in this file define the `COMPOSE_FILE` pre-defined variable, which results in a project directory being set to another folder, Compose will load a second `.env` file, if present. This second `.env` file has a lower precedence.

​	在没有显式 `--env-file` 标志的情况下执行时，Compose 会在你的工作目录 ([PWD](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html#index-PWD)) 中查找 `.env` 文件，并加载其值用于自我配置和插值。如果该文件中的值定义了 `COMPOSE_FILE` 预定义变量，导致项目目录设置到其他文件夹，Compose 将加载第二个 `.env` 文件（如有）。这个第二个 `.env` 文件的优先级较低。

This mechanism makes it possible to invoke an existing Compose project with a custom set of variables as overrides, without the need to pass environment variables by the command line.

​	这种机制使得在不需要通过命令行传递环境变量的情况下，可以使用一组自定义变量覆盖现有的 Compose 项目。

```console
$ cat .env
COMPOSE_FILE=../compose.yaml
POSTGRES_VERSION=9.3

$ cat ../compose.yaml 
services:
  db:
    image: "postgres:${POSTGRES_VERSION}"
$ cat ../.env
POSTGRES_VERSION=9.2

$ docker compose config
services:
  db:
    image: "postgres:9.3"
```

### 从 Shell 中进行替代 Substitute from the shell

You can use existing environment variables from your host machine or from the shell environment where you execute `docker compose` commands. This lets you dynamically inject values into your Docker Compose configuration at runtime. For example, suppose the shell contains `POSTGRES_VERSION=9.3` and you supply the following configuration:

​	可以使用主机机器或执行 `docker compose` 命令的 Shell 环境中的现有环境变量。这可以让你在运行时动态地将值注入到 Docker Compose 配置中。例如，假设 Shell 中包含 `POSTGRES_VERSION=9.3`，并提供以下配置：



```yaml
db:
  image: "postgres:${POSTGRES_VERSION}"
```

When you run `docker compose up` with this configuration, Compose looks for the `POSTGRES_VERSION` environment variable in the shell and substitutes its value in. For this example, Compose resolves the image to `postgres:9.3` before running the configuration.

​	当你运行带有此配置的 `docker compose up` 时，Compose 会在 Shell 中查找 `POSTGRES_VERSION` 环境变量并替换其值。在此示例中，Compose 在运行配置前将镜像解析为 `postgres:9.3`。

If an environment variable is not set, Compose substitutes with an empty string. In the previous example, if `POSTGRES_VERSION` is not set, the value for the image option is `postgres:`.

​	如果环境变量未设置，Compose 会用空字符串替换。在前一个示例中，如果未设置 `POSTGRES_VERSION`，镜像选项的值为 `postgres:`。

> **Note**
>
> 
>
> `postgres:` is not a valid image reference. Docker expects either a reference without a tag, like `postgres` which defaults to the latest image, or with a tag such as `postgres:15`.
>
> ​	`postgres:` 不是有效的镜像引用。Docker 期望要么没有标签的引用（如 `postgres`，默认为最新镜像），要么使用标签如 `postgres:15`。
