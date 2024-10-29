+++
title = "部署一个栈到 Docker Swarm"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/stack-deploy/](https://docs.docker.com/engine/swarm/stack-deploy/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Deploy a stack to a swarm - 部署一个栈到 Docker Swarm

When running Docker Engine in swarm mode, you can use `docker stack deploy` to deploy a complete application stack to the swarm. The `deploy` command accepts a stack description in the form of a [Compose file]({{< ref "/reference/Composefilereference/Legacyversions" >}}).

​	在 Docker Engine 运行于 Swarm 模式下时，可以使用 `docker stack deploy` 将整个应用程序栈部署到集群。`deploy` 命令接受一个 [Compose 文件]({{< ref "/reference/Composefilereference/Legacyversions" >}}) 形式的栈描述。

> **Note**
>
> 
>
> The `docker stack deploy` command uses the legacy [Compose file version 3]({{< ref "/reference/Composefilereference/Legacyversions" >}}) format, used by Compose V1. The latest format, defined by the [Compose specification]({{< ref "/reference/Composefilereference" >}}) isn't compatible with the `docker stack deploy` command.
>
> ​	`docker stack deploy` 命令使用的是 [Compose 文件版本 3]({{< ref "/reference/Composefilereference/Legacyversions" >}}) 的传统格式，即 Compose V1 使用的格式。最新的 [Compose 规范]({{< ref "/reference/Composefilereference" >}}) 格式与 `docker stack deploy` 命令不兼容。
>
> For more information about the evolution of Compose, see [History of Compose](https://docs.docker.com/compose/history/).
>
> ​	了解 Compose 的演进，请参见 [Compose 历史](https://docs.docker.com/compose/history/)。

To run through this tutorial, you need:

​	要完成此教程，需要具备以下条件：

1. A Docker Engine running in [Swarm mode]({{< ref "/manuals/DockerEngine/Swarmmode/RunDockerEngineinswarmmode" >}}). If you're not familiar with Swarm mode, you might want to read [Swarm mode key concepts]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}}) and [How services work]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Howserviceswork" >}}).一个运行在 [Swarm 模式]({{< ref "/manuals/DockerEngine/Swarmmode/RunDockerEngineinswarmmode" >}}) 下的 Docker Engine。如果你不熟悉 Swarm 模式，可以先阅读 [Swarm 模式关键概念]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}}) 和 [服务如何工作]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Howserviceswork" >}})。

   > **Note**
   >
   > 
   >
   > If you're trying things out on a local development environment, you can put your engine into Swarm mode with `docker swarm init`.
   >
   > ​	如果只是本地开发环境测试，可以通过 `docker swarm init` 启用 Swarm 模式。
   >
   > If you've already got a multi-node swarm running, keep in mind that all `docker stack` and `docker service` commands must be run from a manager node.
   >
   > ​	如果已有多节点集群，所有 `docker stack` 和 `docker service` 命令都必须在管理节点上运行。

2. A current version of [Docker Compose]({{< ref "/manuals/DockerCompose/Install" >}}).当前版本的 [Docker Compose]({{< ref "/manuals/DockerCompose/Install" >}})。

## 设置 Docker 注册表 Set up a Docker registry

Because a swarm consists of multiple Docker Engines, a registry is required to distribute images to all of them. You can use the [Docker Hub](https://hub.docker.com/) or maintain your own. Here's how to create a throwaway registry, which you can discard afterward.

​	由于集群由多个 Docker Engine 组成，需要一个注册表来将镜像分发到所有节点。可以使用 [Docker Hub](https://hub.docker.com/) 或自建注册表。以下为如何创建一个临时注册表，可在使用后弃用。

1. Start the registry as a service on your swarm:

   在集群中将注册表作为服务启动：

   ```console
   $ docker service create --name registry --publish published=5000,target=5000 registry:2
   ```

2. Check its status with `docker service ls`:

   使用 `docker service ls` 查看注册表状态：

   ```console
   $ docker service ls
   
   ID            NAME      REPLICAS  IMAGE                                                                               COMMAND
   l7791tpuwkco  registry  1/1       registry:2@sha256:1152291c7f93a4ea2ddc95e46d142c31e743b6dd70e194af9e6ebe530f782c17
   ```

   Once it reads `1/1` under `REPLICAS`, it's running. If it reads `0/1`, it's probably still pulling the image.

   ​	当 `REPLICAS` 下显示 `1/1` 时，表示服务正在运行。如果显示 `0/1`，则可能正在拉取镜像。

3. Check that it's working with `curl`:

   使用 `curl` 测试其是否正常工作：

   ```console
   $ curl http://localhost:5000/v2/
   
   {}
   ```

## 创建示例应用 Create the example application

The app used in this guide is based on the hit counter app in the [Get started with Docker Compose]({{< ref "/manuals/DockerCompose/Quickstart" >}}) guide. It consists of a Python app which maintains a counter in a Redis instance and increments the counter whenever you visit it.

​	本指南中的应用基于 [Docker Compose 入门]({{< ref "/manuals/DockerCompose/Quickstart" >}}) 指南中的点击计数器应用。它由一个 Python 应用组成，使用 Redis 记录计数，每次访问时计数增加。

1. Create a directory for the project:

   创建项目目录：

   ```console
   $ mkdir stackdemo
   $ cd stackdemo
   ```

2. Create a file called `app.py` in the project directory and paste this in:

   在项目目录下创建名为 `app.py` 的文件，并粘贴以下代码：

   ```python
   from flask import Flask
   from redis import Redis
   
   app = Flask(__name__)
   redis = Redis(host='redis', port=6379)
   
   @app.route('/')
   def hello():
       count = redis.incr('hits')
       return 'Hello World! I have been seen {} times.\n'.format(count)
   
   if __name__ == "__main__":
       app.run(host="0.0.0.0", port=8000, debug=True)
   ```

3. Create a file called `requirements.txt` and paste these two lines in:

   创建 `requirements.txt` 文件并粘贴以下内容：

   ```none
   flask
   redis
   ```

4. Create a file called `Dockerfile` and paste this in:

   创建 `Dockerfile` 文件并粘贴以下内容：

   ```dockerfile
   # syntax=docker/dockerfile:1
   FROM python:3.4-alpine
   ADD . /code
   WORKDIR /code
   RUN pip install -r requirements.txt
   CMD ["python", "app.py"]
   ```

5. Create a file called `compose.yml` and paste this in:

   创建 `compose.yml` 文件并粘贴以下内容：

   ```yaml
     services:
       web:
         image: 127.0.0.1:5000/stackdemo
         build: .
         ports:
           - "8000:8000"
       redis:
         image: redis:alpine
   ```

   The image for the web app is built using the Dockerfile defined above. It's also tagged with `127.0.0.1:5000` - the address of the registry created earlier. This is important when distributing the app to the swarm.
   
   ​	
   
   1. Web 应用的镜像通过上面的 Dockerfile 构建，并标记为 `127.0.0.1:5000`（即先前创建的注册表的地址），这是应用在集群中分发时所需的关键步骤。

## 使用 Compose 测试应用 Test the app with Compose

1. Start the app with `docker compose up`. This builds the web app image, pulls the Redis image if you don't already have it, and creates two containers. 使用 `docker compose up` 启动应用。这将构建 web 应用镜像，拉取 Redis 镜像（如果尚未下载），并创建两个容器。

   You see a warning about the Engine being in swarm mode. This is because Compose doesn't take advantage of swarm mode, and deploys everything to a single node. You can safely ignore this.

   你可能会看到关于引擎处于 Swarm 模式的警告。这是因为 Compose 不利用 Swarm 模式，所有容器都调度到单个节点上，可以忽略此警告。

   ```console
   $ docker compose up -d
   
   WARNING: The Docker Engine you're using is running in swarm mode.
   
   Compose does not use swarm mode to deploy services to multiple nodes in
   a swarm. All containers are scheduled on the current node.
   
   To deploy your application across the swarm, use `docker stack deploy`.
   
   Creating network "stackdemo_default" with the default driver
   Building web
   ...(build output)...
   Creating stackdemo_redis_1
   Creating stackdemo_web_1
   ```

2. Check that the app is running with `docker compose ps`:

   使用 `docker compose ps` 检查应用是否在运行：

   ```console
   $ docker compose ps
   
         Name                     Command               State           Ports
   -----------------------------------------------------------------------------------
   stackdemo_redis_1   docker-entrypoint.sh redis ...   Up      6379/tcp
   stackdemo_web_1     python app.py                    Up      0.0.0.0:8000->8000/tcp
   ```

   You can test the app with `curl`:

   使用 `curl` 测试应用：

   ```console
   $ curl http://localhost:8000
   Hello World! I have been seen 1 times.
   
   $ curl http://localhost:8000
   Hello World! I have been seen 2 times.
   
   $ curl http://localhost:8000
   Hello World! I have been seen 3 times.
   ```

3. Bring the app down:

   停止应用：

   ```console
   $ docker compose down --volumes
   
   Stopping stackdemo_web_1 ... done
   Stopping stackdemo_redis_1 ... done
   Removing stackdemo_web_1 ... done
   Removing stackdemo_redis_1 ... done
   Removing network stackdemo_default
   ```

## 推送生成的镜像到注册表 Push the generated image to the registry

To distribute the web app's image across the swarm, it needs to be pushed to the registry you set up earlier. With Compose, this is very simple:

​	要将 web 应用的镜像分发到集群，可以通过 Compose 将其推送到之前设置的注册表：



```console
$ docker compose push

Pushing web (127.0.0.1:5000/stackdemo:latest)...
The push refers to a repository [127.0.0.1:5000/stackdemo]
5b5a49501a76: Pushed
be44185ce609: Pushed
bd7330a79bcf: Pushed
c9fc143a069a: Pushed
011b303988d2: Pushed
latest: digest: sha256:a81840ebf5ac24b42c1c676cbda3b2cb144580ee347c07e1bc80e35e5ca76507 size: 1372
```

The stack is now ready to be deployed.

​	现在可以将该栈部署到集群。

## 将栈部署到集群 Deploy the stack to the swarm

1. Create the stack with `docker stack deploy`:

   使用 `docker stack deploy` 创建栈：

   ```console
   $ docker stack deploy --compose-file compose.yml stackdemo
   
   Ignoring unsupported options: build
   
   Creating network stackdemo_default
   Creating service stackdemo_web
   Creating service stackdemo_redis
   ```

   The last argument is a name for the stack. Each network, volume and service name is prefixed with the stack name.

   ​	最后一个参数是栈的名称。每个网络、卷和服务名称都带有栈名称的前缀。

2. Check that it's running with `docker stack services stackdemo`:

   使用 `docker stack services stackdemo` 检查其是否运行中：

   ```console
   $ docker stack services stackdemo
   
   ID            NAME             MODE        REPLICAS  IMAGE
   orvjk2263y1p  stackdemo_redis  replicated  1/1       redis:3.2-alpine@sha256:f1ed3708f538b537eb9c2a7dd50dc90a706f7debd7e1196c9264edeea521a86d
   s1nf0xy8t1un  stackdemo_web    replicated  1/1       127.0.0.1:5000/stackdemo@sha256:adb070e0805d04ba2f92c724298370b7a4eb19860222120d43e0f6351ddbc26f
   ```

   Once it's running, you should see `1/1` under `REPLICAS` for both services. This might take some time if you have a multi-node swarm, as images need to be pulled.

   ​	运行后，`REPLICAS` 应显示 `1/1`。如有多节点集群，则可能需要一些时间来拉取镜像。

   As before, you can test the app with `curl`:

   ​	如之前，可以使用 `curl` 测试应用：

   ```console
   $ curl http://localhost:8000
   Hello World! I have been seen 1 times.
   
   $ curl http://localhost:8000
   Hello World! I have been seen 2 times.
   
   $ curl http://localhost:8000
   Hello World! I have been seen 3 times.
   ```

   With Docker's built-in routing mesh, you can access any node in the swarm on port `8000` and get routed to the app:

   ​	借助 Docker 内置的路由网格，可以通过访问集群中任何节点的 8000 端口访问应用：

   ```console
   $ curl http://address-of-other-node:8000
   Hello World! I have been seen 4 times.
   ```

3. Bring the stack down with `docker stack rm`:

   使用 `docker stack rm` 删除栈：

   ```console
   $ docker stack rm stackdemo
   
   Removing service stackdemo_web
   Removing service stackdemo_redis
   Removing network stackdemo_default
   ```

4. Bring the registry down with `docker service rm`:

   使用 `docker service rm` 删除注册表：

   ```console
   $ docker service rm registry
   ```

5. If you're just testing things out on a local machine and want to bring your Docker Engine out of Swarm mode, use `docker swarm leave`:

   如果仅在本地测试并希望退出 Swarm 模式，可以使用 `docker swarm leave`：

   ```console
   $ docker swarm leave --force
   
   Node left the swarm.
   ```
