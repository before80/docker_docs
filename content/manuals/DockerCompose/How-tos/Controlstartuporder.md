+++
title = "控制启动顺序"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/startup-order/](https://docs.docker.com/compose/how-tos/startup-order/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Control startup and shutdown order in Compose - 在 Compose 中控制启动和关闭顺序

You can control the order of service startup and shutdown with the [depends_on](https://docs.docker.com/reference/compose-file/services/#depends_on) attribute. Compose always starts and stops containers in dependency order, where dependencies are determined by `depends_on`, `links`, `volumes_from`, and `network_mode: "service:..."`.

​	您可以使用 [depends_on](https://docs.docker.com/reference/compose-file/services/#depends_on) 属性控制服务的启动和关闭顺序。Compose 始终按照依赖关系启动和停止容器，依赖关系由 `depends_on`、`links`、`volumes_from` 和 `network_mode: "service:..."` 确定。

A good example of when you might use this is an application which needs to access a database. If both services are started with `docker compose up`, there is a chance this will fail since the application service might start before the database service and won't find a database able to handle its SQL statements.

​	一个常见的应用场景是，当应用程序需要访问数据库时，如果两个服务都通过 `docker compose up` 启动，应用服务可能会在数据库服务启动之前启动，从而找不到可以处理其 SQL 语句的数据库。

## 控制启动顺序 Control startup

On startup, Compose does not wait until a container is "ready", only until it's running. This can cause issues if, for example, you have a relational database system that needs to start its own services before being able to handle incoming connections.

​	在启动时，Compose 不会等待容器完全“准备就绪”，而是仅等待其运行。这会导致问题，例如，关系数据库系统需要先启动其内部服务才能处理传入连接。

The solution for detecting the ready state of a service is to use the `condition` attribute with one of the following options:

​	检测服务就绪状态的解决方案是使用 `condition` 属性，以下选项可用：

- `service_started`
- `service_healthy`. This specifies that a dependency is expected to be “healthy”, which is defined with `healthcheck`, before starting a dependent service.
  - `service_healthy`：指定依赖项需要通过 `healthcheck` 定义的“健康”检查后，才能启动依赖服务。

- `service_completed_successfully`. This specifies that a dependency is expected to run to successful completion before starting a dependent service.
  - `service_completed_successfully`：指定依赖项需要成功完成后，才能启动依赖服务。


## 示例 Example



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

​	Compose 会按依赖关系顺序创建服务。`db` 和 `redis` 会在 `web` 之前创建。

Compose waits for healthchecks to pass on dependencies marked with `service_healthy`. `db` is expected to be "healthy" (as indicated by `healthcheck`) before `web` is created.

​	对于标记为 `service_healthy` 的依赖项，Compose 会等待其健康检查通过后才继续。例如，`db` 需要通过健康检查（通过 `healthcheck` 指定）后，才会创建 `web`。

`restart: true` ensures that if `db` is updated or restarted due to an explicit Compose operation, for example `docker compose restart`, the `web` service is also restarted automatically, ensuring it re-establishes connections or dependencies correctly.

​	`restart: true` 确保如果 `db` 因执行 Compose 操作（如 `docker compose restart`）而更新或重启，`web` 服务也会自动重启，确保其正确重新建立连接或依赖关系。

The healthcheck for the `db` service uses the `pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}'` command to check if the PostgreSQL database is ready. The service is retried every 10 seconds, up to 5 times.

​	`db` 服务的健康检查使用 `pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}` 命令来检查 PostgreSQL 数据库是否准备就绪。该服务每 10 秒重试一次，最多重试 5 次。

Compose also removes services in dependency order. `web` is removed before `db` and `redis`.

​	Compose 也会按依赖关系顺序删除服务。`web` 会在 `db` 和 `redis` 之前被删除。

## 参考信息 Reference information

- [`depends_on`](https://docs.docker.com/reference/compose-file/services/#depends_on)
- [`healthcheck`](https://docs.docker.com/reference/compose-file/services/#healthcheck)
