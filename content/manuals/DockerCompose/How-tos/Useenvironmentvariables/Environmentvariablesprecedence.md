+++
title = "环境变量优先级"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/environment-variables/envvars-precedence/](https://docs.docker.com/compose/how-tos/environment-variables/envvars-precedence/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Environment variables precedence in Docker Compose - Docker Compose 中的环境变量优先级

When the same environment variable is set in multiple sources, Docker Compose follows a precedence rule to determine the value for that variable in your container's environment.

​	当同一个环境变量在多个来源中设置时，Docker Compose 遵循优先级规则，以确定容器中该变量的最终值。

This page contains information on the level of precedence each method of setting environmental variables takes.

​	以下内容描述了每种设置环境变量的方法的优先级。

The order of precedence (highest to lowest) is as follows:

​	优先级从高到低依次为：

1. Set using [`docker compose run -e` in the CLI](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#set-environment-variables-with-docker-compose-run---env). 使用 CLI 中的 [`docker compose run -e`](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#set-environment-variables-with-docker-compose-run---env) 设置。
2. Set with either the `environment` or `env_file` attribute but with the value interpolated from your [shell](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#substitute-from-the-shell) or an environment file. (either your default [`.env` file](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#env-file), or with the [`--env-file` argument](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#substitute-with---env-file) in the CLI). 使用 `environment` 或 `env_file` 属性设置，但值来自 [Shell](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#substitute-from-the-shell) 或环境文件（默认的 [`.env` 文件](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#env-file) 或 CLI 中的 [`--env-file` 参数](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#substitute-with---env-file)）。
3. Set using just the [`environment` attribute](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#use-the-environment-attribute) in the Compose file. 仅在 Compose 文件中使用 [`environment` 属性](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#use-the-environment-attribute) 设置。
4. Use of the [`env_file` attribute](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#use-the-env_file-attribute) in the Compose file. 使用 Compose 文件中的 [`env_file` 属性](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#use-the-env_file-attribute)。
5. Set in a container image in the [ENV directive](https://docs.docker.com/reference/dockerfile/#env). Having any `ARG` or `ENV` setting in a `Dockerfile` evaluates only if there is no Docker Compose entry for `environment`, `env_file` or `run --env`. 在镜像中使用 [ENV 指令](https://docs.docker.com/reference/dockerfile/#env) 设置。Dockerfile 中的 `ARG` 或 `ENV` 设置仅在 Docker Compose 中没有为 `environment`、`env_file` 或 `run --env` 设置时生效。

## 简单示例 Simple example

In the following example, a different value for the same environment variable in an `.env` file and with the `environment` attribute in the Compose file:

​	在以下示例中，相同的环境变量在 `.env` 文件中和 Compose 文件中的 `environment` 属性中定义了不同的值：

```console
$ cat ./webapp.env
NODE_ENV=test

$ cat compose.yml
services:
  webapp:
    image: 'webapp'
    env_file:
     - ./webapp.env
    environment:
     - NODE_ENV=production
```

The environment variable defined with the `environment` attribute takes precedence.

​	`environment` 属性定义的环境变量优先级更高。

```console
$ docker compose run webapp env | grep NODE_ENV
NODE_ENV=production
```

## 进阶示例 Advanced example

The following table uses `VALUE`, an environment variable defining the version for an image, as an example.

​	以下表格使用环境变量 `VALUE` 来定义镜像的版本作为示例。

### How the table works

Each column represents a context from where you can set a value, or substitute in a value for `VALUE`.

​	每列代表了一个可以设置 `VALUE` 或替换 `VALUE` 值的上下文。

The columns `Host OS environment` and `.env` file is listed only for illustration purposes. In reality, they don't result in a variable in the container by itself, but in conjunction with either the `environment` or `env_file` attribute.

​	`Host OS environment` 和 `.env` 文件的列仅作说明之用。实际上，它们本身并不会直接在容器中设置变量，而是与 `environment` 或 `env_file` 属性配合使用。

Each row represents a combination of contexts where `VALUE` is set, substituted, or both. The **Result** column indicates the final value for `VALUE` in each scenario.

​	每一行表示不同的上下文组合，并展示了 `VALUE` 在每种场景中的最终结果。

|  #   | `docker compose run` | `environment` attribute | `env_file` attribute | Image `ENV` | `Host OS` environment | `.env` file |      |   Result 结果   |
| :--: | :------------------: | :---------------------: | :------------------: | :---------: | :-------------------: | :---------: | :--: | :-------------: |
|  1   |          -           |            -            |          -           |      -      |      `VALUE=1.4`      | `VALUE=1.3` |      |        -        |
|  2   |          -           |            -            |     `VALUE=1.6`      | `VALUE=1.5` |      `VALUE=1.4`      |      -      |      | **`VALUE=1.6`** |
|  3   |          -           |       `VALUE=1.7`       |          -           | `VALUE=1.5` |      `VALUE=1.4`      |      -      |      | **`VALUE=1.7`** |
|  4   |          -           |            -            |          -           | `VALUE=1.5` |      `VALUE=1.4`      | `VALUE=1.3` |      | **`VALUE=1.5`** |
|  5   |  `--env VALUE=1.8`   |            -            |          -           | `VALUE=1.5` |      `VALUE=1.4`      | `VALUE=1.3` |      | **`VALUE=1.8`** |
|  6   |    `--env VALUE`     |            -            |          -           | `VALUE=1.5` |      `VALUE=1.4`      | `VALUE=1.3` |      | **`VALUE=1.4`** |
|  7   |    `--env VALUE`     |            -            |          -           | `VALUE=1.5` |           -           | `VALUE=1.3` |      | **`VALUE=1.3`** |
|  8   |          -           |            -            |       `VALUE`        | `VALUE=1.5` |      `VALUE=1.4`      | `VALUE=1.3` |      | **`VALUE=1.4`** |
|  9   |          -           |            -            |       `VALUE`        | `VALUE=1.5` |           -           | `VALUE=1.3` |      | **`VALUE=1.3`** |
|  10  |          -           |         `VALUE`         |          -           | `VALUE=1.5` |      `VALUE=1.4`      | `VALUE=1.3` |      | **`VALUE=1.4`** |
|  11  |          -           |         `VALUE`         |          -           | `VALUE=1.5` |           -           | `VALUE=1.3` |      | **`VALUE=1.3`** |
|  12  |    `--env VALUE`     |       `VALUE=1.7`       |          -           | `VALUE=1.5` |      `VALUE=1.4`      | `VALUE=1.3` |      | **`VALUE=1.4`** |
|  13  |  `--env VALUE=1.8`   |       `VALUE=1.7`       |          -           | `VALUE=1.5` |      `VALUE=1.4`      | `VALUE=1.3` |      | **`VALUE=1.8`** |
|  14  |  `--env VALUE=1.8`   |            -            |     `VALUE=1.6`      | `VALUE=1.5` |      `VALUE=1.4`      | `VALUE=1.3` |      | **`VALUE=1.8`** |
|  15  |  `--env VALUE=1.8`   |       `VALUE=1.7`       |     `VALUE=1.6`      | `VALUE=1.5` |      `VALUE=1.4`      | `VALUE=1.3` |      | **`VALUE=1.8`** |

### 结果说明 Result explanation

Result 1: The local environment takes precedence, but the Compose file is not set to replicate this inside the container, so no such variable is set.

​	结果 1：本地环境优先，但 Compose 文件未设置复制，故该变量未在容器中设置。

Result 2: The `env_file` attribute in the Compose file defines an explicit value for `VALUE` so the container environment is set accordingly.

​	结果 2：Compose 文件中 `env_file` 属性定义了 `VALUE`，因此在容器环境中设置相应值。

Result 3: The `environment` attribute in the Compose file defines an explicit value for `VALUE`, so the container environment is set accordingly/

​	结果 3：Compose 文件中 `environment` 属性定义了 `VALUE`，因此在容器环境中设置相应值。

Result 4: The image's `ENV` directive declares the variable `VALUE`, and since the Compose file is not set to override this value, this variable is defined by image

​	结果 4：镜像的 `ENV` 指令声明了变量 `VALUE`，Compose 文件未设置覆盖，因此该变量定义于镜像中。

Result 5: The `docker compose run` command has the `--env` flag set which an explicit value, and overrides the value set by the image.

​	结果 5：`docker compose run` 命令中的 `--env` 标志设置了明确值，覆盖了镜像设置的值。

Result 6: The `docker compose run` command has the `--env` flag set to replicate the value from the environment. Host OS value takes precedence and is replicated into the container's environment.

​	结果 6：`docker compose run` 中 `--env` 标志设置了复制值。主机 OS 值优先并被复制到容器环境。

Result 7: The `docker compose run` command has the `--env` flag set to replicate the value from the environment. Value from `.env` file is the selected to define the container's environment.

​	结果 7：`docker compose run` 中 `--env` 标志设置了复制值。`.env` 文件的值被选定为定义容器环境。

Result 8: The `env_file` attribute in the Compose file is set to replicate `VALUE` from the local environment. Host OS value takes precedence and is replicated into the container's environment.

​	结果 8：Compose 文件中 `env_file` 属性设置了从本地环境复制 `VALUE`。主机 OS 值优先并被复制到容器环境。

Result 9: The `env_file` attribute in the Compose file is set to replicate `VALUE` from the local environment. Value from `.env` file is the selected to define the container's environment.

​	结果 9：Compose 文件中 `env_file` 属性设置了从本地环境复制 `VALUE`。`.env` 文件的值被选定为定义容器环境。

Result 10: The `environment` attribute in the Compose file is set to replicate `VALUE` from the local environment. Host OS value takes precedence and is replicated into the container's environment.

​	结果 10：Compose 文件中 `environment` 属性设置了从本地环境复制 `VALUE`。主机 OS 值优先并被复制到容器环境。

Result 11: The `environment` attribute in the Compose file is set to replicate `VALUE` from the local environment. Value from `.env` file is the selected to define the container's environment.

​	结果 11：Compose 文件中 `environment` 属性设置了从本地环境复制 `VALUE`。`.env` 文件的值被选定为定义容器环境。

Result 12: The `--env` flag has higher precedence than the `environment` and `env_file` attributes and is to set to replicate `VALUE` from the local environment. Host OS value takes precedence and is replicated into the container's environment.

​	结果 12：`--env` 标志优先于 `environment` 和 `env_file` 属性，设置为从本地环境复制 `VALUE`。主机 OS 值优先并被复制到容器环境。

Results 13 to 15: The `--env` flag has higher precedence than the `environment` and `env_file` attributes and so sets the value.

​	结果 13 到 15：`--env` 标志优先于 `environment` 和 `env_file` 属性并设置了值。
