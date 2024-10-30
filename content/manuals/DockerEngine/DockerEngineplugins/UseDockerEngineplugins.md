+++
title = "使用 Docker 引擎插件"
date = 2024-10-23T14:54:40+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/extend/legacy_plugins/](https://docs.docker.com/engine/extend/legacy_plugins/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use Docker Engine plugins - 使用 Docker 引擎插件

This document describes the Docker Engine plugins generally available in Docker Engine. To view information on plugins managed by Docker, refer to [Docker Engine plugin system]({{< ref "/manuals/DockerEngine/DockerEngineplugins" >}}).

​	本文档描述了 Docker 引擎中一般可用的插件。要查看 Docker 管理的插件信息，请参见 [Docker 引擎插件系统]({{< ref "/manuals/DockerEngine/DockerEngineplugins" >}})。

You can extend the capabilities of the Docker Engine by loading third-party plugins. This page explains the types of plugins and provides links to several volume and network plugins for Docker.

​	您可以通过加载第三方插件来扩展 Docker 引擎的功能。此页面解释了插件的类型，并提供了一些用于 Docker 的卷和网络插件的链接。

## 插件类型 Types of plugins

Plugins extend Docker's functionality. They come in specific types. For example, a [volume plugin]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockervolumeplugins" >}}) might enable Docker volumes to persist across multiple Docker hosts and a [network plugin]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockernetworkdriverplugins" >}}) might provide network plumbing.

​	插件扩展了 Docker 的功能。它们具有特定的类型。例如，一个[卷插件]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockervolumeplugins" >}})可以使 Docker 卷在多个 Docker 主机之间持久存在，而一个[网络插件]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockernetworkdriverplugins" >}})可以提供网络支持。

Currently Docker supports authorization, volume and network driver plugins. In the future it will support additional plugin types.

​	目前 Docker 支持授权、卷和网络驱动插件。未来将支持更多插件类型。

## 安装插件 Installing a plugin

Follow the instructions in the plugin's documentation.

​	请按照插件文档中的说明进行操作。

## 查找插件 Finding a plugin

The sections below provide an overview of available third-party plugins.

​	以下部分概述了可用的第三方插件。

### 网络插件 Network plugins

