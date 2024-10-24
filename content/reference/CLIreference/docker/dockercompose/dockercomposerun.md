+++
title = "docker compose run"
date = 2024-10-23T14:54:43+08:00
weight = 190
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/run/](https://docs.docker.com/reference/cli/docker/compose/run/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose run

| Description | Run a one-off command on a service                         |
| :---------- | ---------------------------------------------------------- |
| Usage       | `docker compose run [OPTIONS] SERVICE [COMMAND] [ARGS...]` |

## [Description](https://docs.docker.com/reference/cli/docker/compose/run/#description)

Runs a one-time command against a service.

The following command starts the `web` service and runs `bash` as its command:



```console
$ docker compose run web bash
```

Commands you use with run start in new containers with configuration defined by that of the service, including volumes, links, and other details. However, there are two important differences:

First, the command passed by `run` overrides the command defined in the service configuration. For example, if the `web` service configuration is started with `bash`, then `docker compose run web python app.py` overrides it with `python app.py`.

The second difference is that the `docker compose run` command does not create any of the ports specified in the service configuration. This prevents port collisions with already-open ports. If you do want the service’s ports to be created and mapped to the host, specify the `--service-ports`



```console
$ docker compose run --service-ports web python manage.py shell
```

Alternatively, manual port mapping can be specified with the `--publish` or `-p` options, just as when using docker run:



```console
$ docker compose run --publish 8080:80 -p 2022:22 -p 127.0.0.1:2021:21 web python manage.py shell
```

If you start a service configured with links, the run command first checks to see if the linked service is running and starts the service if it is stopped. Once all the linked services are running, the run executes the command you passed it. For example, you could run:



```console
$ docker compose run db psql -h db -U docker
```

This opens an interactive PostgreSQL shell for the linked `db` container.

If you do not want the run command to start linked containers, use the `--no-deps` flag:



```console
$ docker compose run --no-deps web python manage.py shell
```

If you want to remove the container after running while overriding the container’s restart policy, use the `--rm` flag:



```console
$ docker compose run --rm web python manage.py db upgrade
```

This runs a database upgrade script, and removes the container when finished running, even if a restart policy is specified in the service configuration.

## [Options](https://docs.docker.com/reference/cli/docker/compose/run/#options)

| Option                | Default | Description                                                  |
| --------------------- | ------- | ------------------------------------------------------------ |
| `--build`             |         | Build image before starting container                        |
| `--cap-add`           |         | Add Linux capabilities                                       |
| `--cap-drop`          |         | Drop Linux capabilities                                      |
| `-d, --detach`        |         | Run container in background and print container ID           |
| `--entrypoint`        |         | Override the entrypoint of the image                         |
| `-e, --env`           |         | Set environment variables                                    |
| `-i, --interactive`   | `true`  | Keep STDIN open even if not attached                         |
| `-l, --label`         |         | Add or override a label                                      |
| `--name`              |         | Assign a name to the container                               |
| `-T, --no-TTY`        | `true`  | Disable pseudo-TTY allocation (default: auto-detected)       |
| `--no-deps`           |         | Don't start linked services                                  |
| `-p, --publish`       |         | Publish a container's port(s) to the host                    |
| `--quiet-pull`        |         | Pull without printing progress information                   |
| `--remove-orphans`    |         | Remove containers for services not defined in the Compose file |
| `--rm`                |         | Automatically remove the container when it exits             |
| `-P, --service-ports` |         | Run command with all service's ports enabled and mapped to the host |
| `--use-aliases`       |         | Use the service's network useAliases in the network(s) the container connects to |
| `-u, --user`          |         | Run as specified username or uid                             |
| `-v, --volume`        |         | Bind mount a volume                                          |
| `-w, --workdir`       |         | Working directory inside the container                       |
