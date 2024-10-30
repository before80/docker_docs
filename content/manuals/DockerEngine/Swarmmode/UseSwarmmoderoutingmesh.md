+++
title = "使用 Swarm 模式路由网格"
date = 2024-10-23T14:54:40+08:00
weight = 140
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/ingress/](https://docs.docker.com/engine/swarm/ingress/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use Swarm mode routing mesh - 使用 Swarm 模式路由网格

Docker Engine Swarm mode makes it easy to publish ports for services to make them available to resources outside the swarm. All nodes participate in an ingress routing mesh. The routing mesh enables each node in the swarm to accept connections on published ports for any service running in the swarm, even if there's no task running on the node. The routing mesh routes all incoming requests to published ports on available nodes to an active container.

​	Docker Engine 的 Swarm 模式使得为服务发布端口变得简单，以便使它们可被 Swarm 外部的资源访问。所有节点都参与入口路由网格。路由网格使 Swarm 中的每个节点都可以接受正在运行的服务的已发布端口上的连接，即使该节点上没有任务在运行。路由网格将所有传入请求路由到已发布端口上的可用节点中的活跃容器。

To use the ingress network in the swarm, you need to have the following ports open between the swarm nodes before you enable Swarm mode:

​	要在 Swarm 中使用入口网络，需要在启用 Swarm 模式之前，在 Swarm 节点之间开放以下端口：

- Port `7946` TCP/UDP for container network discovery.
  - `7946` TCP/UDP 端口用于容器网络发现。

- Port `4789` UDP (configurable) for the container ingress network.
  - `4789` UDP 端口（可配置）用于容器入口网络。


When setting up networking in a Swarm, special care should be taken. Consult the [tutorial](https://docs.docker.com/engine/swarm/swarm-tutorial/#open-protocols-and-ports-between-the-hosts) for an overview.

​	在 Swarm 中设置网络时需要特别注意。请参考[教程](https://docs.docker.com/engine/swarm/swarm-tutorial/#open-protocols-and-ports-between-the-hosts) 以获得概览。

You must also open the published port between the swarm nodes and any external resources, such as an external load balancer, that require access to the port.

​	还必须在 Swarm 节点和任何需要访问端口的外部资源（例如外部负载均衡器）之间开放已发布的端口。

You can also [bypass the routing mesh](https://docs.docker.com/engine/swarm/ingress/#bypass-the-routing-mesh) for a given service.

​	您也可以为某个服务[绕过路由网格](https://docs.docker.com/engine/swarm/ingress/#bypass-the-routing-mesh)。

## 为服务发布端口 Publish a port for a service

Use the `--publish` flag to publish a port when you create a service. `target` is used to specify the port inside the container, and `published` is used to specify the port to bind on the routing mesh. If you leave off the `published` port, a random high-numbered port is bound for each service task. You need to inspect the task to determine the port.

​	在创建服务时使用 `--publish` 标志发布端口。`target` 用于指定容器内部的端口，而 `published` 用于指定绑定在路由网格上的端口。如果省略 `published` 端口，则为每个服务任务绑定一个随机的高编号端口。需要检查任务以确定端口。

```console
$ docker service create \
  --name <SERVICE-NAME> \
  --publish published=<PUBLISHED-PORT>,target=<CONTAINER-PORT> \
  IMAGE
```

> **Note**
>
> 
>
> The older form of this syntax is a colon-separated string, where the published port is first and the target port is second, such as `-p 8080:80`. The new syntax is preferred because it is easier to read and allows more flexibility.
>
> ​	较旧的语法使用冒号分隔的字符串，其中已发布端口在前，目标端口在后，例如 `-p 8080:80`。推荐使用新的语法，因为它更易读并提供更多灵活性。

The `<PUBLISHED-PORT>` is the port where the swarm makes the service available. If you omit it, a random high-numbered port is bound. The `<CONTAINER-PORT>` is the port where the container listens. This parameter is required.

​	`<PUBLISHED-PORT>` 是 Swarm 使服务可用的端口。如果省略，将绑定一个随机的高编号端口。`<CONTAINER-PORT>` 是容器监听的端口，该参数是必需的。

For example, the following command publishes port 80 in the nginx container to port 8080 for any node in the swarm:

​	例如，以下命令将 nginx 容器中的端口 80 发布到 Swarm 中任何节点的端口 8080：



```console
$ docker service create \
  --name my-web \
  --publish published=8080,target=80 \
  --replicas 2 \
  nginx
```

When you access port 8080 on any node, Docker routes your request to an active container. On the swarm nodes themselves, port 8080 may not actually be bound, but the routing mesh knows how to route the traffic and prevents any port conflicts from happening.

​	访问 Swarm 中任意节点上的端口 8080 时，Docker 会将请求路由到活跃的容器。在 Swarm 节点上，端口 8080 可能实际上未绑定，但路由网格知道如何路由流量，并防止端口冲突。

The routing mesh listens on the published port for any IP address assigned to the node. For externally routable IP addresses, the port is available from outside the host. For all other IP addresses the access is only available from within the host.

​	路由网格监听分配给节点的任何 IP 地址的已发布端口。对于可路由的外部 IP 地址，该端口可从主机外部访问。对于其他 IP 地址，访问仅限于主机内部。

![Service ingress image](UseSwarmmoderoutingmesh_img/ingress-routing-mesh.webp)

You can publish a port for an existing service using the following command:

​	可以使用以下命令为现有服务发布端口：



```console
$ docker service update \
  --publish-add published=<PUBLISHED-PORT>,target=<CONTAINER-PORT> \
  SERVICE
```

You can use `docker service inspect` to view the service's published port. For instance:

​	可以使用 `docker service inspect` 查看服务的已发布端口，例如：



```console
$ docker service inspect --format="{{json .Endpoint.Spec.Ports}}" my-web

[{"Protocol":"tcp","TargetPort":80,"PublishedPort":8080}]
```

The output shows the `<CONTAINER-PORT>` (labeled `TargetPort`) from the containers and the `<PUBLISHED-PORT>` (labeled `PublishedPort`) where nodes listen for requests for the service.

​	输出显示了容器中的 `<CONTAINER-PORT>`（标签为 `TargetPort`）和节点监听服务请求的 `<PUBLISHED-PORT>`（标签为 `PublishedPort`）。

### 仅发布 TCP 或 UDP 端口 Publish a port for TCP only or UDP only

By default, when you publish a port, it is a TCP port. You can specifically publish a UDP port instead of or in addition to a TCP port. When you publish both TCP and UDP ports, if you omit the protocol specifier, the port is published as a TCP port. If you use the longer syntax (recommended), set the `protocol` key to either `tcp` or `udp`.

​	默认情况下，发布端口为 TCP 端口。您可以选择性地发布 UDP 端口而不是 TCP 端口，或同时发布这两者。如果您发布两个协议端口而未指定协议，则端口作为 TCP 端口发布。如果使用较长语法（推荐），请将 `protocol` 设置为 `tcp` 或 `udp`。

#### TCP only

Long syntax:

​	长语法：

```console
$ docker service create --name dns-cache \
  --publish published=53,target=53 \
  dns-cache
```

Short syntax:

​	短语法：

```console
$ docker service create --name dns-cache \
  -p 53:53 \
  dns-cache
```

#### TCP and UDP

Long syntax:

​	长语法：

```console
$ docker service create --name dns-cache \
  --publish published=53,target=53 \
  --publish published=53,target=53,protocol=udp \
  dns-cache
```

Short syntax:

​	短语法：

```console
$ docker service create --name dns-cache \
  -p 53:53 \
  -p 53:53/udp \
  dns-cache
```

#### UDP only

Long syntax:

​	长语法：

```console
$ docker service create --name dns-cache \
  --publish published=53,target=53,protocol=udp \
  dns-cache
```

Short syntax:

​	短语法：

```console
$ docker service create --name dns-cache \
  -p 53:53/udp \
  dns-cache
```

## 绕过路由网格 Bypass the routing mesh

By default, swarm services which publish ports do so using the routing mesh. When you connect to a published port on any swarm node (whether it is running a given service or not), you are redirected to a worker which is running that service, transparently. Effectively, Docker acts as a load balancer for your swarm services.

​	默认情况下，发布端口的 Swarm 服务会使用路由网格。当连接 Swarm 中任何节点上的已发布端口时，您将被透明地重定向到运行该服务的工作节点。Docker 实际上充当 Swarm 服务的负载均衡器。

You can bypass the routing mesh, so that when you access the bound port on a given node, you are always accessing the instance of the service running on that node. This is referred to as `host` mode. There are a few things to keep in mind.

​	您可以绕过路由网格，使在特定节点上访问绑定端口时始终访问该节点上运行的服务实例。这称为 `host` 模式。需要注意以下几点：

- If you access a node which is not running a service task, the service does not listen on that port. It is possible that nothing is listening, or that a completely different application is listening.
  - 如果访问未运行服务任务的节点，则服务不会在该端口上监听。可能没有任何服务在监听，或者完全不同的应用程序在监听。

- If you expect to run multiple service tasks on each node (such as when you have 5 nodes but run 10 replicas), you cannot specify a static target port. Either allow Docker to assign a random high-numbered port (by leaving off the `published`), or ensure that only a single instance of the service runs on a given node, by using a global service rather than a replicated one, or by using placement constraints.
  - 如果期望在每个节点上运行多个服务任务（例如当您有 5 个节点但运行 10 个副本时），则不能指定静态目标端口。可以让 Docker 分配随机的高编号端口（通过省略 `published`），或确保每个节点上只运行一个服务实例，使用全局服务而不是复制的服务，或使用放置约束。


To bypass the routing mesh, you must use the long `--publish` service and set `mode` to `host`. If you omit the `mode` key or set it to `ingress`, the routing mesh is used. The following command creates a global service using `host` mode and bypassing the routing mesh.

​	要绕过路由网格，必须使用长 `--publish` 语法并将 `mode` 设置为 `host`。如果省略 `mode` 或将其设置为 `ingress`，则使用路由网格。以下命令使用 `host` 模式并绕过路由网格创建一个全局服务：

```console
$ docker service create --name dns-cache \
  --publish published=53,target=53,protocol=udp,mode=host \
  --mode global \
  dns-cache
```

## 配置外部负载均衡器 Configure an external load balancer

You can configure an external load balancer for swarm services, either in combination with the routing mesh or without using the routing mesh at all.

​	可以为 Swarm 服务配置外部负载均衡器，既可以与路由网格结合使用，也可以完全绕过路由网格。

### 使用路由网格 Using the routing mesh

You can configure an external load balancer to route requests to a swarm service. For example, you could configure [HAProxy](https://www.haproxy.org/) to balance requests to an nginx service published to port 8080.

​	可以配置外部负载均衡器来路由请求到 Swarm 服务。例如，您可以配置 [HAProxy](https://www.haproxy.org/) 来均衡到发布到端口 8080 的 nginx 服务的请求。

![Ingress with external load balancer image](UseSwarmmoderoutingmesh_img/ingress-lb.webp)

In this case, port 8080 must be open between the load balancer and the nodes in the swarm. The swarm nodes can reside on a private network that is accessible to the proxy server, but that is not publicly accessible.

​	在这种情况下，端口 8080 必须在负载均衡器和 Swarm 节点之间开放。Swarm 节点可以位于代理服务器可访问的私有网络中，但不对外部公开。

You can configure the load balancer to balance requests between every node in the swarm even if there are no tasks scheduled on the node. For example, you could have the following HAProxy configuration in `/etc/haproxy/haproxy.cfg`:

​	可以将负载均衡器配置为在 Swarm 中每个节点之间均衡请求，即使该节点上没有安排任务。例如，您可以在 `/etc/haproxy/haproxy.cfg` 中配置以下 HAProxy：



```bash
global
        log /dev/log    local0
        log /dev/log    local1 notice
...snip...

# Configure HAProxy to listen on port 80
# 配置 HAProxy 在端口 80 上监听
frontend http_front
   bind *:80
   stats uri /haproxy?stats
   default_backend http_back

# Configure HAProxy to route requests to swarm nodes on port 8080
# 配置 HAProxy 将请求路由到 Swarm 节点的 8080 端口
backend http_back
   balance roundrobin
   server node1 192.168.99.100:8080 check
   server node2 192.168.99.101:8080 check
   server node3 192.168.99.102:8080 check
```

When you access the HAProxy load balancer on port 80, it forwards requests to nodes in the swarm. The swarm routing mesh routes the request to an active task. If, for any reason the swarm scheduler dispatches tasks to different nodes, you don't need to reconfigure the load balancer.

​	访问 HAProxy 负载均衡器的端口 80 时，会将请求转发到 Swarm 中的节点。Swarm 路由网格将请求路由到活跃的任务。如果由于某种原因 Swarm 调度器将任务分派到不同的节点，则无需重新配置负载均衡器。

You can configure any type of load balancer to route requests to swarm nodes. To learn more about HAProxy, see the [HAProxy documentation](https://cbonte.github.io/haproxy-dconv/).

​	可以配置任何类型的负载均衡器来路由请求到 Swarm 节点。要了解更多有关 HAProxy 的信息，请参阅 [HAProxy 文档](https://cbonte.github.io/haproxy-dconv/)。

### 不使用路由网格 Without the routing mesh

To use an external load balancer without the routing mesh, set `--endpoint-mode` to `dnsrr` instead of the default value of `vip`. In this case, there is not a single virtual IP. Instead, Docker sets up DNS entries for the service such that a DNS query for the service name returns a list of IP addresses, and the client connects directly to one of these.

​	要在不使用路由网格的情况下使用外部负载均衡器，请将 `--endpoint-mode` 设置为 `dnsrr` 而不是默认的 `vip`。在这种情况下，没有单一的虚拟 IP。相反，Docker 为服务设置了 DNS 条目，使得对服务名称的 DNS 查询会返回 IP 地址列表，客户端可以直接连接到其中一个。

You can't use `--endpoint-mode dnsrr` together with `--publish mode=ingress`. You must run your own load balancer in front of the service. A DNS query for the service name on the Docker host returns a list of IP addresses for the nodes running the service. Configure your load balancer to consume this list and balance the traffic across the nodes. See [Configure service discovery](https://docs.docker.com/engine/swarm/networking/#configure-service-discovery).

​	不能同时使用 `--endpoint-mode dnsrr` 和 `--publish mode=ingress`。必须在服务前运行自己的负载均衡器。Docker 主机上对服务名称的 DNS 查询会返回运行该服务的节点的 IP 地址列表。将负载均衡器配置为使用此列表并在节点之间平衡流量。请参阅 [配置服务发现](https://docs.docker.com/engine/swarm/networking/#configure-service-discovery)。

## 了解更多 Learn more

- [Deploy services to a swarm 部署服务到 Swarm]({{< ref "/manuals/DockerEngine/Swarmmode/Deployservicestoaswarm" >}})
