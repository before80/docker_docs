+++
title = "在 Compose 中使用配置文件（Profiles）"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/profiles/](https://docs.docker.com/compose/how-tos/profiles/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Using profiles with Compose - 在 Compose 中使用配置文件（Profiles）

Profiles help you adjust your Compose application for different environments or use cases by selectively activating services. Services can be assigned to one or more profiles; unassigned services start by default, while assigned ones only start when their profile is active. This setup means specific services, like those for debugging or development, to be included in a single `compose.yml` file and activated only as needed.

​	配置文件帮助您通过有选择地激活服务来调整 Compose 应用，以适应不同的环境或使用场景。服务可以被分配到一个或多个配置文件；未分配的服务默认启动，而已分配的服务仅在其配置文件激活时启动。这种设置允许将调试或开发等特定服务包含在单个 `compose.yml` 文件中，并根据需要激活它们。

## 为服务分配配置文件 Assigning profiles to services

Services are associated with profiles through the [`profiles` attribute](https://docs.docker.com/reference/compose-file/services/#profiles) which takes an array of profile names:

​	服务通过 [`profiles` 属性](https://docs.docker.com/reference/compose-file/services/#profiles) 与配置文件关联，该属性接受一个配置文件名称数组：



```yaml
services:
  frontend:
    image: frontend
    profiles: [frontend]

  phpmyadmin:
    image: phpmyadmin
    depends_on: [db]
    profiles: [debug]

  backend:
    image: backend

  db:
    image: mysql
```

Here the services `frontend` and `phpmyadmin` are assigned to the profiles `frontend` and `debug` respectively and as such are only started when their respective profiles are enabled.

​	在此示例中，服务 `frontend` 和 `phpmyadmin` 分别分配到配置文件 `frontend` 和 `debug`，因此它们仅在各自的配置文件启用时才会启动。

Services without a `profiles` attribute are always enabled. In this case running `docker compose up` would only start `backend` and `db`.

​	没有 `profiles` 属性的服务始终启用。在这种情况下，运行 `docker compose up` 只会启动 `backend` 和 `db`。

Valid profiles names follow the regex format of `[a-zA-Z0-9][a-zA-Z0-9_.-]+`.

​	有效的配置文件名称遵循正则表达式格式 `[a-zA-Z0-9][a-zA-Z0-9_.-]+`。

> **Tip**
>
> 
>
> The core services of your application shouldn't be assigned `profiles` so they are always enabled and automatically started.
>
> ​	应用的核心服务不应分配 `profiles`，这样它们始终启用并自动启动。

## 启动特定的配置文件 Start specific profiles

To start a specific profile supply the `--profile` [command-line option]({{< ref "/reference/CLIreference/docker/dockercompose" >}}) or use the [`COMPOSE_PROFILES` environment variable](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_profiles):

​	要启动特定的配置文件，可以使用 `--profile` 命令行选项或 [`COMPOSE_PROFILES` 环境变量](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_profiles)：

```console
$ docker compose --profile debug up
```



```console
$ COMPOSE_PROFILES=debug docker compose up
```

Both commands start the services with the `debug` profile enabled. In the previous `compose.yml` file, this starts the services `db`, `backend` and `phpmyadmin`.

​	这两个命令启动 `debug` 配置文件启用的服务。在上面的 `compose.yml` 文件中，这会启动服务 `db`、`backend` 和 `phpmyadmin`。

### 启动多个配置文件 Start multiple profiles

You can also enable multiple profiles, e.g. with `docker compose --profile frontend --profile debug up` the profiles `frontend` and `debug` will be enabled.

​	您还可以启用多个配置文件，例如使用 `docker compose --profile frontend --profile debug up` 启用 `frontend` 和 `debug` 配置文件。

Multiple profiles can be specified by passing multiple `--profile` flags or a comma-separated list for the `COMPOSE_PROFILES` environment variable:

​	可以通过传递多个 `--profile` 标志或使用逗号分隔列表为 `COMPOSE_PROFILES` 环境变量指定多个配置文件：

xxxxxxxxxx5 1services:2  app:3    image: backend4    pre_stop:5      - command: ./data_flush.shyaml

```console
$ docker compose --profile frontend --profile debug up
```



```console
$ COMPOSE_PROFILES=frontend,debug docker compose up
```

If you want to enable all profiles at the same time, you can run `docker compose --profile "*"`.

​	如果想要同时启用所有配置文件，可以运行 `docker compose --profile "*"`。

## 自动启动配置文件和依赖解析 Auto-starting profiles and dependency resolution

When a service with assigned `profiles` is explicitly targeted on the command line its profiles are started automatically so you don't need to start them manually. This can be used for one-off services and debugging tools. As an example consider the following configuration:

​	当命令行中显式指定的服务分配了 `profiles` 时，其配置文件会自动启动，因此您无需手动启动它们。这可用于一次性服务和调试工具。以下是一个示例配置：



```yaml
services:
  backend:
    image: backend

  db:
    image: mysql

  db-migrations:
    image: backend
    command: myapp migrate
    depends_on:
      - db
    profiles:
      - tools
```



```sh
# Only start backend and db
# 仅启动 backend 和 db
$ docker compose up -d

# This runs db-migrations (and,if necessary, start db)
# by implicitly enabling the profiles `tools`
# 运行 db-migrations（如果需要，启动 db）
# 通过隐式启用 `tools` 配置文件
$ docker compose run db-migrations
```

But keep in mind that `docker compose` only automatically starts the profiles of the services on the command line and not of any dependencies.

​	但请记住，`docker compose` 仅自动启动命令行中指定服务的配置文件，而不会自动启动任何依赖服务的配置文件。

This means that any other services the targeted service `depends_on` should either:

​	这意味着被目标服务 `depends_on` 的任何其他服务应满足以下条件之一：

- Share a common profile
  - 共享一个公共配置文件

- Always be started, by omitting `profiles` or having a matching profile started explicitly
  - 始终启动，方法是省略 `profiles` 或显式启用匹配的配置文件




```yaml
services:
  web:
    image: web

  mock-backend:
    image: backend
    profiles: ["dev"]
    depends_on:
      - db

  db:
    image: mysql
    profiles: ["dev"]

  phpmyadmin:
    image: phpmyadmin
    profiles: ["debug"]
    depends_on:
      - db
```



```sh
# Only start "web"
$ docker compose up -d

# Start mock-backend (and, if necessary, db)
# by implicitly enabling profiles `dev`
# 启动 mock-backend（如果需要，启动 db）
# 通过隐式启用 `dev` 配置文件
$ docker compose up -d mock-backend

# This fails because profiles "dev" is not enabled
# 此操作失败，因为配置文件 "dev" 未启用
$ docker compose up phpmyadmin
```

Although targeting `phpmyadmin` automatically starts the profiles `debug`, it doesn't automatically start the profiles required by `db` which is `dev`.

​	尽管针对 `phpmyadmin` 会自动启动 `debug` 配置文件，但不会自动启动 `db` 所需的 `dev` 配置文件。

To fix this you either have to add the `debug` profile to the `db` service:

​	为解决此问题，可以将 `debug` 配置文件添加到 `db` 服务中：

```yaml
db:
  image: mysql
  profiles: ["debug", "dev"]
```

or start the `dev` profile explicitly:

​	或者显式启动 `dev` 配置文件：

```console
# Profiles "debug" is started automatically by targeting phpmyadmin
# 通过指定 phpmyadmin 自动启动 "debug" 配置文件
$ docker compose --profile dev up phpmyadmin
$ COMPOSE_PROFILES=dev docker compose up phpmyadmin
```

## 停止特定的配置文件 Stop specific profiles

As with starting specific profiles, you can use the `--profile` [command-line option](https://docs.docker.com/reference/cli/docker/compose/#use--p-to-specify-a-project-name) or use the [`COMPOSE_PROFILES` environment variable](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_profiles):

​	与启动特定配置文件类似，可以使用 `--profile` [命令行选项](https://docs.docker.com/reference/cli/docker/compose/#use--p-to-specify-a-project-name) 或使用 [`COMPOSE_PROFILES` 环境变量](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_profiles)：



```console
$ docker compose --profile debug down
```



```console
$ COMPOSE_PROFILES=debug docker compose down
```

Both commands stop and remove services with the `debug` profile. In the following `compose.yml` file, this stops the services `db` and `phpmyadmin`.

​	这两个命令停止并移除 `debug` 配置文件的服务。在下面的 `compose.yml` 文件中，这会停止服务 `db` 和 `phpmyadmin`。



```yaml
services:
  frontend:
    image: frontend
    profiles: [frontend]

  phpmyadmin:
    image: phpmyadmin
    depends_on: [db]
    profiles: [debug]

  backend:
    image: backend

  db:
    image: mysql
```

> **Note**
>
> 
>
> Running `docker compose down` only stops `backend` and `db`.
>
> ​	运行 `docker compose down` 仅会停止 `backend` 和 `db`。

## 参考信息 Reference information

[`profiles`](https://docs.docker.com/reference/compose-file/services/#profiles)
