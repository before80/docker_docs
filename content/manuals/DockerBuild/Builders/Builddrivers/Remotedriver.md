+++
title = "远程驱动"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/builders/drivers/remote/](https://docs.docker.com/build/builders/drivers/remote/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Remote driver - 远程驱动

The Buildx remote driver allows for more complex custom build workloads, allowing you to connect to externally managed BuildKit instances. This is useful for scenarios that require manual management of the BuildKit daemon, or where a BuildKit daemon is exposed from another source.

​	Buildx 的远程驱动允许您连接到外部管理的 BuildKit 实例，以实现更复杂的自定义构建工作负载。这适用于需要手动管理 BuildKit 守护进程或由其他来源暴露的 BuildKit 守护进程的场景。

## 概述 Synopsis



```console
$ docker buildx create \
  --name remote \
  --driver remote \
  tcp://localhost:1234
```

The following table describes the available driver-specific options that you can pass to `--driver-opt`:

​	下表描述了可以传递给 `--driver-opt` 的特定驱动选项：

| Parameter      | Type    | Default            | Description                                                  |
| -------------- | ------- | ------------------ | ------------------------------------------------------------ |
| `key`          | String  |                    | 设置 TLS 客户端密钥。 Sets the TLS client key.               |
| `cert`         | String  |                    | 向 `buildkitd` 提供的 TLS 客户端证书的绝对路径。 Absolute path to the TLS client certificate to present to `buildkitd`. |
| `cacert`       | String  |                    | 用于验证的 TLS 证书颁发机构的绝对路径。 Absolute path to the TLS certificate authority used for validation. |
| `servername`   | String  | Endpoint hostname. | 在请求中使用的 TLS 服务器名称。 TLS server name used in requests. |
| `default-load` | Boolean | `false`            | 自动将镜像加载到 Docker 引擎镜像存储中。 Automatically load images to the Docker Engine image store. |

## 示例：通过 Unix 套接字的远程 BuildKit Example: Remote BuildKit over Unix sockets

This guide shows you how to create a setup with a BuildKit daemon listening on a Unix socket, and have Buildx connect through it.

​	以下步骤展示了如何创建一个监听 Unix 套接字的 BuildKit 守护进程，并通过 Buildx 连接到它。

1. Ensure that [BuildKit](https://github.com/moby/buildkit) is installed. 确保已安装 [BuildKit](https://github.com/moby/buildkit)。

   For example, you can launch an instance of buildkitd with:

   例如，可以启动一个 buildkitd 实例：

   ```console
   $ sudo ./buildkitd --group $(id -gn) --addr unix://$HOME/buildkitd.sock
   ```

   Alternatively, [see here](https://github.com/moby/buildkit/blob/master/docs/rootless.md) for running buildkitd in rootless mode or [here](https://github.com/moby/buildkit/tree/master/examples/systemd) for examples of running it as a systemd service.

   ​	或者，您可以参考[此处](https://github.com/moby/buildkit/blob/master/docs/rootless.md)运行无 root 模式的 buildkitd，或[此处](https://github.com/moby/buildkit/tree/master/examples/systemd)以 systemd 服务的形式运行它。

2. Check that you have a Unix socket that you can connect to.

   检查您可以连接的 Unix 套接字。

   ```console
   $ ls -lh /home/user/buildkitd.sock
   srw-rw---- 1 root user 0 May  5 11:04 /home/user/buildkitd.sock
   ```

3. Connect Buildx to it using the remote driver:

   使用远程驱动连接 Buildx：

   ```console
   $ docker buildx create \
     --name remote-unix \
     --driver remote \
     unix://$HOME/buildkitd.sock
   ```

4. List available builders with `docker buildx ls`. You should then see `remote-unix` among them:

   使用 `docker buildx ls` 列出可用构建器。您应看到 `remote-unix`：

   ```console
   $ docker buildx ls
   NAME/NODE           DRIVER/ENDPOINT                        STATUS  PLATFORMS
   remote-unix         remote
     remote-unix0      unix:///home/.../buildkitd.sock        running linux/amd64, linux/amd64/v2, linux/amd64/v3, linux/386
   default *           docker
     default           default                                running linux/amd64, linux/386
   ```

You can switch to this new builder as the default using `docker buildx use remote-unix`, or specify it per build using `--builder`:

​	可以使用 `docker buildx use remote-unix` 将该构建器设为默认构建器，或在构建时使用 `--builder` 指定：

```console
$ docker buildx build --builder=remote-unix -t test --load .
```

Remember that you need to use the `--load` flag if you want to load the build result into the Docker daemon.

​	如果希望将构建结果加载到 Docker 守护进程中，请记得使用 `--load` 标志。

## 示例：在 Docker 容器中的远程 BuildKit - Example: Remote BuildKit in Docker container

This guide will show you how to create setup similar to the `docker-container` driver, by manually booting a BuildKit Docker container and connecting to it using the Buildx remote driver. This procedure will manually create a container and access it via it's exposed port. (You'd probably be better of just using the `docker-container` driver that connects to BuildKit through the Docker daemon, but this is for illustration purposes.)

​	本指南将展示如何手动启动 BuildKit Docker 容器并通过 Buildx 远程驱动连接到它的设置，这类似于 `docker-container` 驱动。本过程将手动创建一个容器并通过暴露的端口访问它。（实际应用中，使用连接 BuildKit 的 `docker-container` 驱动可能更为简便，但这里是为了演示。）

1. Generate certificates for BuildKit. 为 BuildKit 生成证书。

   You can use this [bake definition](https://github.com/moby/buildkit/blob/master/examples/create-certs) as a starting point:

   可以使用此 [bake 定义](https://github.com/moby/buildkit/blob/master/examples/create-certs)作为起点：

   ```console
   SAN="localhost 127.0.0.1" docker buildx bake "https://github.com/moby/buildkit.git#master:examples/create-certs"
   ```

   Note that while it's possible to expose BuildKit over TCP without using TLS, it's not recommended. Doing so allows arbitrary access to BuildKit without credentials.

   ​	请注意，虽然可以在没有 TLS 的情况下通过 TCP 暴露 BuildKit，但不建议这样做，因为这会允许未经身份验证的访问。

2. With certificates generated in `.certs/`, startup the container:

   使用生成的 `.certs/` 目录启动容器：

   ```console
   $ docker run -d --rm \
     --name=remote-buildkitd \
     --privileged \
     -p 1234:1234 \
     -v $PWD/.certs:/etc/buildkit/certs \
     moby/buildkit:latest \
     --addr tcp://0.0.0.0:1234 \
     --tlscacert /etc/buildkit/certs/daemon/ca.pem \
     --tlscert /etc/buildkit/certs/daemon/cert.pem \
     --tlskey /etc/buildkit/certs/daemon/key.pem
   ```

   This command starts a BuildKit container and exposes the daemon's port 1234 to localhost.

   ​	此命令启动 BuildKit 容器，并将守护进程的 1234 端口暴露到本地主机。

3. Connect to this running container using Buildx:

   使用 Buildx 连接到运行中的容器：

   ```console
   $ docker buildx create \
     --name remote-container \
     --driver remote \
     --driver-opt cacert=${PWD}/.certs/client/ca.pem,cert=${PWD}/.certs/client/cert.pem,key=${PWD}/.certs/client/key.pem,servername=TLS_SERVER_NAME \
     tcp://localhost:1234
   ```

   Alternatively, use the `docker-container://` URL scheme to connect to the BuildKit container without specifying a port:

   ​	或者，可以使用 `docker-container://` URL 模式连接到 BuildKit 容器，而不指定端口：

   ```console
   $ docker buildx create \
     --name remote-container \
     --driver remote \
     docker-container://remote-container
   ```

## 示例：在 Kubernetes 中的远程 BuildKit - Example: Remote BuildKit in Kubernetes

This guide will show you how to create a setup similar to the `kubernetes` driver by manually creating a BuildKit `Deployment`. While the `kubernetes` driver will do this under-the-hood, it might sometimes be desirable to scale BuildKit manually. Additionally, when executing builds from inside Kubernetes pods, the Buildx builder will need to be recreated from within each pod or copied between them.

​	本指南将展示如何手动创建一个 BuildKit `Deployment`，类似于 `kubernetes` 驱动。虽然 `kubernetes` 驱动可以在后台自动完成此操作，但有时手动扩容 BuildKit 更加灵活。此外，在 Kubernetes Pod 内执行构建时，可能需要在每个 Pod 内重新创建或在它们之间复制 Buildx 构建器。

1. Create a Kubernetes deployment of `buildkitd`, as per the instructions [here](https://github.com/moby/buildkit/tree/master/examples/kubernetes). 按照[此处](https://github.com/moby/buildkit/tree/master/examples/kubernetes)的说明创建 `buildkitd` 的 Kubernetes 部署。

   Following the guide, create certificates for the BuildKit daemon and client using [create-certs.sh](https://github.com/moby/buildkit/blob/master/examples/kubernetes/create-certs.sh), and create a deployment of BuildKit pods with a service that connects to them.

   ​	根据指南，使用 [create-certs.sh](https://github.com/moby/buildkit/blob/master/examples/kubernetes/create-certs.sh) 为 BuildKit 守护进程和客户端生成证书，并创建带有连接服务的 BuildKit Pod 部署。

2. Assuming that the service is called `buildkitd`, create a remote builder in Buildx, ensuring that the listed certificate files are present:

   假设服务名为 `buildkitd`，在 Buildx 中创建一个远程构建器，并确保列出的证书文件存在：

   ```console
   $ docker buildx create \
     --name remote-kubernetes \
     --driver remote \
     --driver-opt cacert=${PWD}/.certs/client/ca.pem,cert=${PWD}/.certs/client/cert.pem,key=${PWD}/.certs/client/key.pem \
     tcp://buildkitd.default.svc:1234
   ```

Note that this only works internally, within the cluster, since the BuildKit setup guide only creates a `ClusterIP` service. To access a builder remotely, you can set up and use an ingress, which is outside the scope of this guide.

​	注意，这仅在集群内部工作，因为 BuildKit 设置指南只创建了 `ClusterIP` 服务。如需远程访问构建器，可以设置并使用 ingress，但本文不涉及该内容。

### 在 Kubernetes 中调试远程构建器 Debug a remote builder in Kubernetes

If you're having trouble accessing a remote builder deployed in Kubernetes, you can use the `kube-pod://` URL scheme to connect directly to a BuildKit pod through the Kubernetes API. Note that this method only connects to a single pod in the deployment.

​	如果在访问部署在 Kubernetes 中的远程构建器时遇到问题，可以使用 `kube-pod://` URL 模式通过 Kubernetes API 直接连接到 BuildKit Pod。请注意，此方法仅连接部署中的单个 Pod。

```console
$ kubectl get pods --selector=app=buildkitd -o json | jq -r '.items[].metadata.name'
buildkitd-XXXXXXXXXX-xxxxx
$ docker buildx create \
  --name remote-container \
  --driver remote \
  kube-pod://buildkitd-XXXXXXXXXX-xxxxx
```

Alternatively, use the port forwarding mechanism of `kubectl`:

​	或者，使用 `kubectl` 的端口转发机制：

```console
$ kubectl port-forward svc/buildkitd 1234:1234
```

Then you can point the remote driver at `tcp://localhost:1234`.

​	然后可以将远程驱动指向 `tcp://localhost:1234`。
