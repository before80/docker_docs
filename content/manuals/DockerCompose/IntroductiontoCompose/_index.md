+++
title = "Introduction to Compose"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: []()
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# How Compose works

With Docker Compose you use a YAML configuration file, known as the [Compose file](https://docs.docker.com/compose/intro/compose-application-model/#the-compose-file), to configure your application’s services, and then you create and start all the services from your configuration with the [Compose CLI](https://docs.docker.com/compose/intro/compose-application-model/#cli).

The Compose file, or `compose.yaml` file, follows the rules provided by the [Compose Specification](https://docs.docker.com/reference/compose-file/) in how to define multi-container applications. This is the Docker Compose implementation of the formal [Compose Specification](https://github.com/compose-spec/compose-spec).

## [The Compose file](https://docs.docker.com/compose/intro/compose-application-model/#the-compose-file)

The default path for a Compose file is `compose.yaml` (preferred) or `compose.yml` that is placed in the working directory. Compose also supports `docker-compose.yaml` and `docker-compose.yml` for backwards compatibility of earlier versions. If both files exist, Compose prefers the canonical `compose.yaml`.

You can use [fragments](https://docs.docker.com/reference/compose-file/fragments/) and [extensions](https://docs.docker.com/reference/compose-file/extension/) to keep your Compose file efficient and easy to maintain.

Multiple Compose files can be [merged](https://docs.docker.com/reference/compose-file/merge/) together to define the application model. The combination of YAML files is implemented by appending or overriding YAML elements based on the Compose file order you set. Simple attributes and maps get overridden by the highest order Compose file, lists get merged by appending. Relative paths are resolved based on the first Compose file's parent folder, whenever complimentary files being merged are hosted in other folders. As some Compose file elements can both be expressed as single strings or complex objects, merges apply to the expanded form. For more information, see [Working with multiple Compose files](https://docs.docker.com/compose/how-tos/multiple-compose-files/).

If you want to reuse other Compose files, or factor out parts of your application model into separate Compose files, you can also use [`include`](https://docs.docker.com/reference/compose-file/include/). This is useful if your Compose application is dependent on another application which is managed by a different team, or needs to be shared with others.

## [CLI](https://docs.docker.com/compose/intro/compose-application-model/#cli)

The Docker CLI lets you interact with your Docker Compose applications through the `docker compose` command, and its subcommands. Using the CLI, you can manage the lifecycle of your multi-container applications defined in the `compose.yaml` file. The CLI commands enable you to start, stop, and configure your applications effortlessly.

### [Key commands](https://docs.docker.com/compose/intro/compose-application-model/#key-commands)

To start all the services defined in your `compose.yaml` file:



```console
$ docker compose up
```

To stop and remove the running services:



```console
$ docker compose down 
```

If you want to monitor the output of your running containers and debug issues, you can view the logs with:



```console
$ docker compose logs
```

To lists all the services along with their current status:



```console
$ docker compose ps
```

For a full list of all the Compose CLI commands, see the [reference documentation](https://docs.docker.com/reference/cli/docker/compose/).

## [Illustrative example](https://docs.docker.com/compose/intro/compose-application-model/#illustrative-example)

The following example illustrates the Compose concepts outlined above. The example is non-normative.

Consider an application split into a frontend web application and a backend service.

The frontend is configured at runtime with an HTTP configuration file managed by infrastructure, providing an external domain name, and an HTTPS server certificate injected by the platform's secured secret store.

The backend stores data in a persistent volume.

Both services communicate with each other on an isolated back-tier network, while the frontend is also connected to a front-tier network and exposes port 443 for external usage.

![Compose application example](_index_img/compose-application.webp)

The example application is composed of the following parts:

- 2 services, backed by Docker images: `webapp` and `database`
- 1 secret (HTTPS certificate), injected into the frontend
- 1 configuration (HTTP), injected into the frontend
- 1 persistent volume, attached to the backend
- 2 networks



```yml
services:
  frontend:
    image: example/webapp
    ports:
      - "443:8043"
    networks:
      - front-tier
      - back-tier
    configs:
      - httpd-config
    secrets:
      - server-certificate

  backend:
    image: example/database
    volumes:
      - db-data:/etc/data
    networks:
      - back-tier

volumes:
  db-data:
    driver: flocker
    driver_opts:
      size: "10GiB"

configs:
  httpd-config:
    external: true

secrets:
  server-certificate:
    external: true

networks:
  # The presence of these objects is sufficient to define them
  front-tier: {}
  back-tier: {}
```

The `docker compose up` command starts the `frontend` and `backend` services, create the necessary networks and volumes, and injects the configuration and secret into the frontend service.

`docker compose ps` provides a snapshot of the current state of your services, making it easy to see which containers are running, their status, and the ports they are using:



```text
$ docker compose ps

NAME                IMAGE                COMMAND                  SERVICE             CREATED             STATUS              PORTS
example-frontend-1  example/webapp       "nginx -g 'daemon of…"   frontend            2 minutes ago       Up 2 minutes        0.0.0.0:443->8043/tcp
example-backend-1   example/database     "docker-entrypoint.s…"   backend             2 minutes ago       Up 2 minutes
```

## [What's next](https://docs.docker.com/compose/intro/compose-application-model/#whats-next)

- [Quickstart](https://docs.docker.com/compose/gettingstarted/)
- [Explore some sample applications](https://docs.docker.com/compose/support-and-feedback/samples-for-compose/)
- [Familiarize yourself with the Compose Specification](https://docs.docker.com/reference/compose-file/)
