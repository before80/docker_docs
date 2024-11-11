+++
title = "Kubernetes 驱动"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/builders/drivers/kubernetes/](https://docs.docker.com/build/builders/drivers/kubernetes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Kubernetes driver - Kubernetes 驱动

The Kubernetes driver lets you connect your local development or CI environments to builders in a Kubernetes cluster to allow access to more powerful compute resources, optionally on multiple native architectures.

​	Kubernetes 驱动允许您将本地开发或 CI 环境连接到 Kubernetes 集群中的构建器，以便访问更强大的计算资源，并且可以选择支持多个原生架构。

## 概述 Synopsis

Run the following command to create a new builder, named `kube`, that uses the Kubernetes driver:

​	运行以下命令创建一个使用 Kubernetes 驱动的新构建器，命名为 `kube`：





```console
$ docker buildx create \
  --bootstrap \
  --name=kube \
  --driver=kubernetes \
  --driver-opt=[key=value,...]
```

The following table describes the available driver-specific options that you can pass to `--driver-opt`:

​	以下表格描述了可以传递给 `--driver-opt` 的特定驱动选项：

| Parameter                    | Type         | Default                                                      | Description                                                  |
| ---------------------------- | ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `image`                      | String       |                                                              | 设置运行 BuildKit 使用的镜像。 Sets the image to use for running BuildKit. |
| `namespace`                  | String       | 当前 Kubernetes 上下文中的命名空间 Namespace in current Kubernetes context | 设置 Kubernetes 命名空间。 Sets the Kubernetes namespace.    |
| `default-load`               | Boolean      | `false`                                                      | 自动将镜像加载到 Docker 引擎镜像存储中。Automatically load images to the Docker Engine image store. |
| `replicas`                   | Integer      | 1                                                            | 设置要创建的 Pod 副本数量。参见[扩容 BuildKit](https://docs.docker.com/build/builders/drivers/kubernetes/#scaling-buildkit) Sets the number of Pod replicas to create. See [scaling BuildKit](https://docs.docker.com/build/builders/drivers/kubernetes/#scaling-buildkit) |
| `requests.cpu`               | CPU units    |                                                              | 设置以 Kubernetes CPU 单位指定的请求 CPU 值。例如 `requests.cpu=100m` 或 `requests.cpu=2`  - Sets the request CPU value specified in units of Kubernetes CPU. For example `requests.cpu=100m` or `requests.cpu=2` |
| `requests.memory`            | Memory size  |                                                              | 设置以字节或有效后缀指定的请求内存值。例如 `requests.memory=500Mi` 或 `requests.memory=4G` - Sets the request memory value specified in bytes or with a valid suffix. For example `requests.memory=500Mi` or `requests.memory=4G` |
| `requests.ephemeral-storage` | Storage size |                                                              | 设置以字节或有效后缀指定的请求临时存储值。例如 `requests.ephemeral-storage=2Gi` - Sets the request ephemeral-storage value specified in bytes or with a valid suffix. For example `requests.ephemeral-storage=2Gi` |
| `limits.cpu`                 | CPU units    |                                                              | 设置以 Kubernetes CPU 单位指定的限制 CPU 值。例如 `requests.cpu=100m` 或 `requests.cpu=2` - Sets the limit CPU value specified in units of Kubernetes CPU. For example `requests.cpu=100m` or `requests.cpu=2` |
| `limits.memory`              | Memory size  |                                                              | 设置以字节或有效后缀指定的限制内存值。例如 `requests.memory=500Mi` 或 `requests.memory=4G` - Sets the limit memory value specified in bytes or with a valid suffix. For example `requests.memory=500Mi` or `requests.memory=4G` |
| `limits.ephemeral-storage`   | Storage size |                                                              | 设置以字节或有效后缀指定的限制临时存储值。例如 `requests.ephemeral-storage=100M` - Sets the limit ephemeral-storage value specified in bytes or with a valid suffix. For example `requests.ephemeral-storage=100M` |
| `nodeselector`               | CSV string   |                                                              | 设置 Pod 的 `nodeSelector` 标签。参见[节点分配](https://docs.docker.com/build/builders/drivers/kubernetes/#node-assignment)。 - Sets the pod's `nodeSelector` label(s). See [node assignment](https://docs.docker.com/build/builders/drivers/kubernetes/#node-assignment). |
| `annotation`                 | CSV string   |                                                              | 为部署和 Pod 设置额外的注解。 Sets additional annotations on the deployments and pods. |
| `labels`                     | CSV string   |                                                              | 为部署和 Pod 设置额外的标签。 Sets additional labels on the deployments and pods. |
| `tolerations`                | CSV string   |                                                              | 配置 Pod 的容忍度。参见[节点分配](https://docs.docker.com/build/builders/drivers/kubernetes/#node-assignment)。 Configures the pod's taint toleration. See [node assignment](https://docs.docker.com/build/builders/drivers/kubernetes/#node-assignment). |
| `serviceaccount`             | String       |                                                              | 设置 Pod 的 `serviceAccountName`。 Sets the pod's `serviceAccountName`. |
| `schedulername`              | String       |                                                              | 设置负责调度 Pod 的调度器名称。 Sets the scheduler responsible for scheduling the pod. |
| `timeout`                    | Time         | `120s`                                                       | 设置在构建前 Buildx 等待 Pod 启动的超时时间。 Set the timeout limit that determines how long Buildx will wait for pods to be provisioned before a build. |
| `rootless`                   | Boolean      | `false`                                                      | 以非 root 用户运行容器。参见[无 root 模式](https://docs.docker.com/build/builders/drivers/kubernetes/#rootless-mode)。 Run the container as a non-root user. See [rootless mode](https://docs.docker.com/build/builders/drivers/kubernetes/#rootless-mode). |
| `loadbalance`                | String       | `sticky`                                                     | 负载均衡策略（`sticky` 或 `random`）。如果设置为 `sticky`，则 Pod 是根据上下文路径的哈希值选择的。 Load-balancing strategy (`sticky` or `random`). If set to `sticky`, the pod is chosen using the hash of the context path. |
| `qemu.install`               | Boolean      | `false`                                                      | 安装 QEMU 模拟以支持多平台。参见 [QEMU](https://docs.docker.com/build/builders/drivers/kubernetes/#qemu)。 Install QEMU emulation for multi platforms support. See [QEMU](https://docs.docker.com/build/builders/drivers/kubernetes/#qemu). |
| `qemu.image`                 | String       | `tonistiigi/binfmt:latest`                                   | 设置 QEMU 模拟镜像。参见 [QEMU](https://docs.docker.com/build/builders/drivers/kubernetes/#qemu)。 Sets the QEMU emulation image. See [QEMU](https://docs.docker.com/build/builders/drivers/kubernetes/#qemu). |

## 扩容 BuildKit - Scaling BuildKit

One of the main advantages of the Kubernetes driver is that you can scale the number of builder replicas up and down to handle increased build load. Scaling is configurable using the following driver options:

​	Kubernetes 驱动的一个主要优点是，您可以调整构建器副本的数量以应对增加的构建负载。扩容可以通过以下驱动选项配置：

- `replicas=N`

  This scales the number of BuildKit pods to the desired size. By default, it only creates a single pod. Increasing the number of replicas lets you take advantage of multiple nodes in your cluster.

  ​	将 BuildKit Pod 数量扩展到所需大小。默认情况下，仅创建一个 Pod。增加副本数量可以利用集群中的多个节点。

- `requests.cpu`, `requests.memory`, `requests.ephemeral-storage`, `limits.cpu`, `limits.memory`, `limits.ephemeral-storage`

  These options allow requesting and limiting the resources available to each BuildKit pod according to the official Kubernetes documentation [here](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/).
  
  ​	这些选项允许根据官方 Kubernetes 文档[此处](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)的说明请求和限制每个 BuildKit Pod 可用的资源。

For example, to create 4 replica BuildKit pods:

​	例如，创建 4 个 BuildKit Pod 副本：

```console
$ docker buildx create \
  --bootstrap \
  --name=kube \
  --driver=kubernetes \
  --driver-opt=namespace=buildkit,replicas=4
```

Listing the pods, you get this:

​	列出 Pod，您将看到如下内容：





```console
$ kubectl -n buildkit get deployments
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
kube0   4/4     4            4           8s

$ kubectl -n buildkit get pods
NAME                     READY   STATUS    RESTARTS   AGE
kube0-6977cdcb75-48ld2   1/1     Running   0          8s
kube0-6977cdcb75-rkc6b   1/1     Running   0          8s
kube0-6977cdcb75-vb4ks   1/1     Running   0          8s
kube0-6977cdcb75-z4fzs   1/1     Running   0          8s
```

Additionally, you can use the `loadbalance=(sticky|random)` option to control the load-balancing behavior when there are multiple replicas. `random` selects random nodes from the node pool, providing an even workload distribution across replicas. `sticky` (the default) attempts to connect the same build performed multiple times to the same node each time, ensuring better use of local cache.

​	此外，您可以使用 `loadbalance=(sticky|random)` 选项在存在多个副本时控制负载均衡行为。`random` 从节点池中随机选择节点，使工作负载均匀分布在副本之间。`sticky`（默认值）尝试将多次执行的同一构建连接到同一个节点，以更好地利用本地缓存。

For more information on scalability, see the options for [`docker buildx create`](https://docs.docker.com/reference/cli/docker/buildx/create/#driver-opt).

​	有关可扩展性的更多信息，请参阅 [`docker buildx create`](https://docs.docker.com/reference/cli/docker/buildx/create/#driver-opt) 选项。

## 节点分配 Node assignment

The Kubernetes driver allows you to control the scheduling of BuildKit pods using the `nodeSelector` and `tolerations` driver options. You can also set the `schedulername` option if you want to use a custom scheduler altogether.

​	Kubernetes 驱动允许您使用 `nodeSelector` 和 `tolerations` 驱动选项控制 BuildKit Pod 的调度。如果想使用自定义调度器，还可以设置 `schedulername` 选项。

You can use the `annotations` and `labels` driver options to apply additional metadata to the deployments and pods that's hosting your builders.

​	您可以使用 `annotations` 和 `labels` 驱动选项为托管构建器的部署和 Pod 添加额外的元数据。

The value of the `nodeSelector` parameter is a comma-separated string of key-value pairs, where the key is the node label and the value is the label text. For example: `"nodeselector=kubernetes.io/arch=arm64"`

​	`nodeSelector` 参数的值是用逗号分隔的键值对字符串，其中键是节点标签，值是标签文本。例如：`"nodeselector=kubernetes.io/arch=arm64"`

The `tolerations` parameter is a semicolon-separated list of taints. It accepts the same values as the Kubernetes manifest. Each `tolerations` entry specifies a taint key and the value, operator, or effect. For example: `"tolerations=key=foo,value=bar;key=foo2,operator=exists;key=foo3,effect=NoSchedule"`

​	`tolerations` 参数是由分号分隔的容忍度列表。它接受与 Kubernetes 清单相同的值。每个 `tolerations` 条目指定一个容忍度的键、值、运算符或效果。例如：`"tolerations=key=foo,value=bar;key=foo2,operator=exists;key=foo3,effect=NoSchedule"`

These options accept CSV-delimited strings as values. Due to quoting rules for shell commands, you must wrap the values in single quotes. You can even wrap all of `--driver-opt` in single quotes, for example:

​	这些选项接受 CSV 分隔的字符串作为值。由于 shell 命令的引号规则，您必须将这些值用单引号包裹起来。您甚至可以将整个 `--driver-opt` 包裹在单引号中，例如：



```console
$ docker buildx create \
  --bootstrap \
  --name=kube \
  --driver=kubernetes \
  '--driver-opt="nodeselector=label1=value1,label2=value2","tolerations=key=key1,value=value1"'
```

## 多平台构建 Multi-platform builds

The Kubernetes driver has support for creating [multi-platform images]({{< ref "/manuals/DockerBuild/Building/Multi-platform" >}}), either using QEMU or by leveraging the native architecture of nodes.

​	Kubernetes 驱动支持使用 QEMU 或利用节点的原生架构创建[多平台镜像]({{< ref "/manuals/DockerBuild/Building/Multi-platform" >}})。

### QEMU

Like the `docker-container` driver, the Kubernetes driver also supports using [QEMU](https://www.qemu.org/) (user mode) to build images for non-native platforms. Include the `--platform` flag and specify which platforms you want to output to.

​	与 `docker-container` 驱动一样，Kubernetes 驱动也支持使用 [QEMU](https://www.qemu.org/)（用户模式）为非原生平台构建镜像。包含 `--platform` 标志并指定您想要输出的平台。

For example, to build a Linux image for `amd64` and `arm64`:

​	例如，构建一个针对 `amd64` 和 `arm64` 的 Linux 镜像：

```console
$ docker buildx build \
  --builder=kube \
  --platform=linux/amd64,linux/arm64 \
  -t <user>/<image> \
  --push .
```

> **Warning**
>
> 
>
> QEMU performs full-CPU emulation of non-native platforms, which is much slower than native builds. Compute-heavy tasks like compilation and compression/decompression will likely take a large performance hit.
>
> ​	QEMU 会对非原生平台执行完全的 CPU 仿真，比原生构建速度慢得多。编译、压缩/解压缩等计算密集型任务的性能可能会受到显著影响。

Using a custom BuildKit image or invoking non-native binaries in builds may require that you explicitly turn on QEMU using the `qemu.install` option when creating the builder:

​	使用自定义 BuildKit 镜像或在构建中调用非原生二进制文件时，可能需要在创建构建器时显式开启 QEMU，可以通过设置 `qemu.install` 选项来实现：

```console
$ docker buildx create \
  --bootstrap \
  --name=kube \
  --driver=kubernetes \
  --driver-opt=namespace=buildkit,qemu.install=true
```

### Native

If you have access to cluster nodes of different architectures, the Kubernetes driver can take advantage of these for native builds. To do this, use the `--append` flag of `docker buildx create`.

​	如果可以访问不同架构的集群节点，Kubernetes 驱动可以利用这些节点进行原生构建。为此，使用 `docker buildx create` 的 `--append` 标志。

First, create your builder with explicit support for a single architecture, for example `amd64`:

​	首先，创建一个明确支持单一架构的构建器，例如 `amd64`：

```console
$ docker buildx create \
  --bootstrap \
  --name=kube \
  --driver=kubernetes \
  --platform=linux/amd64 \
  --node=builder-amd64 \
  --driver-opt=namespace=buildkit,nodeselector="kubernetes.io/arch=amd64"
```

This creates a Buildx builder named `kube`, containing a single builder node named `builder-amd64`. Assigning a node name using `--node` is optional. Buildx generates a random node name if you don't provide one.

​	此命令创建一个名为 `kube` 的 Buildx 构建器，包含一个名为 `builder-amd64` 的构建器节点。使用 `--node` 指定节点名称是可选的。如果未提供，Buildx 会生成一个随机节点名称。

Note that the Buildx concept of a node isn't the same as the Kubernetes concept of a node. A Buildx node in this case could connect multiple Kubernetes nodes of the same architecture together.

​	注意，Buildx 的节点概念与 Kubernetes 的节点概念不同。在此情况下，Buildx 节点可以连接同一架构的多个 Kubernetes 节点。

With the `kube` builder created, you can now introduce another architecture into the mix using `--append`. For example, to add `arm64`:

​	创建 `kube` 构建器后，您可以使用 `--append` 引入另一个架构。例如，添加 `arm64`：





```console
$ docker buildx create \
  --append \
  --bootstrap \
  --name=kube \
  --driver=kubernetes \
  --platform=linux/arm64 \
  --node=builder-arm64 \
  --driver-opt=namespace=buildkit,nodeselector="kubernetes.io/arch=arm64"
```

Listing your builders shows both nodes for the `kube` builder:

​	列出构建器，显示 `kube` 构建器的两个节点：





```console
$ docker buildx ls
NAME/NODE       DRIVER/ENDPOINT                                         STATUS   PLATFORMS
kube            kubernetes
  builder-amd64 kubernetes:///kube?deployment=builder-amd64&kubeconfig= running  linux/amd64*, linux/amd64/v2, linux/amd64/v3, linux/386
  builder-arm64 kubernetes:///kube?deployment=builder-arm64&kubeconfig= running  linux/arm64*
```

You can now build multi-arch `amd64` and `arm64` images, by specifying those platforms together in your build command:

​	现在，您可以通过在构建命令中同时指定这些平台来构建 `amd64` 和 `arm64` 的多架构镜像：





```console
$ docker buildx build --builder=kube --platform=linux/amd64,linux/arm64 -t <user>/<image> --push .
```

You can repeat the `buildx create --append` command for as many architectures that you want to support.

​	您可以对支持的所有架构重复执行 `buildx create --append` 命令。

## Rootless mode

The Kubernetes driver supports rootless mode. For more information on how rootless mode works, and it's requirements, see [here](https://github.com/moby/buildkit/blob/master/docs/rootless.md).

​	Kubernetes 驱动支持无 root 模式。有关无 root 模式的工作原理及其要求的更多信息，请参阅[此处](https://github.com/moby/buildkit/blob/master/docs/rootless.md)。

To turn it on in your cluster, you can use the `rootless=true` driver option:

​	在集群中启用此模式，可以使用 `rootless=true` 驱动选项：





```console
$ docker buildx create \
  --name=kube \
  --driver=kubernetes \
  --driver-opt=namespace=buildkit,rootless=true
```

This will create your pods without `securityContext.privileged`.

​	这将创建不带 `securityContext.privileged` 的 Pod。

Requires Kubernetes version 1.19 or later. Using Ubuntu as the host kernel is recommended.

​	需要 Kubernetes 版本 1.19 或更高版本。建议使用 Ubuntu 作为主机内核。

## 示例：在 Kubernetes 中创建 Buildx 构建器 Example: Creating a Buildx builder in Kubernetes

This guide shows you how to:

​	本指南展示如何：

- Create a namespace for your Buildx resources
  - 为 Buildx 资源创建命名空间

- Create a Kubernetes builder.
  - 创建 Kubernetes 构建器

- List the available builders
  - 列出可用的构建器

- Build an image using your Kubernetes builders
  - 使用 Kubernetes 构建器构建镜像


前置条件： Prerequisites:

- You have an existing Kubernetes cluster. If you don't already have one, you can follow along by installing [minikube](https://minikube.sigs.k8s.io/docs/).
  - 您拥有现有的 Kubernetes 集群。如果没有，可以通过安装 [minikube](https://minikube.sigs.k8s.io/docs/) 跟随操作。

- The cluster you want to connect to is accessible via the `kubectl` command, with the `KUBECONFIG` environment variable [set appropriately](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/#set-the-kubeconfig-environment-variable) if necessary.
  - 您要连接的集群可以通过 `kubectl` 命令访问，并且 `KUBECONFIG` 环境变量已[适当设置](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/#set-the-kubeconfig-environment-variable)。


1. Create a `buildkit` namespace. 创建 `buildkit` 命名空间。

   Creating a separate namespace helps keep your Buildx resources separate from other resources in the cluster.

   创建单独的命名空间有助于将 Buildx 资源与集群中的其他资源分离。

   ```console
   $ kubectl create namespace buildkit
   namespace/buildkit created
   ```

2. Create a new builder with the Kubernetes driver:

   使用 Kubernetes 驱动创建新构建器：

   ```console
   $ docker buildx create \
     --bootstrap \
     --name=kube \
     --driver=kubernetes \
     --driver-opt=namespace=buildkit
   ```

   > **Note**
   >
   > 
   >
   > Remember to specify the namespace in driver options.
   >
   > ​	记得在驱动选项中指定命名空间。

3. List available builders using `docker buildx ls`

   使用 `docker buildx ls` 列出可用构建器

   ```console
   $ docker buildx ls
   NAME/NODE                DRIVER/ENDPOINT STATUS  PLATFORMS
   kube                     kubernetes
     kube0-6977cdcb75-k9h9m                 running linux/amd64, linux/amd64/v2, linux/amd64/v3, linux/386
   default *                docker
     default                default         running linux/amd64, linux/386
   ```

4. Inspect the running pods created by the build driver with `kubectl`.

   使用 `kubectl` 检查由构建驱动创建的运行中的 Pod。

   ```console
   $ kubectl -n buildkit get deployments
   NAME    READY   UP-TO-DATE   AVAILABLE   AGE
   kube0   1/1     1            1           32s
   
   $ kubectl -n buildkit get pods
   NAME                     READY   STATUS    RESTARTS   AGE
   kube0-6977cdcb75-k9h9m   1/1     Running   0          32s
   ```

   The build driver creates the necessary resources on your cluster in the specified namespace (in this case, `buildkit`), while keeping your driver configuration locally.

   ​	构建驱动在指定的命名空间（本例中为 `buildkit`）中创建集群所需的资源，同时保持本地的驱动配置。

5. Use your new builder by including the `--builder` flag when running buildx commands. For example: :

   通过在运行 buildx 命令时包含 `--builder` 标志来使用新构建器。例如：

   ```console
   # Replace <registry> with your Docker username
   # and <image> with the name of the image you want to build
   docker buildx build \
     --builder=kube \
     -t <registry>/<image> \
     --push .
   ```

That's it: you've now built an image from a Kubernetes pod, using Buildx.

​	完成了：您现在已使用 Buildx 从 Kubernetes Pod 中构建了一个镜像。

## 进一步阅读 Further reading

For more information on the Kubernetes driver, see the [buildx reference](https://docs.docker.com/reference/cli/docker/buildx/create/#driver).

​	有关 Kubernetes 驱动的更多信息，请参阅 [buildx 参考](https://docs.docker.com/reference/cli/docker/buildx/create/#driver)。
