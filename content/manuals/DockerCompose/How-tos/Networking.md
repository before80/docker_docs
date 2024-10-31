+++
title = "网络"
date = 2024-10-23T14:54:40+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/networking/](https://docs.docker.com/compose/how-tos/networking/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Networking in Compose - Compose 中的网络

> **Important**
>
> 
>
> Docker's documentation refers to and describes Compose V2 functionality.
>
> ​	Docker 的文档描述的是 Compose V2 功能。
>
> Effective July 2023, Compose V1 stopped receiving updates and is no longer in new Docker Desktop releases. Compose V2 has replaced it and is now integrated into all current Docker Desktop versions. For more information, see [Migrate to Compose V2](https://docs.docker.com/compose/migrate).
>
> ​	从 2023 年 7 月开始，Compose V1 停止更新且不再包含在新的 Docker Desktop 版本中。Compose V2 已替代 V1 并集成到所有当前 Docker Desktop 版本中。有关更多信息，请参见 [迁移到 Compose V2](https://docs.docker.com/compose/migrate)。

By default Compose sets up a single [network]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkcreate" >}}) for your app. Each container for a service joins the default network and is both reachable by other containers on that network, and discoverable by the service's name.

​	默认情况下，Compose 为您的应用程序设置一个[网络]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkcreate" >}})。每个服务的容器都会加入该默认网络，可以通过服务名称在网络上被其他容器访问和发现。

> **Note**
>
> 
>
> Your app's network is given a name based on the "project name", which is based on the name of the directory it lives in. You can override the project name with either the [`--project-name` flag]({{< ref "/reference/CLIreference/docker/dockercompose" >}}) or the [`COMPOSE_PROJECT_NAME` environment variable](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_project_name).
>
> ​	应用程序的网络名称基于“项目名称”，即应用所在目录的名称。您可以使用 [`--project-name` 参数]({{< ref "/reference/CLIreference/docker/dockercompose" >}})或 [`COMPOSE_PROJECT_NAME` 环境变量](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_project_name)覆盖项目名称。

For example, suppose your app is in a directory called `myapp`, and your `compose.yml` looks like this:

​	例如，假设您的应用位于一个名为 `myapp` 的目录中，并且您的 `compose.yml` 文件如下所示：

```yaml
services:
  web:
    build: .
    ports:
      - "8000:8000"
  db:
    image: postgres
    ports:
      - "8001:5432"
```

When you run `docker compose up`, the following happens:

​	当您运行 `docker compose up` 时，会发生以下情况：

1. A network called `myapp_default` is created. 创建一个名为 `myapp_default` 的网络。
2. A container is created using `web`'s configuration. It joins the network `myapp_default` under the name `web`. 使用 `web` 的配置创建一个容器，并以 `web` 为名称加入网络 `myapp_default`。
3. A container is created using `db`'s configuration. It joins the network `myapp_default` under the name `db`. 使用 `db` 的配置创建一个容器，并以 `db` 为名称加入网络 `myapp_default`。

Each container can now look up the service name `web` or `db` and get back the appropriate container's IP address. For example, `web`'s application code could connect to the URL `postgres://db:5432` and start using the Postgres database.

​	现在，每个容器可以通过服务名称 `web` 或 `db` 查找相应的 IP 地址。例如，`web` 的应用代码可以连接到 URL `postgres://db:5432` 并使用 Postgres 数据库。

It is important to note the distinction between `HOST_PORT` and `CONTAINER_PORT`. In the above example, for `db`, the `HOST_PORT` is `8001` and the container port is `5432` (postgres default). Networked service-to-service communication uses the `CONTAINER_PORT`. When `HOST_PORT` is defined, the service is accessible outside the swarm as well.

​	需要注意 `HOST_PORT` 和 `CONTAINER_PORT` 之间的区别。在上述示例中，对于 `db`，`HOST_PORT` 为 `8001`，而容器端口为 `5432`（Postgres 的默认端口）。服务间的网络通信使用 `CONTAINER_PORT`。当定义 `HOST_PORT` 时，服务也可在 Swarm 之外访问。

Within the `web` container, your connection string to `db` would look like `postgres://db:5432`, and from the host machine, the connection string would look like `postgres://{DOCKER_IP}:8001` for example `postgres://localhost:8001` if your container is running locally.

​	在 `web` 容器内，连接 `db` 的连接字符串为 `postgres://db:5432`；在主机上，该连接字符串为 `postgres://{DOCKER_IP}:8001`，例如 `postgres://localhost:8001`（如果容器在本地运行）。

## 更新网络上的容器 Update containers on the network

If you make a configuration change to a service and run `docker compose up` to update it, the old container is removed and the new one joins the network under a different IP address but the same name. Running containers can look up that name and connect to the new address, but the old address stops working.

​	如果对服务进行了配置更改并运行 `docker compose up` 以更新它，旧容器将被移除，新容器加入网络并分配不同的 IP 地址，但名称保持不变。正在运行的容器可以查找该名称并连接到新地址，而旧地址将失效。

If any containers have connections open to the old container, they are closed. It is a container's responsibility to detect this condition, look up the name again and reconnect.

​	如果有容器与旧容器保持连接，这些连接将被关闭。容器需检测到此情况，重新查找名称并重新连接。

> **Tip**
>
> 
>
> Reference containers by name, not IP, whenever possible. Otherwise you’ll need to constantly update the IP address you use.
>
> ​	尽量通过名称而非 IP 引用容器，否则需要频繁更新使用的 IP 地址。

## 链接容器 Link containers

Links allow you to define extra aliases by which a service is reachable from another service. They are not required to enable services to communicate. By default, any service can reach any other service at that service's name. In the following example, `db` is reachable from `web` at the hostnames `db` and `database`:

​	链接允许您定义其他服务可以通过额外的别名访问的服务。这不是启用服务间通信的必要条件。默认情况下，任何服务都可以通过该服务的名称访问其他服务。在以下示例中，`db` 可以通过主机名 `db` 和 `database` 从 `web` 访问：



```yaml
services:

  web:
    build: .
    links:
      - "db:database"
  db:
    image: postgres
```

See the [links reference](https://docs.docker.com/reference/compose-file/services/#links) for more information.

​	有关详细信息，请参阅 [links 参考](https://docs.docker.com/reference/compose-file/services/#links)。

## 多主机网络 Multi-host networking

When deploying a Compose application on a Docker Engine with [Swarm mode enabled]({{< ref "/manuals/DockerEngine/Swarmmode" >}}), you can make use of the built-in `overlay` driver to enable multi-host communication.

​	当在启用[Swarm 模式]({{< ref "/manuals/DockerEngine/Swarmmode" >}})的 Docker 引擎上部署 Compose 应用时，可以使用内置的 `overlay` 驱动实现多主机通信。

Overlay networks are always created as `attachable`. You can optionally set the [`attachable`](https://docs.docker.com/reference/compose-file/networks/#attachable) property to `false`.

​	Overlay 网络始终被创建为 `attachable`。您可以选择将 [`attachable`](https://docs.docker.com/reference/compose-file/networks/#attachable) 属性设置为 `false`。

Consult the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}), to see how to set up a Swarm cluster, and the [Getting started with multi-host networking]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingwithoverlaynetworks" >}}) to learn about multi-host overlay networks.

​	请参考 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})了解如何设置 Swarm 集群，并查看[多主机网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingwithoverlaynetworks" >}})以了解多主机 Overlay 网络。

## 指定自定义网络 Specify custom networks

Instead of just using the default app network, you can specify your own networks with the top-level `networks` key. This lets you create more complex topologies and specify [custom network drivers]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockernetworkdriverplugins" >}}) and options. You can also use it to connect services to externally-created networks which aren't managed by Compose.

​	您可以使用顶级 `networks` 键指定自己的网络，而不是仅使用默认的应用网络。这可以创建更复杂的拓扑结构，并指定[自定义网络驱动程序]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockernetworkdriverplugins" >}})和选项。您还可以将服务连接到非 Compose 管理的外部创建的网络。

Each service can specify what networks to connect to with the service-level `networks` key, which is a list of names referencing entries under the top-level `networks` key.

​	每个服务可以通过服务级别的 `networks` 键指定要连接的网络，该键是引用顶级 `networks` 键下条目的名称列表。

The following example shows a Compose file which defines two custom networks. The `proxy` service is isolated from the `db` service, because they do not share a network in common. Only `app` can talk to both.

​	以下示例展示了一个定义了两个自定义网络的 Compose 文件。由于没有共享的公共网络，`proxy` 服务与 `db` 服务隔离。只有 `app` 可以与两者通信。

```yaml
services:
  proxy:
    build: ./proxy
    networks:
      - frontend
  app:
    build: ./app
    networks:
      - frontend
      - backend
  db:
    image: postgres
    networks:
      - backend

networks:
  frontend:
    # Specify driver options
    # 指定驱动程序选项
    driver: bridge
    driver_opts:
      com.docker.network.bridge.host_binding_ipv4: "127.0.0.1"
  backend:
    # Use a custom driver
    # 使用自定义驱动
    driver: custom-driver
```

Networks can be configured with static IP addresses by setting the [ipv4_address and/or ipv6_address](https://docs.docker.com/reference/compose-file/services/#ipv4_address-ipv6_address) for each attached network.

​	通过为每个连接的网络设置 [ipv4_address 和/或 ipv6_address](https://docs.docker.com/reference/compose-file/services/#ipv4_address-ipv6_address)，可以为网络配置静态 IP 地址。

Networks can also be given a [custom name](https://docs.docker.com/reference/compose-file/networks/#name):

​	网络也可以通过[自定义名称](https://docs.docker.com/reference/compose-file/networks/#name)赋予特定名称：

```yaml
services:
  # ...
networks:
  frontend:
    name: custom_frontend
    driver: custom-driver-1
```

## 配置默认网络 Configure the default network

Instead of, or as well as, specifying your own networks, you can also change the settings of the app-wide default network by defining an entry under `networks` named `default`:

​	除了指定自定义网络，您还可以通过在 `networks` 下定义名为 `default` 的条目更改应用的默认网络设置：

```yaml
services:
  web:
    build: .
    ports:
      - "8000:8000"
  db:
    image: postgres

networks:
  default:
    # Use a custom driver
    # 使用自定义驱动
    driver: custom-driver-1
```

## 使用预先存在的网络 Use a pre-existing network

If you want your containers to join a pre-existing network, use the [`external` option](https://docs.docker.com/reference/compose-file/networks/#external)

​	如果您希望容器加入现有的网络，请使用 [`external` 选项](https://docs.docker.com/reference/compose-file/networks/#external)：

```yaml
services:
  # ...
networks:
  network1:
    name: my-pre-existing-network
    external: true
```

Instead of attempting to create a network called `[projectname]_default`, Compose looks for a network called `my-pre-existing-network` and connects your app's containers to it.

​	Compose 不会尝试创建名为 `[projectname]_default` 的网络，而是查找名为 `my-pre-existing-network` 的网络并将您的应用容器连接到该网络。

## 进一步的参考信息 Further reference information

For full details of the network configuration options available, see the following references:

​	有关网络配置选项的完整详细信息，请参阅以下参考：

- [Top-level `networks` element 顶级 `networks` 元素]({{< ref "/reference/Composefilereference/Networkstop-levelelements" >}})
- [Service-level `networks` attribute 服务级别的 `networks` 属性](https://docs.docker.com/reference/compose-file/services/#networks)