| Plugin                                                       | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [Contiv Networking](https://github.com/contiv/netplugin)     | 一个开源网络插件，为多租户微服务部署提供基础架构和安全策略，同时为非容器工作负载提供与物理网络的集成。Contiv Networking 实现了 Docker 1.9 及更高版本中提供的远程驱动程序和 IPAM API。An open source network plugin to provide infrastructure and security policies for a multi-tenant micro services deployment, while providing an integration to physical network for non-container workload. Contiv Networking implements the remote driver and IPAM APIs available in Docker 1.9 onwards. |
| [Kuryr Network Plugin](https://github.com/openstack/kuryr)   | 作为 OpenStack Kuryr 项目的一部分开发的网络插件，利用 OpenStack 网络服务 Neutron 实现 Docker 网络（libnetwork）远程驱动 API，并包含 IPAM 驱动程序。A network plugin is developed as part of the OpenStack Kuryr project and implements the Docker networking (libnetwork) remote driver API by utilizing Neutron, the OpenStack networking service. It includes an IPAM driver as well. |
| [Kathará Network Plugin](https://github.com/KatharaFramework/NetworkPlugin) | Docker 网络插件，用于 Kathará，这是一个开源的基于容器的网络仿真系统，用于展示交互式演示/课程、在沙箱环境中测试生产网络或开发新网络协议。Docker Network Plugin used by Kathará, an open source container-based network emulation system for showing interactive demos/lessons, testing production networks in a sandbox environment, or developing new network protocols. |

### 卷插件 Volume plugins

| Plugin                                                       | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [Azure File Storage plugin](https://github.com/Azure/azurefile-dockervolumedriver) | 使用 SMB 3.0 协议将 Microsoft [Azure File Storage](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/) 共享挂载为 Docker 容器中的卷。[了解更多](https://azure.microsoft.com/blog/persistent-docker-volumes-with-azure-file-storage/)。 Lets you mount Microsoft [Azure File Storage](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/) shares to Docker containers as volumes using the SMB 3.0 protocol. [Learn more](https://azure.microsoft.com/blog/persistent-docker-volumes-with-azure-file-storage/). |
| [BeeGFS Volume Plugin](https://github.com/RedCoolBeans/docker-volume-beegfs) | 一个开源的卷插件，用于在 BeeGFS 并行文件系统中创建持久卷。An open source volume plugin to create persistent volumes in a BeeGFS parallel file system. |
| [Blockbridge plugin](https://github.com/blockbridge/blockbridge-docker-volume) | 提供对基于容器的可扩展持久存储选项的访问，支持单主机和多主机 Docker 环境，具有租户隔离、自动配置、加密、安全删除、快照和 QoS 等功能。A volume plugin that provides access to an extensible set of container-based persistent storage options. It supports single and multi-host Docker environments with features that include tenant isolation, automated provisioning, encryption, secure deletion, snapshots and QoS. |
| [Contiv Volume Plugin](https://github.com/contiv/volplugin)  | 一个开源的卷插件，提供支持多租户的持久分布式存储，支持 Ceph 和 NFS。An open source volume plugin that provides multi-tenant, persistent, distributed storage with intent based consumption. It has support for Ceph and NFS. |
| [Convoy plugin](https://github.com/rancher/convoy)           | 支持多种存储后端的卷插件，包括 device mapper 和 NFS，是一个用 Go 编写的简单独立可执行文件，提供了支持供应商扩展（如快照、备份和恢复）的框架。A volume plugin for a variety of storage back-ends including device mapper and NFS. It's a simple standalone executable written in Go and provides the framework to support vendor-specific extensions such as snapshots, backups and restore. |
| [DigitalOcean Block Storage plugin](https://github.com/omallo/docker-volume-plugin-dostorage) | 将 DigitalOcean 的 [块存储解决方案](https://www.digitalocean.com/products/storage/) 集成到 Docker 生态系统中，自动将指定的块存储卷附加到 DigitalOcean droplet 并将卷内容提供给运行在该 droplet 上的 Docker 容器。Integrates DigitalOcean's [block storage solution](https://www.digitalocean.com/products/storage/) into the Docker ecosystem by automatically attaching a given block storage volume to a DigitalOcean droplet and making the contents of the volume available to Docker containers running on that droplet. |
| [DRBD plugin](https://www.drbd.org/en/supported-projects/docker) | 提供通过 [DRBD](https://www.drbd.org/) 复制的高可用存储，将写入 Docker 卷的数据在 DRBD 节点集群中进行复制。A volume plugin that provides highly available storage replicated by [DRBD](https://www.drbd.org/). Data written to the docker volume is replicated in a cluster of DRBD nodes. |
| [Flocker plugin](https://github.com/ScatterHQ/flocker)       | 提供多主机可移植卷的卷插件，允许在 Docker 中运行数据库等有状态的容器，并在多主机集群中移动它们。A volume plugin that provides multi-host portable volumes for Docker, enabling you to run databases and other stateful containers and move them around across a cluster of machines. |
| [Fuxi Volume Plugin](https://github.com/openstack/fuxi)      | 作为 OpenStack Kuryr 项目的一部分开发的卷插件，通过利用 OpenStack 块存储服务 Cinder 实现 Docker 卷插件 API。A volume plugin that is developed as part of the OpenStack Kuryr project and implements the Docker volume plugin API by utilizing Cinder, the OpenStack block storage service. |
| [gce-docker plugin](https://github.com/mcuadros/gce-docker)  | 一个卷插件，能够附加、格式化和挂载 Google Compute [持久磁盘](https://cloud.google.com/compute/docs/disks/persistent-disks)。A volume plugin able to attach, format and mount Google Compute [persistent-disks](https://cloud.google.com/compute/docs/disks/persistent-disks). |
| [GlusterFS plugin](https://github.com/calavera/docker-volume-glusterfs) | 一个使用 GlusterFS 管理多主机卷的卷插件。A volume plugin that provides multi-host volumes management for Docker using GlusterFS. |
| [Horcrux Volume Plugin](https://github.com/muthu-r/horcrux)  | 提供按需的版本控制数据访问的卷插件，是一个开源插件，用 Go 编写，支持 SCP、[Minio](https://www.minio.io/) 和 Amazon S3。A volume plugin that allows on-demand, version controlled access to your data. Horcrux is an open-source plugin, written in Go, and supports SCP, [Minio](https://www.minio.io/) and Amazon S3. |
| [HPE 3Par Volume Plugin](https://github.com/hpe-storage/python-hpedockerplugin/) | 支持 HPE 3Par 和 StoreVirtual iSCSI 存储阵列的卷插件。A volume plugin that supports HPE 3Par and StoreVirtual iSCSI storage arrays. |
| [Infinit volume plugin](https://infinit.sh/documentation/docker/volume-plugin) | 使得使用 Docker 挂载和管理 Infinit 卷更为简单的卷插件。A volume plugin that makes it easy to mount and manage Infinit volumes using Docker. |
| [IPFS Volume Plugin](https://github.com/vdemeester/docker-volume-ipfs) | 一个开源卷插件，允许使用 [IPFS](https://ipfs.io/) 文件系统作为卷。An open source volume plugin that allows using an [ipfs](https://ipfs.io/) filesystem as a volume. |
| [Keywhiz plugin](https://github.com/calavera/docker-volume-keywhiz) | 使用 Keywhiz 作为中央存储库提供凭据和秘密管理的插件。A plugin that provides credentials and secret management using Keywhiz as a central repository. |
| [Linode Volume Plugin](https://github.com/linode/docker-volume-linode) | 允许从 Linode 内部管理 Linode 块存储为 Docker 卷的插件。A plugin that adds the ability to manage Linode Block Storage as Docker Volumes from within a Linode. |
| [Local Persist Plugin](https://github.com/CWSpear/local-persist) | 扩展默认 `local` 驱动功能的卷插件，允许您在主机的任意位置指定挂载点，即使通过 `docker volume rm` 移除卷，文件也将始终持久存在。A volume plugin that extends the default `local` driver's functionality by allowing you specify a mountpoint anywhere on the host, which enables the files to *always persist*, even if the volume is removed via `docker volume rm`. |
| [NetApp Plugin](https://github.com/NetApp/netappdvp) (nDVP)  | 直接与 Docker 生态系统集成的卷插件，用于 NetApp 存储组合。nDVP 包支持从存储平台向 Docker 主机的存储资源配置和管理。A volume plugin that provides direct integration with the Docker ecosystem for the NetApp storage portfolio. The nDVP package supports the provisioning and management of storage resources from the storage platform to Docker hosts, with a robust framework for adding additional platforms in the future. |
| [Netshare plugin](https://github.com/ContainX/docker-volume-netshare) | 为 NFS 3/4、AWS EFS 和 CIFS 文件系统提供卷管理的卷插件。A volume plugin that provides volume management for NFS 3/4, AWS EFS and CIFS file systems. |
| [Nimble Storage Volume Plugin](https://scod.hpedev.io/docker_volume_plugins/hpe_nimble_storage/index.html) | 一个集成了 Nimble Storage Unified Flash Fabric 阵列的卷插件，支持自我配置的安全多租户卷和克隆。A volume plug-in that integrates with Nimble Storage Unified Flash Fabric arrays. The plug-in abstracts array volume capabilities to the Docker administrator to allow self-provisioning of secure multi-tenant volumes and clones. |
| [OpenStorage Plugin](https://github.com/libopenstorage/openstorage) | 一个集群感知的卷插件，为文件和块存储解决方案提供卷管理，具有 FUSE、NFS、NBD 和 EBS 等示例驱动程序。A cluster-aware volume plugin that provides volume management for file and block storage solutions. It implements a vendor neutral specification for implementing extensions such as CoS, encryption, and snapshots. It has example drivers based on FUSE, NFS, NBD and EBS to name a few. |
| [Portworx Volume Plugin](https://github.com/portworx/px-dev) | 将任何服务器转变为横向扩展的计算/存储节点的卷插件，提供容器细粒度的存储和跨节点的高可用卷。A volume plugin that turns any server into a scale-out converged compute/storage node, providing container granular storage and highly available volumes across any node, using a shared-nothing storage backend that works with any docker scheduler. |
| [Quobyte Volume Plugin](https://github.com/quobyte/docker-volume) | 将 Docker 连接到 [Quobyte](https://www.quobyte.com/containers) 数据中心文件系统的卷插件，一个通用的可扩展和容错的存储平台。A volume plugin that connects Docker to [Quobyte](https://www.quobyte.com/containers)'s data center file system, a general-purpose scalable and fault-tolerant storage platform. |
| [REX-Ray plugin](https://github.com/emccode/rexray)          | 一个用 Go 编写的卷插件，为多个平台（包括 VirtualBox、EC2、Google Compute Engine、OpenStack 和 EMC）提供高级存储功能。A volume plugin which is written in Go and provides advanced storage functionality for many platforms including VirtualBox, EC2, Google Compute Engine, OpenStack, and EMC. |
| [Virtuozzo Storage and Ploop plugin](https://github.com/virtuozzo/docker-volume-ploop) | 支持 Virtuozzo 存储分布式云文件系统和 ploop 设备的卷插件。A volume plugin with support for Virtuozzo Storage distributed cloud file system as well as ploop devices. |
| [VMware vSphere Storage Plugin](https://github.com/vmware/docker-volume-vsphere) | vSphere 的 Docker 卷驱动，帮助客户满足 vSphere 环境中 Docker 容器的持久存储需求。 Docker Volume Driver for vSphere enables customers to address persistent storage requirements for Docker containers in vSphere environments. |

### 授权插件Authorization plugins

| Plugin                                                       | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [Casbin AuthZ Plugin](https://github.com/casbin/casbin-authz-plugin) | 基于 [Casbin](https://github.com/casbin/casbin) 的授权插件，支持 ACL、RBAC、ABAC 等访问控制模型。可自定义访问控制模型，策略可以持久化到文件或数据库中。An authorization plugin based on [Casbin](https://github.com/casbin/casbin), which supports access control models like ACL, RBAC, ABAC. The access control model can be customized. The policy can be persisted into file or DB. |
| [HBM plugin](https://github.com/kassisol/hbm)                | 一个授权插件，可防止带有特定参数的命令执行。An authorization plugin that prevents from executing commands with certains parameters. |
| [Twistlock AuthZ Broker](https://github.com/twistlock/authz) | 一个基本的可扩展授权插件，可以直接在主机或容器中运行，允许您定义用户策略并在授权过程中评估这些策略。如果 Docker 守护进程使用 `--tlsverify` 启动，则提供基本授权（从证书的通用名中提取用户名）。A basic extendable authorization plugin that runs directly on the host or inside a container. This plugin allows you to define user policies that it evaluates during authorization. Basic authorization is provided if Docker daemon is started with the --tlsverify flag (username is extracted from the certificate common name). |

## 插件故障排除 Troubleshooting a plugin

If you are having problems with Docker after loading a plugin, ask the authors of the plugin for help. The Docker team may not be able to assist you.

​	如果加载插件后遇到 Docker 问题，请向插件作者寻求帮助。Docker 团队可能无法提供帮助。

## 编写插件 Writing a plugin

If you are interested in writing a plugin for Docker, or seeing how they work under the hood, see the [Docker plugins reference]({{< ref "/manuals/DockerEngine/DockerEngineplugins/DockerPluginAPI" >}}).

​	如果您有兴趣为 Docker 编写插件或想了解插件的工作原理，请参阅 [Docker 插件参考]({{< ref "/manuals/DockerEngine/DockerEngineplugins/DockerPluginAPI" >}})。

[Request changes](https://github.com/docker/docs/issues/new?template=doc_issue.yml&location=https%3a%2f%2fdocs.docker.com%2fengine%2fextend%2flegacy_plugins%2f&labels=status%2Ftriage)

