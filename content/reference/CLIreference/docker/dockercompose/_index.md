+++
title = "docker compose"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/](https://docs.docker.com/reference/cli/docker/compose/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose

| Description | Docker Compose   |
| :---------- | ---------------- |
| Usage       | `docker compose` |

## Description

You can use the compose subcommand, `docker compose [-f <arg>...] [options] [COMMAND] [ARGS...]`, to build and manage multiple services in Docker containers.

​	你可以使用 compose 子命令 `docker compose [-f <arg>...] [options] [COMMAND] [ARGS...]` 来构建和管理多个 Docker 容器中的服务。

### 使用 `-f` 指定一个或多个 Compose 文件的名称和路径 Use `-f` to specify the name and path of one or more Compose files

Use the `-f` flag to specify the location of a Compose configuration file.

​	使用 `-f` 标志来指定 Compose 配置文件的位置。

#### 指定多个 Compose 文件 Specifying multiple Compose files

You can supply multiple `-f` configuration files. When you supply multiple files, Compose combines them into a single configuration. Compose builds the configuration in the order you supply the files. Subsequent files override and add to their predecessors.

​	你可以提供多个 `-f` 配置文件。当你提供多个文件时，Compose 会将它们合并为一个单一的配置。Compose 按照你提供文件的顺序构建配置。后续文件会覆盖并添加到前面的文件。

For example, consider this command line:

​	例如，考虑以下命令行：

```console
$ docker compose -f docker-compose.yml -f docker-compose.admin.yml run backup_db
```

The `docker-compose.yml` file might specify a `webapp` service.

​	`docker-compose.yml` 文件可能会指定一个 `webapp` 服务：

```yaml
services:
  webapp:
    image: examples/web
    ports:
      - "8000:8000"
    volumes:
      - "/data"
```

If the `docker-compose.admin.yml` also specifies this same service, any matching fields override the previous file. New values, add to the `webapp` service configuration.

​	如果 `docker-compose.admin.yml` 也指定了同样的服务，那么任何匹配的字段都会覆盖前面的文件。新值会添加到 `webapp` 服务的配置中：

```yaml
services:
  webapp:
    build: .
    environment:
      - DEBUG=1
```

When you use multiple Compose files, all paths in the files are relative to the first configuration file specified with `-f`. You can use the `--project-directory` option to override this base path.

​	当你使用多个 Compose 文件时，所有文件中的路径都相对于第一个使用 `-f` 指定的配置文件。你可以使用 `--project-directory` 选项来覆盖这个基本路径。

Use a `-f` with `-` (dash) as the filename to read the configuration from stdin. When stdin is used all paths in the configuration are relative to the current working directory.

​	使用 `-f` 和 `-`（短横线）作为文件名可以从标准输入读取配置。当使用标准输入时，所有配置中的路径都相对于当前工作目录。

The `-f` flag is optional. If you don’t provide this flag on the command line, Compose traverses the working directory and its parent directories looking for a `compose.yaml` or `docker-compose.yaml` file.

​	`-f` 标志是可选的。如果你不在命令行中提供这个标志，Compose 会遍历工作目录及其父目录，寻找 `compose.yaml` 或 `docker-compose.yaml` 文件。

#### 指定单个 Compose 文件的路径 Specifying a path to a single Compose file

You can use the `-f` flag to specify a path to a Compose file that is not located in the current directory, either from the command line or by setting up a `COMPOSE_FILE` environment variable in your shell or in an environment file.

​	你可以使用 `-f` 标志指定一个不在当前目录中的 Compose 文件的路径，无论是通过命令行还是通过在你的 shell 或环境文件中设置 `COMPOSE_FILE` 环境变量。

For an example of using the `-f` option at the command line, suppose you are running the Compose Rails sample, and have a `compose.yaml` file in a directory called `sandbox/rails`. You can use a command like `docker compose pull` to get the postgres image for the db service from anywhere by using the `-f` flag as follows:

​	例如，假设你正在运行 Compose Rails 示例，并且在一个名为 `sandbox/rails` 的目录中有一个 `compose.yaml` 文件。你可以使用类似 `docker compose pull` 的命令来从任何地方获取 db 服务的 postgres 镜像，方法如下：

```console
$ docker compose -f ~/sandbox/rails/compose.yaml pull db
```

### 使用 `-p` 指定项目名称 Use `-p` to specify a project name

Each configuration has a project name. Compose sets the project name using the following mechanisms, in order of precedence:

​	每个配置都有一个项目名称。Compose 使用以下机制按优先顺序设置项目名称：

- The `-p` command line flag
  - `-p` 命令行标志

- The `COMPOSE_PROJECT_NAME` environment variable
  - `COMPOSE_PROJECT_NAME` 环境变量

- The top level `name:` variable from the config file (or the last `name:` from a series of config files specified using `-f`)
  - 配置文件中的顶级 `name:` 变量（或使用 `-f` 指定的一系列配置文件中的最后一个 `name:`）

- The `basename` of the project directory containing the config file (or containing the first config file specified using `-f`)
  - 包含配置文件的项目目录的 `basename`（或包含第一个使用 `-f` 指定的配置文件的目录）

- The `basename` of the current directory if no config file is specified Project names must contain only lowercase letters, decimal digits, dashes, and underscores, and must begin with a lowercase letter or decimal digit. If the `basename` of the project directory or current directory violates this constraint, you must use one of the other mechanisms.
  - 如果未指定配置文件，则使用当前目录的 `basename` 项目名称必须仅包含小写字母、十进制数字、短横线和下划线，并且必须以小写字母或十进制数字开头。如果项目目录或当前目录的 `basename` 违反了这个约束，你必须使用其他机制。




```console
$ docker compose -p my_project ps -a
NAME                 SERVICE    STATUS     PORTS
my_project_demo_1    demo       running

$ docker compose -p my_project logs
demo_1  | PING localhost (127.0.0.1): 56 data bytes
demo_1  | 64 bytes from 127.0.0.1: seq=0 ttl=64 time=0.095 ms
```

### 使用配置文件启用可选服务 Use profiles to enable optional services

Use `--profile` to specify one or more active profiles Calling `docker compose --profile frontend up` starts the services with the profile `frontend` and services without any specified profiles. You can also enable multiple profiles, e.g. with `docker compose --profile frontend --profile debug up` the profiles `frontend` and `debug` is enabled.

​	使用 `--profile` 指定一个或多个活动配置文件。调用 `docker compose --profile frontend up` 启动带有 `frontend` 配置文件的服务以及没有指定配置文件的服务。你也可以启用多个配置文件，例如使用 `docker compose --profile frontend --profile debug up` 启用 `frontend` 和 `debug` 配置文件。

Profiles can also be set by `COMPOSE_PROFILES` environment variable.

​	配置文件也可以通过 `COMPOSE_PROFILES` 环境变量设置。

### 配置并行性 Configuring parallelism

Use `--parallel` to specify the maximum level of parallelism for concurrent engine calls. Calling `docker compose --parallel 1 pull` pulls the pullable images defined in the Compose file one at a time. This can also be used to control build concurrency.

​	使用 `--parallel` 指定并发引擎调用的最大并行级别。调用 `docker compose --parallel 1 pull` 按顺序拉取 Compose 文件中定义的可拉取镜像。这也可以用于控制构建并发性。

Parallelism can also be set by the `COMPOSE_PARALLEL_LIMIT` environment variable.

​	并行性也可以通过 `COMPOSE_PARALLEL_LIMIT` 环境变量设置。

### 设置环境变量 Set up environment variables

You can set environment variables for various docker compose options, including the `-f`, `-p` and `--profiles` flags.

​	你可以为各种 docker compose 选项设置环境变量，包括 `-f`、`-p` 和 `--profiles` 标志。

Setting the `COMPOSE_FILE` environment variable is equivalent to passing the `-f` flag, `COMPOSE_PROJECT_NAME` environment variable does the same as the `-p` flag, `COMPOSE_PROFILES` environment variable is equivalent to the `--profiles` flag and `COMPOSE_PARALLEL_LIMIT` does the same as the `--parallel` flag.

​	设置 `COMPOSE_FILE` 环境变量相当于传递 `-f` 标志，`COMPOSE_PROJECT_NAME` 环境变量与 `-p` 标志相同，`COMPOSE_PROFILES` 环境变量相当于 `--profiles` 标志，`COMPOSE_PARALLEL_LIMIT` 与 `--parallel` 标志相同。

If flags are explicitly set on the command line, the associated environment variable is ignored.

​	如果命令行上显式设置了标志，则会忽略关联的环境变量。

Setting the `COMPOSE_IGNORE_ORPHANS` environment variable to `true` stops docker compose from detecting orphaned containers for the project.

​	将 `COMPOSE_IGNORE_ORPHANS` 环境变量设置为 `true` 可以阻止 docker compose 检测项目的孤儿容器。

Setting the `COMPOSE_MENU` environment variable to `false` disables the helper menu when running `docker compose up` in attached mode. Alternatively, you can also run `docker compose up --menu=false` to disable the helper menu.

​	将 `COMPOSE_MENU` 环境变量设置为 `false` 会在以附加模式运行 `docker compose up` 时禁用帮助菜单。或者，你也可以运行 `docker compose up --menu=false` 来禁用帮助菜单。

### 使用干运行模式测试你的命令 Use Dry Run mode to test your command

Use `--dry-run` flag to test a command without changing your application stack state. Dry Run mode shows you all the steps Compose applies when executing a command, for example:

​	使用 `--dry-run` 标志测试命令而不改变你的应用堆栈状态。干运行模式会显示 Compose 执行命令时应用的所有步骤，例如：

```console
$ docker compose --dry-run up --build -d
[+] Pulling 1/1
 ✔ DRY-RUN MODE -  db Pulled                                                                                                                                                                                                               0.9s
[+] Running 10/8
 ✔ DRY-RUN MODE -    build service backend                                                                                                                                                                                                 0.0s
 ✔ DRY-RUN MODE -  ==> ==> writing image dryRun-754a08ddf8bcb1cf22f310f09206dd783d42f7dd                                                                                                                                                   0.0s
 ✔ DRY-RUN MODE -  ==> ==> naming to nginx-golang-mysql-backend                                                                                                                                                                            0.0s
 ✔ DRY-RUN MODE -  Network nginx-golang-mysql_default                                    Created                                                                                                                                           0.0s
 ✔ DRY-RUN MODE -  Container nginx-golang-mysql-db-1                                     Created                                                                                                                                           0.0s
 ✔ DRY-RUN MODE -  Container nginx-golang-mysql-backend-1                                Created                                                                                                                                           0.0s
 ✔ DRY-RUN MODE -  Container nginx-golang-mysql-proxy-1                                  Created                                                                                                                                           0.0s
 ✔ DRY-RUN MODE -  Container nginx-golang-mysql-db-1                                     Healthy                                                                                                                                           0.5s
 ✔ DRY-RUN MODE -  Container nginx-golang-mysql-backend-1                                Started                                                                                                                                           0.0s
 ✔ DRY-RUN MODE -  Container nginx-golang-mysql-proxy-1                                  Started                                     Started
```

From the example above, you can see that the first step is to pull the image defined by `db` service, then build the `backend` service. Next, the containers are created. The `db` service is started, and the `backend` and `proxy` wait until the `db` service is healthy before starting.

​	从上面的示例中，你可以看到第一步是拉取由 `db` 服务定义的镜像，然后构建 `backend` 服务。接下来，创建容器。`db` 服务启动，而 `backend` 和 `proxy` 等待 `db` 服务健康后再启动。

Dry Run mode works with almost all commands. You cannot use Dry Run mode with a command that doesn't change the state of a Compose stack such as `ps`, `ls`, `logs` for example.

​	干运行模式适用于几乎所有命令。你不能对不改变 Compose 堆栈状态的命令（例如 `ps`、`ls`、`logs`）使用干运行模式。

## Options

| Option                | Default | Description                                                  |
| --------------------- | ------- | ------------------------------------------------------------ |
| `--all-resources`     |         | 包含所有资源，即使那些不被服务使用的资源 Include all resources, even those not used by services |
| `--ansi`              | `auto`  | 控制何时打印 ANSI 控制字符（"never" Control when to print ANSI control characters ("never"\|"always"\|"auto") |
| `--compatibility`     |         | 以向后兼容模式运行 compose Run compose in backward compatibility mode |
| `--dry-run`           |         | 在干运行模式下执行命令 Execute command in dry run mode       |
| `--env-file`          |         | 指定备用环境文件 Specify an alternate environment file       |
| `-f, --file`          |         | Compose 配置文件 Compose configuration files                 |
| `--parallel`          | `-1`    | 控制最大并行度，-1 表示无限 Control max parallelism, -1 for unlimited |
| `--profile`           |         | 指定要启用的配置文件 Specify a profile to enable             |
| `--progress`          | `auto`  | 设置进度输出的类型（auto、tty、plain、json、quiet） Set type of progress output (auto, tty, plain, json, quiet) |
| `--project-directory` |         | 指定备用工作目录（默认：第一个指定的 Compose 文件的路径） Specify an alternate working directory (default: the path of the, first specified, Compose file) |
| `-p, --project-name`  |         | 项目名称 Project name                                        |

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker compose alpha`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposealpha" >}}) | 实验性命令 Experimental commands                             |
| [`docker compose build`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposebuild" >}}) | 构建或重建服务 Build or rebuild services                     |
| [`docker compose config`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeconfig" >}}) | 解析、解决并以规范格式呈现 compose 文件 Parse, resolve and render compose file in canonical format |
| [`docker compose cp`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposecp" >}}) | 在服务容器和本地文件系统之间复制文件/文件夹 Copy files/folders between a service container and the local filesystem |
| [`docker compose create`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposecreate" >}}) | 为服务创建容器Creates containers for a service               |
| [`docker compose down`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposedown" >}}) | 停止并删除容器、网络 Stop and remove containers, networks    |
| [`docker compose events`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeevents" >}}) | 接收来自容器的实时事件 Receive real time events from containers |
| [`docker compose exec`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeexec" >}}) | 在运行的容器中执行命令 Execute a command in a running container |
| [`docker compose images`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeimages" >}}) | 列出创建的容器使用的镜像 List images used by the created containers |
| [`docker compose kill`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposekill" >}}) | 强制停止服务容器 Force stop service containers               |
| [`docker compose logs`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposelogs" >}}) | 查看容器的输出 View output from containers                   |
| [`docker compose ls`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposels" >}}) | 列出正在运行的 compose 项目 List running compose projects    |
| [`docker compose pause`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposepause" >}}) | 暂停服务 Pause services                                      |
| [`docker compose port`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeport" >}}) | 打印端口绑定的公共端口 Print the public port for a port binding |
| [`docker compose ps`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeps" >}}) | 列出容器 List containers                                     |
| [`docker compose pull`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposepull" >}}) | 拉取服务镜像 Pull service images                             |
| [`docker compose push`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposepush" >}}) | 推送服务镜像 Push service images                             |
| [`docker compose restart`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposerestart" >}}) | 重启服务容器 Restart service containers                      |
| [`docker compose rm`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposerm" >}}) | 删除已停止的服务容器 Removes stopped service containers      |
| [`docker compose run`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposerun" >}}) | 在服务上运行一次性命令 Run a one-off command on a service    |
| [`docker compose start`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposestart" >}}) | 启动服务 Start services                                      |
| [`docker compose stop`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposestop" >}}) | 停止服务 Stop services                                       |
| [`docker compose top`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposetop" >}}) | 显示正在运行的进程 Display the running processes             |
| [`docker compose unpause`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeunpause" >}}) | 解除暂停服务 Unpause services                                |
| [`docker compose up`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeup" >}}) | 创建并启动容器 Create and start containers                   |
| [`docker compose version`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeversion" >}}) | 显示 Docker Compose 版本信息 Show the Docker Compose version information |
| [`docker compose wait`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposewait" >}}) | 阻塞直到第一个服务容器停止 Block until the first service container stops |
| [`docker compose watch`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposewatch" >}}) | 监视服务的构建上下文，并在文件更新时重建/刷新容器 Watch build context for service and rebuild/refresh containers when files are updated |
