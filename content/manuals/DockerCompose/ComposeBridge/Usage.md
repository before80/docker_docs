+++
title = "Usage"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/compose/bridge/usage/](https://docs.docker.com/compose/bridge/usage/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use the default Compose Bridge transformation - 使用默认的 Compose Bridge 转换

**Experimental 实验性功能**

Compose Bridge is an [Experimental](https://docs.docker.com/release-lifecycle/#experimental) product.

​	Compose Bridge 是一个[实验性](https://docs.docker.com/release-lifecycle/#experimental)产品。

Compose Bridge supplies an out-of-the box transformation for your Compose configuration file. Based on an arbitrary `compose.yaml` file, Compose Bridge produces:

​	Compose Bridge 为您的 Compose 配置文件提供了开箱即用的转换方案。基于任意的 `compose.yaml` 文件，Compose Bridge 生成以下内容：

- A [Namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) so all your resources are isolated and don't conflict with resources from other deployments.
  - 一个[命名空间 (Namespace)](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)，确保所有资源被隔离，不会与其他部署的资源冲突。

- A [ConfigMap](https://kubernetes.io/docs/concepts/configuration/configmap/) with an entry for each and every [config]({{< ref "/reference/Composefilereference/Configstop-levelelements" >}}) resource in your Compose application.
  - 包含每个 config 资源条目的 [ConfigMap](https://kubernetes.io/docs/concepts/configuration/configmap/)。

- [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) for application services. This ensures that the specified number of instances of your application are maintained in the Kubernetes cluster.
  - 为应用服务创建的 [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)，以保证在 Kubernetes 集群中维持指定数量的应用实例。

- [Services](https://kubernetes.io/docs/concepts/services-networking/service/) for ports exposed by your services, used for service-to-service communication.
  - 为服务间通信提供的[服务 (Service)](https://kubernetes.io/docs/concepts/services-networking/service/)，用于暴露服务的端口。

- [Services](https://kubernetes.io/docs/concepts/services-networking/service/) for ports published by your services, with type `LoadBalancer` so that Docker Desktop will also expose the same port on the host.
  - 为服务发布的端口提供的[服务 (Service)](https://kubernetes.io/docs/concepts/services-networking/service/)，类型为 `LoadBalancer`，以便 Docker Desktop 也能在主机上暴露相同端口。

- [Network policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/) to replicate the networking topology defined in your `compose.yaml` file.
  - [网络策略 (Network policies)](https://kubernetes.io/docs/concepts/services-networking/network-policies/)，以复制 `compose.yaml` 文件中定义的网络拓扑。

- [PersistentVolumeClaims](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) for your volumes, using `hostpath` storage class so that Docker Desktop manages volume creation.
  - 为您的数据卷创建的 [PersistentVolumeClaims](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)，使用 `hostpath` 存储类以便 Docker Desktop 管理卷的创建。

- [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) with your secret encoded. This is designed for local use in a testing environment.
  - 编码后的[密钥 (Secrets)](https://kubernetes.io/docs/concepts/configuration/secret/)，专为本地测试环境设计。


It also supplies a Kustomize overlay dedicated to Docker Desktop with:

​	它还提供了专为 Docker Desktop 设计的 Kustomize 覆盖层：

- `Loadbalancer` for services which need to expose ports on host.
  - 为需要在主机上公开端口的服务提供 `Loadbalancer`。

- A `PersistentVolumeClaim` to use the Docker Desktop storage provisioner `desktop-storage-provisioner` to handle volume provisioning more effectively.
  - 使用 Docker Desktop 存储提供程序 `desktop-storage-provisioner` 的 `PersistentVolumeClaim` 以更高效地处理卷创建。

- A Kustomize file to link all the resources together.
  - 一个 Kustomize 文件，用于将所有资源链接在一起。


## 使用默认的 Compose Bridge 转换 Use the default Compose Bridge transformation

To use the default transformation run the following command:

​	要使用默认的转换，运行以下命令：

```console
$ compose-bridge convert
```

Compose looks for a `compose.yaml` file inside the current directory and then converts it.

​	Compose 会在当前目录中查找 `compose.yaml` 文件并进行转换。

The following output is displayed

​	以下是显示的输出：

```console
$ compose-bridge convert -f compose.yaml 
Kubernetes resource api-deployment.yaml created
Kubernetes resource db-deployment.yaml created
Kubernetes resource web-deployment.yaml created
Kubernetes resource api-expose.yaml created
Kubernetes resource db-expose.yaml created
Kubernetes resource web-expose.yaml created
Kubernetes resource 0-avatars-namespace.yaml created
Kubernetes resource default-network-policy.yaml created
Kubernetes resource private-network-policy.yaml created
Kubernetes resource public-network-policy.yaml created
Kubernetes resource db-db_data-persistentVolumeClaim.yaml created
Kubernetes resource api-service.yaml created
Kubernetes resource web-service.yaml created
Kubernetes resource kustomization.yaml created
Kubernetes resource db-db_data-persistentVolumeClaim.yaml created
Kubernetes resource api-service.yaml created
Kubernetes resource web-service.yaml created
Kubernetes resource kustomization.yaml created
```

These files are then stored within your project in the `/out` folder.

​	这些文件会存储在项目的 `/out` 文件夹中。

The Kubernetes manifests can then be used to run the application on Kubernetes using the standard deployment command `kubectl apply -k out/overlays/desktop/`.

​	然后，您可以使用标准部署命令 `kubectl apply -k out/overlays/desktop/` 在 Kubernetes 上运行应用程序。

> **Note**
>
> 
>
> Make sure you have enabled Kubernetes in Docker Desktop before you deploy your Compose Bridge transformations.
>
> ​	确保在部署 Compose Bridge 转换之前，已在 Docker Desktop 中启用 Kubernetes。

If you want to convert a `compose.yaml` file that is located in another directory, you can run:

​	如果要转换位于其他目录中的 `compose.yaml` 文件，可以运行：

```console
$ compose-bridge convert -f <path-to-file>/compose.yaml 
```

To see all available flags, run:

​	查看所有可用的标志，运行：

```console
$ compose-bridge convert --help
```

> **Tip**
>
> 
>
> You can now convert and deploy your Compose project to a Kubernetes cluster from the Compose file viewer.
>
> ​	您现在可以直接从 Compose 文件查看器将 Compose 项目转换并部署到 Kubernetes 集群。
>
> Make sure you are signed in to your Docker account, navigate to your container in the **Containers** view, and in the top-right corner select **View configurations** and then **Convert and Deploy to Kubernetes**.
>
> ​	确保已登录 Docker 账户，导航到 **Containers** 视图中的容器，在右上角选择 **View configurations**，然后选择 **Convert and Deploy to Kubernetes**。

## What's next?

- [Explore how you can customize Compose Bridge 探索如何自定义 Compose Bridge]({{< ref "/manuals/DockerCompose/ComposeBridge/Customize" >}})
- [Explore the advanced integration 探索高级集成]({{< ref "/manuals/DockerCompose/ComposeBridge/Advanced" >}})
