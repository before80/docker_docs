+++
title = "使用 Compose 部署 Notary 服务器"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/trust/deploying_notary/](https://docs.docker.com/engine/security/trust/deploying_notary/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Deploy Notary Server with Compose - 使用 Compose 部署 Notary 服务器

The easiest way to deploy Notary Server is by using Docker Compose. To follow the procedure on this page, you must have already [installed Docker Compose]({{< ref "/manuals/DockerCompose/Install" >}}).

​	使用 Docker Compose 是部署 Notary 服务器的最简单方式。要遵循本页面的步骤，您必须已经[安装了 Docker Compose]({{< ref "/manuals/DockerCompose/Install" >}})。

1. Clone the Notary repository.

   克隆 Notary 仓库。

   ```consolse
   $ git clone https://github.com/theupdateframework/notary.git
   ```

2. Build and start Notary Server with the sample certificates.

   使用示例证书构建并启动 Notary 服务器。

   ```consolse
   $ docker compose up -d 
   ```

   For more detailed documentation about how to deploy Notary Server, see the [instructions to run a Notary service](https://github.com/theupdateframework/notary/blob/master/docs/running_a_service.md) as well as [the Notary repository](https://github.com/theupdateframework/notary) for more information.

   ​	有关如何部署 Notary 服务器的更详细文档，请参阅[运行 Notary 服务的说明](https://github.com/theupdateframework/notary/blob/master/docs/running_a_service.md)以及 [Notary 仓库](https://github.com/theupdateframework/notary)以获取更多信息。

3. Make sure that your Docker or Notary client trusts Notary Server's certificate before you try to interact with the Notary server. 在尝试与 Notary 服务器进行交互之前，请确保您的 Docker 或 Notary 客户端信任 Notary 服务器的证书。

See the instructions for [Docker](https://docs.docker.com/reference/cli/docker/#notary) or for [Notary](https://github.com/docker/notary#using-notary) depending on which one you are using.

​	请根据您使用的客户端查看[Docker](https://docs.docker.com/reference/cli/docker/#notary)或 [Notary](https://github.com/docker/notary#using-notary) 的相关说明。

## 如果您想在生产环境中使用 Notary If you want to use Notary in production

Check back here for instructions after Notary Server has an official stable release. To get a head start on deploying Notary in production, see [the Notary repository](https://github.com/theupdateframework/notary).

​	当 Notary 服务器发布官方稳定版本后，请查看此处的说明。若要在生产环境中提前部署 Notary，请参阅 [Notary 仓库](https://github.com/theupdateframework/notary)。

