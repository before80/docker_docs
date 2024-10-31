+++
title = "Merge"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/](https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Merge Compose files - 合并 Compose 文件

Docker Compose lets you merge and override a set of Compose files together to create a composite Compose file.

​	Docker Compose 允许将一组 Compose 文件合并和覆盖在一起以创建复合的 Compose 文件。

By default, Compose reads two files, a `compose.yml` and an optional `compose.override.yml` file. By convention, the `compose.yml` contains your base configuration. The override file can contain configuration overrides for existing services or entirely new services.

​	默认情况下，Compose 读取两个文件，一个 `compose.yml` 文件和一个可选的 `compose.override.yml` 文件。通常情况下，`compose.yml` 包含基本配置，而覆盖文件可以包含现有服务的配置覆盖或完全新的服务。

If a service is defined in both files, Compose merges the configurations using the rules described below and in the [Compose Specification]({{< ref "/reference/Composefilereference/Merge" >}}).

​	如果服务在两个文件中都定义，Compose 会使用下述规则和 [Compose 规范]({{< ref "/reference/Composefilereference/Merge" >}}) 中描述的规则合并配置。

## 如何合并多个 Compose 文件 How to merge multiple Compose files

To use multiple override files, or an override file with a different name, you can either use the pre-defined [COMPOSE_FILE](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_file) environment variable, or use the `-f` option to specify the list of files.

​	要使用多个覆盖文件或具有不同名称的覆盖文件，可以使用预定义的 [COMPOSE_FILE](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_file) 环境变量，或使用 `-f` 选项指定文件列表。

Compose merges files in the order they're specified on the command line. Subsequent files may merge, override, or add to their predecessors.

​	Compose 按照命令行中指定的顺序合并文件。后续文件可以合并、覆盖或添加到其前面的文件中。

For example:



```console
$ docker compose -f compose.yml -f compose.admin.yml run backup_db
```

The `compose.yml` file might specify a `webapp` service.

​	`compose.yml` 文件可能指定了一个 `webapp` 服务：



```yaml
webapp:
  image: examples/web
  ports:
    - "8000:8000"
  volumes:
    - "/data"
```

The `compose.admin.yml` may also specify this same service:

​	`compose.admin.yml` 文件也可能指定了相同的服务：



```yaml
webapp:
  environment:
    - DEBUG=1
```

Any matching fields override the previous file. New values, add to the `webapp` service configuration:

​	任何匹配的字段都会覆盖前一个文件的内容。新值将添加到 `webapp` 服务配置中：



```yaml
webapp:
  image: examples/web
  ports:
    - "8000:8000"
  volumes:
    - "/data"
  environment:
    - DEBUG=1
    - ANOTHER_VARIABLE=value
```

> **Important**
>
> 
>
> When you use multiple Compose files, you must make sure all paths in the files are relative to the base Compose file (the first Compose file specified with `-f`). This is required because override files need not be valid Compose files. Override files can contain small fragments of configuration. Tracking which fragment of a service is relative to which path is difficult and confusing, so to keep paths easier to understand, all paths must be defined relative to the base file.
>
> ​	使用多个 Compose 文件时，必须确保文件中的所有路径都相对于基本 Compose 文件（即使用 `-f` 指定的第一个 Compose 文件）。这是因为覆盖文件不必是有效的 Compose 文件。覆盖文件可以包含配置的小片段。为了简化路径定义，所有路径必须相对于基础文件。

### 其他信息 Additional information

- Using `-f` is optional. If not provided, Compose searches the working directory and its parent directories for a `compose.yml` and a `compose.override.yml` file. You must supply at least the `compose.yml` file. If both files exist on the same directory level, Compose combines them into a single configuration.

  - 使用 `-f` 是可选的。如果未提供，Compose 会在工作目录及其父目录中搜索 `compose.yml` 和 `compose.override.yml` 文件。必须至少提供 `compose.yml` 文件。如果两个文件位于相同的目录级别，Compose 会将它们合并为单个配置。

- When you use multiple Compose files, all paths in the files are relative to the first configuration file specified with `-f`. You can use the `--project-directory` option to override this base path.

  - 使用多个 Compose 文件时，文件中的所有路径都相对于 `-f` 指定的第一个配置文件。可以使用 `--project-directory` 选项覆盖此基础路径。

- You can use a `-f` with `-` (dash) as the filename to read the configuration from `stdin`. For example:

  可以使用 `-f` 和 `-`（短划线）作为文件名，从 `stdin` 读取配置。例如：

  ```console
  $ docker compose -f - <<EOF
    webapp:
      image: examples/web
      ports:
       - "8000:8000"
      volumes:
       - "/data"
      environment:
       - DEBUG=1
    EOF
  ```

  When `stdin` is used, all paths in the configuration are relative to the current working directory.

  ​	当使用 `stdin` 时，配置中的所有路径均相对于当前工作目录。

- You can use the `-f` flag to specify a path to a Compose file that is not located in the current directory, either from the command line or by setting up a [COMPOSE_FILE environment variable](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_file) in your shell or in an environment file. 可以通过 `-f` 标志指定不在当前目录中的 Compose 文件路径，或通过在 Shell 或环境文件中设置 [COMPOSE_FILE 环境变量](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_file)。

  For example, if you are running the [Compose Rails sample](https://github.com/docker/awesome-compose/tree/master/official-documentation-samples/rails/README.md), and have a `compose.yml` file in a directory called `sandbox/rails`. You can use a command like [docker compose pull]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposepull" >}}) to get the postgres image for the `db` service from anywhere by using the `-f` flag as follows: `docker compose -f ~/sandbox/rails/compose.yml pull db`

  ​	示例：如果在 `sandbox/rails` 目录中有一个名为 `compose.yml` 的文件，可以使用如下命令获取 `db` 服务的 postgres 镜像：`docker compose -f ~/sandbox/rails/compose.yml pull db`

  Here's the full example:

  ​	完整示例：

  ```console
  $ docker compose -f ~/sandbox/rails/compose.yml pull db
  Pulling db (postgres:latest)...
  latest: Pulling from library/postgres
  ef0380f84d05: Pull complete
  50cf91dc1db8: Pull complete
  d3add4cd115c: Pull complete
  467830d8a616: Pull complete
  089b9db7dc57: Pull complete
  6fba0a36935c: Pull complete
  81ef0e73c953: Pull complete
  338a6c4894dc: Pull complete
  15853f32f67c: Pull complete
  044c83d92898: Pull complete
  17301519f133: Pull complete
  dcca70822752: Pull complete
  cecf11b8ccf3: Pull complete
  Digest: sha256:1364924c753d5ff7e2260cd34dc4ba05ebd40ee8193391220be0f9901d4e1651
  Status: Downloaded newer image for postgres:latest
  ```

## 合并规则 Merging rules

Compose copies configurations from the original service over to the local one. If a configuration option is defined in both the original service and the local service, the local value replaces or extends the original value.

​	Compose 将原始服务中的配置复制到本地服务中。如果配置选项在原始服务和本地服务中均定义，则本地值会替换或扩展原始值。

For single-value options like `image`, `command` or `mem_limit`, the new value replaces the old value.

​	对于 `image`、`command` 或 `mem_limit` 等单值选项，新值会替换旧值。

original service:

​	原始服务：

```yaml
services:
  myservice:
    # ...
    command: python app.py
```

local service:

​	本地服务：

```yaml
services:
  myservice:
    # ...
    command: python otherapp.py
```

result:

​	结果：

```yaml
services:
  myservice:
    # ...
    command: python otherapp.py
```

For the multi-value options `ports`, `expose`, `external_links`, `dns`, `dns_search`, and `tmpfs`, Compose concatenates both sets of values:

​	对于多值选项 `ports`、`expose`、`external_links`、`dns`、`dns_search` 和 `tmpfs`，Compose 会连接两组值：

original service:

​	原始服务：

```yaml
services:
  myservice:
    # ...
    expose:
      - "3000"
```

local service:

​	本地服务：

```yaml
services:
  myservice:
    # ...
    expose:
      - "4000"
      - "5000"
```

result:

​	结果：

```yaml
services:
  myservice:
    # ...
    expose:
      - "3000"
      - "4000"
      - "5000"
```

In the case of `environment`, `labels`, `volumes`, and `devices`, Compose "merges" entries together with locally defined values taking precedence. For `environment` and `labels`, the environment variable or label name determines which value is used:

​	对于 `environment`、`labels`、`volumes` 和 `devices`，Compose 会“合并”条目，本地定义的值优先。对于 `environment` 和 `labels`，环境变量或标签名称决定了哪个值被使用：

original service:

​	原始服务：

```yaml
services:
  myservice:
    # ...
    environment:
      - FOO=original
      - BAR=original
```

local service:

​	本地服务：

```yaml
services:
  myservice:
    # ...
    environment:
      - BAR=local
      - BAZ=local
```

result:

​	结果：

```yaml
services:
  myservice:
    # ...
    environment:
      - FOO=original
      - BAR=local
      - BAZ=local
```

Entries for `volumes` and `devices` are merged using the mount path in the container:

​	对于 `volumes` 和 `devices`，Compose 使用容器内的挂载路径进行合并：

original service:

​	原始服务：

```yaml
services:
  myservice:
    # ...
    volumes:
      - ./original:/foo
      - ./original:/bar
```

local service:

​	本地服务：

```yaml
services:
  myservice:
    # ...
    volumes:
      - ./local:/bar
      - ./local:/baz
```

result:

​	结果：

```yaml
services:
  myservice:
    # ...
    volumes:
      - ./original:/foo
      - ./local:/bar
      - ./local:/baz
```

For more merging rules, see [Merge and override]({{< ref "/reference/Composefilereference/Merge" >}}) in the Compose Specification.

​	有关更多合并规则，请参见 Compose 规范中的 [合并和覆盖]({{< ref "/reference/Composefilereference/Merge" >}})。

## Example

A common use case for multiple files is changing a development Compose app for a production-like environment (which may be production, staging or CI). To support these differences, you can split your Compose configuration into a few different files:

​	多个文件的常见用例是将开发 Compose 应用更改为类似生产的环境（例如生产、预发布或 CI）。为支持这些差异，可以将 Compose 配置拆分为几个不同的文件：

Start with a base file that defines the canonical configuration for the services.

​	首先，定义一个基本文件来确定服务的规范配置。

`compose.yml`

```yaml
services:
  web:
    image: example/my_web_app:latest
    depends_on:
      - db
      - cache

  db:
    image: postgres:latest

  cache:
    image: redis:latest
```

In this example the development configuration exposes some ports to the host, mounts our code as a volume, and builds the web image.

​	在此示例中，开发配置将一些端口暴露给主机，将代码作为卷挂载，并构建 web 镜像。

`compose.override.yml`

```yaml
services:
  web:
    build: .
    volumes:
      - '.:/code'
    ports:
      - 8883:80
    environment:
      DEBUG: 'true'

  db:
    command: '-d'
    ports:
     - 5432:5432

  cache:
    ports:
      - 6379:6379
```

When you run `docker compose up` it reads the overrides automatically.

​	当运行 `docker compose up` 时，它会自动读取覆盖配置。

To use this Compose app in a production environment, another override file is created, which might be stored in a different git repo or managed by a different team.

​	在生产环境中使用此 Compose 应用时，可以创建另一个覆盖文件，该文件可能存储在不同的 git 仓库中或由不同的团队管理。

`compose.prod.yml`

```yaml
services:
  web:
    ports:
      - 80:80
    environment:
      PRODUCTION: 'true'

  cache:
    environment:
      TTL: '500'
```

To deploy with this production Compose file you can run

​	要使用此生产 Compose 文件部署，可以运行



```console
$ docker compose -f compose.yml -f compose.prod.yml up -d
```

This deploys all three services using the configuration in `compose.yml` and `compose.prod.yml` but not the dev configuration in `compose.override.yml`.

​	这会使用 `compose.yml` 和 `compose.prod.yml` 中的配置来部署所有三个服务，而不会包含开发配置中的内容 `compose.override.yml`。

For more information, see [Using Compose in production]({{< ref "/manuals/DockerCompose/How-tos/UseComposeinproduction" >}}).

​	有关更多信息，请参见 [在生产中使用 Compose]({{< ref "/manuals/DockerCompose/How-tos/UseComposeinproduction" >}})。

## 限制 Limitations

Docker Compose supports relative paths for the many resources to be included in the application model: build context for service images, location of file defining environment variables, path to a local directory used in a bind-mounted volume. With such a constraint, code organization in a monorepo can become hard as a natural choice would be to have dedicated folders per team or component, but then the Compose files relative paths become irrelevant.

​	Docker Compose 支持应用模型中包含的多个资源的相对路径：服务镜像的构建上下文、定义环境变量的文件位置、本地目录路径（用于绑定挂载卷）。在这样的限制下，在 monorepo 中进行代码组织可能会变得困难，因为一个自然的选择是为每个团队或组件提供专用文件夹，但这样一来 Compose 文件的相对路径就会变得不再相关。

## 参考信息 Reference information

- [Merge rules 合并规则]({{< ref "/reference/Composefilereference/Merge" >}})
