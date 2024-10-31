+++
title = "设置环境变量"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Set environment variables within your container's environment  - 在容器环境中设置环境变量

A container's environment is not set until there's an explicit entry in the service configuration to make this happen. With Compose, there are two ways you can set environment variables in your containers with your Compose file.

​	容器的环境并不会自动设置，除非在服务配置中明确定义。使用 Compose，可以通过两种方式在 Compose 文件中为容器设置环境变量。

> **Tip**
>
> 
>
> Don't use environment variables to pass sensitive information, such as passwords, in to your containers. Use [secrets]({{< ref "/manuals/DockerCompose/How-tos/SecretsinCompose" >}}) instead.
>
> ​	不要使用环境变量来传递敏感信息，例如密码。请改用 [secrets]({{< ref "/manuals/DockerCompose/How-tos/SecretsinCompose" >}})。

## 使用 `environment` 属性 Use the `environment` attribute

You can set environment variables directly in your container's environment with the [`environment` attribute](https://docs.docker.com/reference/compose-file/services/#environment) in your `compose.yml`.

​	可以使用 `compose.yml` 中的 [`environment` 属性](https://docs.docker.com/reference/compose-file/services/#environment) 直接在容器环境中设置环境变量。

It supports both list and mapping syntax:

​	`environment` 支持列表和映射语法：

```yaml
services:
  webapp:
    environment:
      DEBUG: "true"
```

is equivalent to

等价于：

```yaml
services:
  webapp:
    environment:
      - DEBUG=true
```

See [`environment` attribute](https://docs.docker.com/reference/compose-file/services/#environment) for more examples on how to use it.

​	更多示例请参考 [`environment` 属性](https://docs.docker.com/reference/compose-file/services/#environment)。

### 其他信息 Additional information

- You can choose not to set a value and pass the environment variables from your shell straight through to your containers. It works in the same way as `docker run -e VARIABLE ...`:

  可以选择不设置值，将来自 Shell 的环境变量直接传递到容器中。与 `docker run -e VARIABLE ...` 类似：

  ```yaml
  web:
    environment:
      - DEBUG
  ```

The value of the `DEBUG` variable in the container is taken from the value for the same variable in the shell in which Compose is run. Note that in this case no warning is issued if the `DEBUG` variable in the shell environment is not set. 容器中的 `DEBUG` 变量的值将从运行 Compose 时 Shell 中的同名变量获取。注意，如果 Shell 中未设置 `DEBUG` 变量，则不会发出警告。

- You can also take advantage of [interpolation](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#interpolation-syntax). In the following example, the result is similar to the one above but Compose gives you a warning if the `DEBUG` variable is not set in the shell environment or in an `.env` file in the project directory.

  也可以利用[插值](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#interpolation-syntax)。在以下示例中，如果 Shell 环境或项目目录中的 `.env` 文件未设置 `DEBUG` 变量，Compose 将发出警告。

  ```yaml
  web:
    environment:
      - DEBUG=${DEBUG}
  ```

## 使用 `env_file` 属性 Use the `env_file` attribute

A container's environment can also be set using [`.env` files](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#env-file) along with the [`env_file` attribute](https://docs.docker.com/reference/compose-file/services/#env_file).

​	也可以通过 [`.env` 文件](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#env-file)和 [`env_file` 属性](https://docs.docker.com/reference/compose-file/services/#env_file) 设置容器环境：

```yaml
services:
  webapp:
    env_file: "webapp.env"
```

Using an `.env` file lets you use the same file for use by a plain `docker run --env-file ...` command, or to share the same `.env` file within multiple services without the need to duplicate a long `environment` YAML block.

​	使用 `.env` 文件可以在多个服务间共享相同的环境变量文件，而无需重复配置长的 `environment` YAML 块，也可以在 `docker run --env-file ...` 命令中使用相同的文件。

It can also help you keep your environment variables separate from your main configuration file, providing a more organized and secure way to manage sensitive information, as you do not need to place your `.env` file in the root of your project's directory.

​	此方法还可以帮助将环境变量与主配置文件分开管理，从而更有组织性和更安全，避免将 `.env` 文件放在项目根目录。

The [`env_file` attribute](https://docs.docker.com/reference/compose-file/services/#env_file) also lets you use multiple `.env` files in your Compose application.

​	[`env_file` 属性](https://docs.docker.com/reference/compose-file/services/#env_file) 还允许在 Compose 应用中使用多个 `.env` 文件。

The paths to your `.env` file, specified in the `env_file` attribute, are relative to the location of your `compose.yml` file.

​	`env_file` 属性中指定的 `.env` 文件路径相对于 `compose.yml` 文件的位置。

> **Important**
>
> 
>
> Interpolation in `.env` files is a Docker Compose CLI feature.
>
> ​	`.env` 文件中的插值是 Docker Compose CLI 的功能。
>
> It is not supported when running `docker run --env-file ...`.
>
> ​	使用 `docker run --env-file ...` 时不支持。

### 其他信息 Additional information

- If multiple files are specified, they are evaluated in order and can override values set in previous files. 

- As of Docker Compose version 2.24.0, you can set your `.env` file, defined by the `env_file` attribute, to be optional by using the `required` field. When `required` is set to `false` and the `.env` file is missing, Compose silently ignores the entry.

  从 Docker Compose 版本 2.24.0 开始，可以通过 `env_file` 属性的 `required` 字段将 `.env` 文件设为可选。如果 `required` 设置为 `false` 且 `.env` 文件缺失，Compose 将静默忽略该条目。

  ```yaml
  env_file:
    - path: ./default.env
    required: true # default
    - path: ./override.env
    required: false
  ```

- Values in your `.env` file can be overridden from the command line by using [`docker compose run -e`](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#set-environment-variables-with-docker-compose-run---env). `.env` 文件中的值可以通过命令行使用 [`docker compose run -e`](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#set-environment-variables-with-docker-compose-run---env) 覆盖。

## 使用 `docker compose run --env` 设置环境变量 Set environment variables with `docker compose run --env`

Similar to `docker run --env`, you can set environment variables temporarily with `docker compose run --env` or its short form `docker compose run -e`:

​	类似于 `docker run --env`，可以使用 `docker compose run --env` 或其简写形式 `docker compose run -e` 临时设置环境变量：

```console
$ docker compose run -e DEBUG=1 web python console.py
```

### 其他信息 Additional information

- You can also pass a variable from the shell or your environment files by not giving it a value:

  也可以通过不指定值的方式从 Shell 或环境文件传递变量：

  ```console
  $ docker compose run -e DEBUG web python console.py
  ```

The value of the `DEBUG` variable in the container is taken from the value for the same variable in the shell in which Compose is run or from the environment files.

​	容器中的 `DEBUG` 变量值将从运行 Compose 时 Shell 中的同名变量或环境文件中获取。

## 进一步资源 Further resources

- [Understand environment variable precedence 了解环境变量的优先级]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Environmentvariablesprecedence" >}}).
- [Set or change predefined environment variables 设置或更改预定义的环境变量]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Pre-definedenvironmentvariables" >}})
- [Explore best practices 了解最佳实践]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Bestpractices" >}})
- [Understand interpolation 了解插值]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Interpolation" >}})
