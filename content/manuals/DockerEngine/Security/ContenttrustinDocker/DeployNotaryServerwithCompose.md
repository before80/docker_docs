+++
title = "Deploy Notary Server with Compose"
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

# Deploy Notary Server with Compose

The easiest way to deploy Notary Server is by using Docker Compose. To follow the procedure on this page, you must have already [installed Docker Compose]({{< ref "/manuals/DockerCompose/Install" >}}).

1. Clone the Notary repository.

   

   ```consolse
   $ git clone https://github.com/theupdateframework/notary.git
   ```

2. Build and start Notary Server with the sample certificates.

   

   ```consolse
   $ docker compose up -d 
   ```

   For more detailed documentation about how to deploy Notary Server, see the [instructions to run a Notary service](https://github.com/theupdateframework/notary/blob/master/docs/running_a_service.md) as well as [the Notary repository](https://github.com/theupdateframework/notary) for more information.

3. Make sure that your Docker or Notary client trusts Notary Server's certificate before you try to interact with the Notary server.

See the instructions for [Docker](https://docs.docker.com/reference/cli/docker/#notary) or for [Notary](https://github.com/docker/notary#using-notary) depending on which one you are using.

## If you want to use Notary in production

Check back here for instructions after Notary Server has an official stable release. To get a head start on deploying Notary in production, see [the Notary repository](https://github.com/theupdateframework/notary).
