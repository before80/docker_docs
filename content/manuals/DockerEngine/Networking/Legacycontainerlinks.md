+++
title = "Legacy container links"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/network/links/](https://docs.docker.com/engine/network/links/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Legacy container links - 旧版容器链接

> **Warning**
>
> The `--link` flag is a legacy feature of Docker. It may eventually be removed. Unless you absolutely need to continue using it, we recommend that you use user-defined networks to facilitate communication between two containers instead of using `--link`. One feature that user-defined networks do not support that you can do with `--link` is sharing environment variables between containers. However, you can use other mechanisms such as volumes to share environment variables between containers in a more controlled way.
>
> ​	`--link` 标志是 Docker 的旧功能，未来可能会被移除。除非必须继续使用它，否则建议您使用用户定义的网络来实现容器间通信，而不是使用 `--link`。用户定义的网络不支持 `--link` 提供的在容器间共享环境变量的功能，但您可以通过卷等其他机制以更可控的方式在容器间共享环境变量。
>
> See [Differences between user-defined bridges and the default bridge](https://docs.docker.com/engine/network/drivers/bridge/#differences-between-user-defined-bridges-and-the-default-bridge) for some alternatives to using `--link`.
>
> ​	请参阅[用户定义的桥接和默认桥接的差异](https://docs.docker.com/engine/network/drivers/bridge/#differences-between-user-defined-bridges-and-the-default-bridge)，了解 `--link` 的替代方案。

The information in this section explains legacy container links within the Docker default `bridge` network which is created automatically when you install Docker.

​	本节介绍 Docker 默认 `bridge` 网络中的旧版容器链接功能，该网络在您安装 Docker 时自动创建。

Before the [Docker networks feature](https://docs.docker.com/engine/network/), you could use the Docker link feature to allow containers to discover each other and securely transfer information about one container to another container. With the introduction of the Docker networks feature, you can still create links but they behave differently between default `bridge` network and [user defined networks](https://docs.docker.com/engine/network/drivers/bridge/#differences-between-user-defined-bridges-and-the-default-bridge).

​	在 [Docker 网络功能](https://docs.docker.com/engine/network/) 引入之前，可以使用 Docker 链接功能让容器相互发现，并将一个容器的信息安全地传递到另一个容器中。引入 Docker 网络功能后，您仍然可以创建链接，但在默认 `bridge` 网络和[用户定义的网络](https://docs.docker.com/engine/network/drivers/bridge/#differences-between-user-defined-bridges-and-the-default-bridge)之间，其行为有所不同。

This section briefly discusses connecting via a network port and then goes into detail on container linking in default `bridge` network.

​	本节首先简要介绍通过网络端口连接，然后详细讲解在默认 `bridge` 网络中的容器链接。

## 使用网络端口映射连接 Connect using network port mapping

Let's say you used this command to run a simple Python Flask application:

​	假设您使用以下命令运行了一个简单的 Python Flask 应用程序：

```console
$ docker run -d -P training/webapp python app.py
```

> **Note**
>
> 
>
> Containers have an internal network and an IP address. Docker can have a variety of network configurations. You can see more information on Docker networking [here](https://docs.docker.com/engine/network/).
>
> ​	容器有一个内部网络和一个 IP 地址。Docker 可以有多种网络配置。有关 Docker 网络的更多信息，请参见[此处](https://docs.docker.com/engine/network/)。

When that container was created, the `-P` flag was used to automatically map any network port inside it to a random high port within an *ephemeral port range* on your Docker host. Next, when `docker ps` was run, you saw that port 5000 in the container was bound to port 49155 on the host.

​	创建容器时使用了 `-P` 标志，将容器内部的任意网络端口自动映射到 Docker 主机上的一个 *临时端口范围* 中的随机高端口。当运行 `docker ps` 时，可以看到容器中的 5000 端口绑定到主机上的 49155 端口。

```console
$ docker ps nostalgic_morse

CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse
```

You also saw how you can bind a container's ports to a specific port using the `-p` flag. Here port 80 of the host is mapped to port 5000 of the container:

​	也可以使用 `-p` 标志将容器的端口绑定到特定端口。此处将主机的 80 端口映射到容器的 5000 端口：

```console
$ docker run -d -p 80:5000 training/webapp python app.py
```

And you saw why this isn't such a great idea because it constrains you to only one container on that specific port.

​	但这不是一个很好的选择，因为它将您限制在特定端口上只能运行一个容器。

Instead, you may specify a range of host ports to bind a container port to that is different than the default *ephemeral port range*:	

​	相反，您可以指定一系列主机端口，将容器的一个端口绑定到不同于默认的*临时端口范围*：

```console
$ docker run -d -p 8000-9000:5000 training/webapp python app.py
```

This would bind port 5000 in the container to a randomly available port between 8000 and 9000 on the host.

​	这将把容器的 5000 端口绑定到主机上的 8000 到 9000 之间的一个随机可用端口。

There are also a few other ways you can configure the `-p` flag. By default the `-p` flag binds the specified port to all interfaces on the host machine. But you can also specify a binding to a specific interface, for example only to the `localhost`.

​	此外，您还可以通过几种方式配置 `-p` 标志。默认情况下，`-p` 标志将指定端口绑定到主机上的所有接口。但您也可以将其绑定到特定接口，例如仅绑定到 `localhost`：



```console
$ docker run -d -p 127.0.0.1:80:5000 training/webapp python app.py
```

This would bind port 5000 inside the container to port 80 on the `localhost` or `127.0.0.1` interface on the host machine.

​	这会将容器中的 5000 端口绑定到主机 `localhost` 或 `127.0.0.1` 接口上的 80 端口。

Or, to bind port 5000 of the container to a dynamic port but only on the `localhost`, you could use:

​	如果要将容器的 5000 端口绑定到动态端口，但仅限 `localhost`，可以使用：



```console
$ docker run -d -p 127.0.0.1::5000 training/webapp python app.py
```

You can also bind UDP and SCTP (typically used by telecom protocols such as SIGTRAN, Diameter, and S1AP/X2AP) ports by adding a trailing `/udp` or `/sctp`. For example:

​	此外，还可以通过添加尾随的 `/udp` 或 `/sctp` 来绑定 UDP 和 SCTP（通常用于电信协议，如 SIGTRAN、Diameter 和 S1AP/X2AP）端口。例如：



```console
$ docker run -d -p 127.0.0.1:80:5000/udp training/webapp python app.py
```

You also learned about the useful `docker port` shortcut which showed us the current port bindings. This is also useful for showing you specific port configurations. For example, if you've bound the container port to the `localhost` on the host machine, then the `docker port` output reflects that.

​	可以使用有用的 `docker port` 快捷方式显示当前端口绑定情况。例如，如果容器端口已绑定到主机上的 `localhost`，则 `docker port` 输出会反映这一点。

```console
$ docker port nostalgic_morse 5000

127.0.0.1:49155
```

> **Note**
>
> 
>
> The `-p` flag can be used multiple times to configure multiple ports.
>
> ​	可以多次使用 `-p` 标志来配置多个端口。

## 使用链接系统连接 Connect with the linking system

> **Note**
>
> 
>
> This section covers the legacy link feature in the default `bridge` network. Refer to [differences between user-defined bridges and the default bridge](https://docs.docker.com/engine/network/drivers/bridge/#differences-between-user-defined-bridges-and-the-default-bridge) for more information on links in user-defined networks.
>
> ​	本节介绍了默认 `bridge` 网络中的旧版链接功能。有关用户定义网络中链接的更多信息，请参见[用户定义的桥接和默认桥接的差异](https://docs.docker.com/engine/network/drivers/bridge/#differences-between-user-defined-bridges-and-the-default-bridge)。

Network port mappings are not the only way Docker containers can connect to one another. Docker also has a linking system that allows you to link multiple containers together and send connection information from one to another. When containers are linked, information about a source container can be sent to a recipient container. This allows the recipient to see selected data describing aspects of the source container.

​	除了端口映射，Docker 容器还可以通过链接系统相互连接。链接系统允许您将多个容器连接在一起，并将一个容器的信息传递给另一个容器。通过链接，目标容器可以看到关于源容器的部分数据。

### 名称的重要性 The importance of naming

To establish links, Docker relies on the names of your containers. You've already seen that each container you create has an automatically created name; indeed you've become familiar with our old friend `nostalgic_morse` during this guide. You can also name containers yourself. This naming provides two useful functions:

​	为了建立链接，Docker 依赖于容器的名称。你可能已经注意到，每个你创建的容器都有一个自动生成的名称；在这个指南中，我们熟悉的“nostalgic_morse”就是一个例子。你也可以自行命名容器。命名容器有两个重要功能：

1. It can be useful to name containers that do specific functions in a way that makes it easier for you to remember them, for example naming a container containing a web application `web`. 可以为执行特定功能的容器命名，方便记忆，例如命名包含 Web 应用程序的容器为 `web`。
2. It provides Docker with a reference point that allows it to refer to other containers, for example, you can specify to link the container `web` to container `db`. 为 Docker 提供一个参考点，使其可以引用其他容器，例如，可以指定将容器 `web` 链接到容器 `db`。

You can name your container by using the `--name` flag, for example:

​	可以使用 `--name` 标志为容器命名，例如：

```console
$ docker run -d -P --name web training/webapp python app.py
```

This launches a new container and uses the `--name` flag to name the container `web`. You can see the container's name using the `docker ps` command.

​	这会启动一个新容器，并使用 `--name` 标志将其命名为 `web`。可以使用 `docker ps` 命令查看容器的名称。



```console
$ docker ps -l

CONTAINER ID  IMAGE                  COMMAND        CREATED       STATUS       PORTS                    NAMES
aed84ee21bde  training/webapp:latest python app.py  12 hours ago  Up 2 seconds 0.0.0.0:49154->5000/tcp  web
```

You can also use `docker inspect` to return the container's name.

​	还可以使用 `docker inspect` 返回容器的名称。

> **Note**
>
> 
>
> Container names must be unique. That means you can only call one container `web`. If you want to re-use a container name you must delete the old container (with `docker container rm`) before you can create a new container with the same name. As an alternative you can use the `--rm` flag with the `docker run` command. This deletes the container immediately after it is stopped.
>
> ​	容器名称必须唯一。这意味着只能有一个容器名为 `web`。如果需要重用容器名称，必须先删除旧的容器（使用 `docker container rm`），然后才能创建一个具有相同名称的新容器。另一种方法是在 `docker run` 命令中使用 `--rm` 标志。该标志会在容器停止后立即删除该容器。

## 通过链接进行通信 Communication across links

Links allow containers to discover each other and securely transfer information about one container to another container. When you set up a link, you create a conduit between a source container and a recipient container. The recipient can then access select data about the source. To create a link, you use the `--link` flag. First, create a new container, this time one containing a database.

​	链接允许容器相互发现，并将一个容器的信息安全地传输到另一个容器。当你设置一个链接时，会在源容器和接收容器之间创建一个通道。接收容器可以访问源容器的部分数据。要创建链接，可以使用 `--link` 标志。首先，创建一个新容器，这次包含一个数据库。



```console
$ docker run -d --name db training/postgres
```

This creates a new container called `db` from the `training/postgres` image, which contains a PostgreSQL database.

​	这将从 `training/postgres` 镜像创建一个名为 `db` 的新容器，包含一个 PostgreSQL 数据库。

Now, you need to delete the `web` container you created previously so you can replace it with a linked one:

​	现在，需要删除之前创建的 `web` 容器，以便可以用一个链接的容器替换它：

```console
$ docker container rm -f web
```

Now, create a new `web` container and link it with your `db` container.

​	接下来，创建一个新的 `web` 容器，并将其链接到你的 `db` 容器。

```console
$ docker run -d -P --name web --link db:db training/webapp python app.py
```

This links the new `web` container with the `db` container you created earlier. The `--link` flag takes the form:

​	这将新创建的 `web` 容器与之前创建的 `db` 容器链接起来。`--link` 标志的格式为：

```
--link <name or id>:alias
```

Where `name` is the name of the container we're linking to and `alias` is an alias for the link name. That alias is used shortly. The `--link` flag also takes the form:

​	其中 `name` 是要链接的容器名称，`alias` 是链接名称的别名。`--link` 标志还可以采用以下格式：

```
--link <name or id>
```

In this case the alias matches the name. You could write the previous example as:

​	此时别名与名称相同。可以将上例写成：



```console
$ docker run -d -P --name web --link db training/webapp python app.py
```

Next, inspect your linked containers with `docker inspect`:

​	然后，使用 `docker inspect` 检查你的链接容器：

```console
$ docker inspect -f "{{ .HostConfig.Links }}" web

[/db:/web/db]
```

You can see that the `web` container is now linked to the `db` container `web/db`. Which allows it to access information about the `db` container.

​	可以看到 `web` 容器现在已链接到 `db` 容器 `web/db`，这使其可以访问 `db` 容器的信息。

So what does linking the containers actually do? You've learned that a link allows a source container to provide information about itself to a recipient container. In our example, the recipient, `web`, can access information about the source `db`. To do this, Docker creates a secure tunnel between the containers that doesn't need to expose any ports externally on the container; when we started the `db` container we did not use either the `-P` or `-p` flags. That's a big benefit of linking: we don't need to expose the source container, here the PostgreSQL database, to the network.

​	那么链接容器的作用是什么？正如你所学的，链接允许源容器向接收容器提供关于自身的信息。在我们的示例中，接收方 `web` 可以访问源容器 `db` 的信息。Docker 通过两种方式向接收容器暴露源容器的连接信息：

Docker exposes connectivity information for the source container to the recipient container in two ways:

- Environment variables, 环境变量
- Updating the `/etc/hosts` file. 更新 `/etc/hosts` 文件

### 环境变量 Environment variables

Docker creates several environment variables when you link containers. Docker automatically creates environment variables in the target container based on the `--link` parameters. It also exposes all environment variables originating from Docker from the source container. These include variables from:

​	在链接容器时，Docker 会创建多个环境变量。根据 `--link` 参数，Docker 会自动在目标容器中创建环境变量。此外，它还会公开源容器中由 Docker 定义的所有环境变量，包括：

- the `ENV` commands in the source container's Dockerfile
  - Dockerfile 中的 `ENV` 命令定义的变量

- the `-e`, `--env`, and `--env-file` options on the `docker run` command when the source container is started
  - 使用 `docker run` 启动源容器时指定的 `-e`、`--env` 和 `--env-file` 选项


These environment variables enable programmatic discovery from within the target container of information related to the source container.

​	这些环境变量使得目标容器内可以以编程方式发现与源容器相关的信息。

> **Warning**
>
> 
>
> It is important to understand that all environment variables originating from Docker within a container are made available to any container that links to it. This could have serious security implications if sensitive data is stored in them.
>
> ​	需要理解的是，容器内所有由 Docker 定义的环境变量都会向任何链接到它的容器公开。如果这些变量中存储了敏感数据，可能会带来严重的安全隐患。

Docker sets an `<alias>_NAME` environment variable for each target container listed in the `--link` parameter. For example, if a new container called `web` is linked to a database container called `db` via `--link db:webdb`, then Docker creates a `WEBDB_NAME=/web/webdb` variable in the `web` container.

​	Docker 为 `--link` 参数中的每个目标容器设置一个 `<alias>_NAME` 环境变量。例如，如果通过 `--link db:webdb` 将一个新容器 `web` 链接到一个名为 `db` 的数据库容器，Docker 会在 `web` 容器中创建一个 `WEBDB_NAME=/web/webdb` 变量。

Docker also defines a set of environment variables for each port exposed by the source container. Each variable has a unique prefix in the form `<name>_PORT_<port>_<protocol>`

​	Docker 还会为源容器暴露的每个端口定义一组环境变量。每个变量有一个唯一的前缀 `<name>_PORT_<port>_<protocol>`，其中：

The components in this prefix are:

- the alias `<name>` specified in the `--link` parameter (for example, `webdb`)
  - `<name>` 是在 `--link` 参数中指定的别名（例如 `webdb`）

- the `<port>` number exposed 
  - `<port>` 是暴露的端口号

- a `<protocol>` which is either TCP or UDP
  - `<protocol>` 为 TCP 或 UDP


Docker uses this prefix format to define three distinct environment variables:

​	Docker 使用这种前缀格式定义三个不同的环境变量：

- The `prefix_ADDR` variable contains the IP Address from the URL, for example `WEBDB_PORT_5432_TCP_ADDR=172.17.0.82`.
  - `prefix_ADDR` 变量包含 URL 中的 IP 地址，例如 `WEBDB_PORT_5432_TCP_ADDR=172.17.0.82`

- The `prefix_PORT` variable contains just the port number from the URL for example `WEBDB_PORT_5432_TCP_PORT=5432`.
  - `prefix_PORT`变量仅包含 URL 中的端口号，例如 `WEBDB_PORT_5432_TCP_PORT=5432`

- The `prefix_PROTO` variable contains just the protocol from the URL for example `WEBDB_PORT_5432_TCP_PROTO=tcp`.
  - `prefix_PROTO`变量仅包含 URL 中的协议，例如 `WEBDB_PORT_5432_TCP_PROTO=tcp`


If the container exposes multiple ports, an environment variable set is defined for each one. This means, for example, if a container exposes 4 ports that Docker creates 12 environment variables, 3 for each port.

​	如果容器暴露了多个端口，则为每个端口定义一组环境变量。例如，如果容器暴露了 4 个端口，Docker 会创建 12 个环境变量，每个端口有 3 个变量。

Additionally, Docker creates an environment variable called `<alias>_PORT`. This variable contains the URL of the source container's first exposed port. The 'first' port is defined as the exposed port with the lowest number. For example, consider the `WEBDB_PORT=tcp://172.17.0.82:5432` variable. If that port is used for both tcp and udp, then the tcp one is specified.

​	此外，Docker 还会创建一个 `<alias>_PORT` 环境变量。该变量包含源容器的第一个暴露端口的 URL。“第一个”端口是指具有最低编号的暴露端口。

Finally, Docker also exposes each Docker originated environment variable from the source container as an environment variable in the target. For each variable Docker creates an `<alias>_ENV_<name>` variable in the target container. The variable's value is set to the value Docker used when it started the source container.

​	最后，Docker 还会将每个源容器的环境变量作为目标容器中的环境变量公开。对于每个变量，Docker 会在目标容器中创建一个 `<alias>_ENV_<name>` 变量，并将值设置为 Docker 启动源容器时的值。

Returning back to our database example, you can run the `env` command to list the specified container's environment variables.

​	回到我们的数据库示例，可以运行 `env` 命令来列出指定容器的环境变量。

```console
$ docker run --rm --name web2 --link db:db training/webapp env

<...>
DB_NAME=/web2/db
DB_PORT=tcp://172.17.0.5:5432
DB_PORT_5432_TCP=tcp://172.17.0.5:5432
DB_PORT_5432_TCP_PROTO=tcp
DB_PORT_5432_TCP_PORT=5432
DB_PORT_5432_TCP_ADDR=172.17.0.5
<...>
```

You can see that Docker has created a series of environment variables with useful information about the source `db` container. Each variable is prefixed with `DB_`, which is populated from the `alias` you specified above. If the `alias` were `db1`, the variables would be prefixed with `DB1_`. You can use these environment variables to configure your applications to connect to the database on the `db` container. The connection is secure and private; only the linked `web` container can communicate with the `db` container.

​	可以看到，Docker 创建了一系列关于源容器 `db` 的环境变量。每个变量的前缀为 `DB_`，这是由你指定的 `alias` 填充的。如果 `alias` 是 `db1`，变量的前缀会变为 `DB1_`。可以使用这些环境变量配置应用程序，以连接到 `db` 容器中的数据库。连接是安全且私密的；只有链接的 `web` 容器可以与 `db` 容器通信。

### 关于 Docker 环境变量的重要说明 Important notes on Docker environment variables

Unlike host entries in the [`/etc/hosts` file](https://docs.docker.com/engine/network/links/#updating-the-etchosts-file), IP addresses stored in the environment variables are not automatically updated if the source container is restarted. We recommend using the host entries in `/etc/hosts` to resolve the IP address of linked containers.

​	与[`/etc/hosts` 文件](https://docs.docker.com/engine/network/links/#updating-the-etchosts-file)中的主机条目不同，如果源容器重新启动，环境变量中存储的 IP 地址不会自动更新。建议使用 `/etc/hosts` 文件中的主机条目来解析链接容器的 IP 地址。

These environment variables are only set for the first process in the container. Some daemons, such as `sshd`, scrub them when spawning shells for connection.

​	这些环境变量仅适用于容器中的第一个进程。一些守护进程（例如 `sshd`）会在为连接生成 Shell 时清除这些变量。

### 更新 `/etc/hosts` 文件 Updating the `/etc/hosts` file

In addition to the environment variables, Docker adds a host entry for the source container to the `/etc/hosts` file. Here's an entry for the `web` container:

​	除了环境变量之外，Docker 还会将源容器的主机条目添加到 `/etc/hosts` 文件中。以下是 `web` 容器的一个条目：

```console
$ docker run -t -i --rm --link db:webdb training/webapp /bin/bash

root@aed84ee21bde:/opt/webapp# cat /etc/hosts
172.17.0.7  aed84ee21bde
<...>
172.17.0.5  webdb 6e5cdeb2d300 db
```

You can see two relevant host entries. The first is an entry for the `web` container that uses the Container ID as a host name. The second entry uses the link alias to reference the IP address of the `db` container. In addition to the alias you provide, the linked container's name, if unique from the alias provided to the `--link` parameter, and the linked container's hostname are also added to `/etc/hosts` for the linked container's IP address. You can ping that host via any of these entries:

​	可以看到两个相关的主机条目。第一个是 `web` 容器的条目，使用容器 ID 作为主机名。第二个条目使用链接别名来引用 `db` 容器的 IP 地址。除了你提供的别名外，如果链接的容器名称与提供给 `--link` 参数的别名不同，链接容器的名称和主机名也会添加到 `/etc/hosts` 中作为该链接容器的 IP 地址。可以通过以下任一条目来 ping 该主机：



```console
root@aed84ee21bde:/opt/webapp# apt-get install -yqq inetutils-ping
root@aed84ee21bde:/opt/webapp# ping webdb

PING webdb (172.17.0.5): 48 data bytes
56 bytes from 172.17.0.5: icmp_seq=0 ttl=64 time=0.267 ms
56 bytes from 172.17.0.5: icmp_seq=1 ttl=64 time=0.250 ms
56 bytes from 172.17.0.5: icmp_seq=2 ttl=64 time=0.256 ms
```

> **Note**
>
> 
>
> In the example, you had to install `ping` because it was not included in the container initially.
>
> ​	在示例中，需要安装 `ping`，因为它最初没有包含在容器中。

Here, you used the `ping` command to ping the `db` container using its host entry, which resolves to `172.17.0.5`. You can use this host entry to configure an application to make use of your `db` container.

​	这里，使用 `ping` 命令通过其主机条目 ping `db` 容器，该主机条目解析为 `172.17.0.5`。可以使用此主机条目配置应用程序以利用 `db` 容器。

> **Note**
>
> 
>
> You can link multiple recipient containers to a single source. For example, you could have multiple (differently named) web containers attached to your `db` container.
>
> ​	你可以将多个接收容器链接到一个源容器。例如，可以有多个（名称不同的）`web` 容器连接到你的 `db` 容器。

If you restart the source container, the `/etc/hosts` files on the linked containers are automatically updated with the source container's new IP address, allowing linked communication to continue.

​	如果重新启动源容器，链接容器中的 `/etc/hosts` 文件会自动更新为源容器的新 IP 地址，从而允许链接通信继续进行。



```console
$ docker restart db
db

$ docker run -t -i --rm --link db:db training/webapp /bin/bash

root@aed84ee21bde:/opt/webapp# cat /etc/hosts
172.17.0.7  aed84ee21bde
<...>
172.17.0.9  db
```
