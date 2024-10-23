+++
title = "Control startup order"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/compose/how-tos/startup-order/](https://docs.docker.com/compose/how-tos/startup-order/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Control startup and shutdown order in Compose

You can control the order of service startup and shutdown with the [depends_on](https://docs.docker.com/reference/compose-file/services/#depends_on) attribute. Compose always starts and stops containers in dependency order, where dependencies are determined by `depends_on`, `links`, `volumes_from`, and `network_mode: "service:..."`.

A good example of when you might use this is an application which needs to access a database. If both services are started with `docker compose up`, there is a chance this will fail since the application service might start before the database service and won't find a database able to handle its SQL statements.

## [Control startup](https://docs.docker.com/compose/how-tos/startup-order/#control-startup)

On startup, Compose does not wait until a container is "ready", only until it's running. This can cause issues if, for example, you have a relational database system that needs to start its own services before being able to handle incoming connections.

The solution for detecting the ready state of a service is to use the `condition` attribute with one of the following options:

- `service_started`
- `service_healthy`. This specifies that a dependency is expected to be “healthy”, which is defined with `healthcheck`, before starting a dependent service.
- `service_completed_successfully`. This specifies that a dependency is expected to run to successful completion before starting a dependent service.

## [Example](https://docs.docker.com/compose/how-tos/startup-order/#example)



```yaml
services:
  web:
    build: .
    depends_on:
      db:
        condition: service_healthy
        restart: true
      redis:
        condition: service_started
  redis:
    image: redis
  db:
    image: postgres
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
      interval: 10s
      retries: 5
      start_period: 30s
      timeout: 10s
```

Compose creates services in dependency order. `db` and `redis` are created before `web`.

Compose waits for healthchecks to pass on dependencies marked with `service_healthy`. `db` is expected to be "healthy" (as indicated by `healthcheck`) before `web` is created.

`restart: true` ensures that if `db` is updated or restarted due to an explicit Compose operation, for example `docker compose restart`, the `web` service is also restarted automatically, ensuring it re-establishes connections or dependencies correctly.

The healthcheck for the `db` service uses the `pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}'` command to check if the PostgreSQL database is ready. The service is retried every 10 seconds, up to 5 times.

Compose also removes services in dependency order. `web` is removed before `db` and `redis`.

## [Reference information](https://docs.docker.com/compose/how-tos/startup-order/#reference-information)

- [`depends_on`](https://docs.docker.com/reference/compose-file/services/#depends_on)
- [`healthcheck`](https://docs.docker.com/reference/compose-file/services/#healthcheck)
