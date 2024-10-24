+++
title = "docker compose"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/](https://docs.docker.com/reference/cli/docker/compose/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose

| Description | Docker Compose   |
| :---------- | ---------------- |
| Usage       | `docker compose` |

## [Description](https://docs.docker.com/reference/cli/docker/compose/#description)

You can use the compose subcommand, `docker compose [-f <arg>...] [options] [COMMAND] [ARGS...]`, to build and manage multiple services in Docker containers.

### [Use `-f` to specify the name and path of one or more Compose files](https://docs.docker.com/reference/cli/docker/compose/#use--f-to-specify-the-name-and-path-of-one-or-more-compose-files)

Use the `-f` flag to specify the location of a Compose configuration file.

#### [Specifying multiple Compose files](https://docs.docker.com/reference/cli/docker/compose/#specifying-multiple-compose-files)

You can supply multiple `-f` configuration files. When you supply multiple files, Compose combines them into a single configuration. Compose builds the configuration in the order you supply the files. Subsequent files override and add to their predecessors.

For example, consider this command line:



```console
$ docker compose -f docker-compose.yml -f docker-compose.admin.yml run backup_db
```

The `docker-compose.yml` file might specify a `webapp` service.



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



```yaml
services:
  webapp:
    build: .
    environment:
      - DEBUG=1
```

When you use multiple Compose files, all paths in the files are relative to the first configuration file specified with `-f`. You can use the `--project-directory` option to override this base path.

Use a `-f` with `-` (dash) as the filename to read the configuration from stdin. When stdin is used all paths in the configuration are relative to the current working directory.

The `-f` flag is optional. If you don’t provide this flag on the command line, Compose traverses the working directory and its parent directories looking for a `compose.yaml` or `docker-compose.yaml` file.

#### [Specifying a path to a single Compose file](https://docs.docker.com/reference/cli/docker/compose/#specifying-a-path-to-a-single-compose-file)

You can use the `-f` flag to specify a path to a Compose file that is not located in the current directory, either from the command line or by setting up a `COMPOSE_FILE` environment variable in your shell or in an environment file.

For an example of using the `-f` option at the command line, suppose you are running the Compose Rails sample, and have a `compose.yaml` file in a directory called `sandbox/rails`. You can use a command like `docker compose pull` to get the postgres image for the db service from anywhere by using the `-f` flag as follows:



```console
$ docker compose -f ~/sandbox/rails/compose.yaml pull db
```

### [Use `-p` to specify a project name](https://docs.docker.com/reference/cli/docker/compose/#use--p-to-specify-a-project-name)

Each configuration has a project name. Compose sets the project name using the following mechanisms, in order of precedence:

- The `-p` command line flag
- The `COMPOSE_PROJECT_NAME` environment variable
- The top level `name:` variable from the config file (or the last `name:` from a series of config files specified using `-f`)
- The `basename` of the project directory containing the config file (or containing the first config file specified using `-f`)
- The `basename` of the current directory if no config file is specified Project names must contain only lowercase letters, decimal digits, dashes, and underscores, and must begin with a lowercase letter or decimal digit. If the `basename` of the project directory or current directory violates this constraint, you must use one of the other mechanisms.



```console
$ docker compose -p my_project ps -a
NAME                 SERVICE    STATUS     PORTS
my_project_demo_1    demo       running

$ docker compose -p my_project logs
demo_1  | PING localhost (127.0.0.1): 56 data bytes
demo_1  | 64 bytes from 127.0.0.1: seq=0 ttl=64 time=0.095 ms
```

### [Use profiles to enable optional services](https://docs.docker.com/reference/cli/docker/compose/#use-profiles-to-enable-optional-services)

Use `--profile` to specify one or more active profiles Calling `docker compose --profile frontend up` starts the services with the profile `frontend` and services without any specified profiles. You can also enable multiple profiles, e.g. with `docker compose --profile frontend --profile debug up` the profiles `frontend` and `debug` is enabled.

Profiles can also be set by `COMPOSE_PROFILES` environment variable.

### [Configuring parallelism](https://docs.docker.com/reference/cli/docker/compose/#configuring-parallelism)

Use `--parallel` to specify the maximum level of parallelism for concurrent engine calls. Calling `docker compose --parallel 1 pull` pulls the pullable images defined in the Compose file one at a time. This can also be used to control build concurrency.

Parallelism can also be set by the `COMPOSE_PARALLEL_LIMIT` environment variable.

### [Set up environment variables](https://docs.docker.com/reference/cli/docker/compose/#set-up-environment-variables)

You can set environment variables for various docker compose options, including the `-f`, `-p` and `--profiles` flags.

Setting the `COMPOSE_FILE` environment variable is equivalent to passing the `-f` flag, `COMPOSE_PROJECT_NAME` environment variable does the same as the `-p` flag, `COMPOSE_PROFILES` environment variable is equivalent to the `--profiles` flag and `COMPOSE_PARALLEL_LIMIT` does the same as the `--parallel` flag.

If flags are explicitly set on the command line, the associated environment variable is ignored.

Setting the `COMPOSE_IGNORE_ORPHANS` environment variable to `true` stops docker compose from detecting orphaned containers for the project.

Setting the `COMPOSE_MENU` environment variable to `false` disables the helper menu when running `docker compose up` in attached mode. Alternatively, you can also run `docker compose up --menu=false` to disable the helper menu.

### [Use Dry Run mode to test your command](https://docs.docker.com/reference/cli/docker/compose/#use-dry-run-mode-to-test-your-command)

Use `--dry-run` flag to test a command without changing your application stack state. Dry Run mode shows you all the steps Compose applies when executing a command, for example:



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

Dry Run mode works with almost all commands. You cannot use Dry Run mode with a command that doesn't change the state of a Compose stack such as `ps`, `ls`, `logs` for example.

## [Options](https://docs.docker.com/reference/cli/docker/compose/#options)

| Option                | Default | Description                                                  |
| --------------------- | ------- | ------------------------------------------------------------ |
| `--all-resources`     |         | Include all resources, even those not used by services       |
| `--ansi`              | `auto`  | Control when to print ANSI control characters ("never"\|"always"\|"auto") |
| `--compatibility`     |         | Run compose in backward compatibility mode                   |
| `--dry-run`           |         | Execute command in dry run mode                              |
| `--env-file`          |         | Specify an alternate environment file                        |
| `-f, --file`          |         | Compose configuration files                                  |
| `--parallel`          | `-1`    | Control max parallelism, -1 for unlimited                    |
| `--profile`           |         | Specify a profile to enable                                  |
| `--progress`          | `auto`  | Set type of progress output (auto, tty, plain, json, quiet)  |
| `--project-directory` |         | Specify an alternate working directory (default: the path of the, first specified, Compose file) |
| `-p, --project-name`  |         | Project name                                                 |

## [Subcommands](https://docs.docker.com/reference/cli/docker/compose/#subcommands)

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker compose alpha`](https://docs.docker.com/reference/cli/docker/compose/alpha/) | Experimental commands                                        |
| [`docker compose build`](https://docs.docker.com/reference/cli/docker/compose/build/) | Build or rebuild services                                    |
| [`docker compose config`](https://docs.docker.com/reference/cli/docker/compose/config/) | Parse, resolve and render compose file in canonical format   |
| [`docker compose cp`](https://docs.docker.com/reference/cli/docker/compose/cp/) | Copy files/folders between a service container and the local filesystem |
| [`docker compose create`](https://docs.docker.com/reference/cli/docker/compose/create/) | Creates containers for a service                             |
| [`docker compose down`](https://docs.docker.com/reference/cli/docker/compose/down/) | Stop and remove containers, networks                         |
| [`docker compose events`](https://docs.docker.com/reference/cli/docker/compose/events/) | Receive real time events from containers                     |
| [`docker compose exec`](https://docs.docker.com/reference/cli/docker/compose/exec/) | Execute a command in a running container                     |
| [`docker compose images`](https://docs.docker.com/reference/cli/docker/compose/images/) | List images used by the created containers                   |
| [`docker compose kill`](https://docs.docker.com/reference/cli/docker/compose/kill/) | Force stop service containers                                |
| [`docker compose logs`](https://docs.docker.com/reference/cli/docker/compose/logs/) | View output from containers                                  |
| [`docker compose ls`](https://docs.docker.com/reference/cli/docker/compose/ls/) | List running compose projects                                |
| [`docker compose pause`](https://docs.docker.com/reference/cli/docker/compose/pause/) | Pause services                                               |
| [`docker compose port`](https://docs.docker.com/reference/cli/docker/compose/port/) | Print the public port for a port binding                     |
| [`docker compose ps`](https://docs.docker.com/reference/cli/docker/compose/ps/) | List containers                                              |
| [`docker compose pull`](https://docs.docker.com/reference/cli/docker/compose/pull/) | Pull service images                                          |
| [`docker compose push`](https://docs.docker.com/reference/cli/docker/compose/push/) | Push service images                                          |
| [`docker compose restart`](https://docs.docker.com/reference/cli/docker/compose/restart/) | Restart service containers                                   |
| [`docker compose rm`](https://docs.docker.com/reference/cli/docker/compose/rm/) | Removes stopped service containers                           |
| [`docker compose run`](https://docs.docker.com/reference/cli/docker/compose/run/) | Run a one-off command on a service                           |
| [`docker compose start`](https://docs.docker.com/reference/cli/docker/compose/start/) | Start services                                               |
| [`docker compose stop`](https://docs.docker.com/reference/cli/docker/compose/stop/) | Stop services                                                |
| [`docker compose top`](https://docs.docker.com/reference/cli/docker/compose/top/) | Display the running processes                                |
| [`docker compose unpause`](https://docs.docker.com/reference/cli/docker/compose/unpause/) | Unpause services                                             |
| [`docker compose up`](https://docs.docker.com/reference/cli/docker/compose/up/) | Create and start containers                                  |
| [`docker compose version`](https://docs.docker.com/reference/cli/docker/compose/version/) | Show the Docker Compose version information                  |
| [`docker compose wait`](https://docs.docker.com/reference/cli/docker/compose/wait/) | Block until the first service container stops                |
| [`docker compose watch`](https://docs.docker.com/reference/cli/docker/compose/watch/) | Watch build context for service and rebuild/refresh containers when files are updated |
