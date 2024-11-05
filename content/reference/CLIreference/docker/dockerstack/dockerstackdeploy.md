+++
title = "docker stack deploy"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/stack/deploy/](https://docs.docker.com/reference/cli/docker/stack/deploy/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker stack deploy

| Description | Deploy a new stack or update an existing stack |
| :---------- | ---------------------------------------------- |
| Usage       | `docker stack deploy [OPTIONS] STACK`          |
| Aliases     | `docker stack up`                              |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 编排器。

## Description

Create and update a stack from a `compose` file on the swarm.

​	从 `compose` 文件在 Swarm 上创建和更新栈。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理器和工作节点，请参阅文档中的 Swarm 模式部分。

## Options

| Option                                                       | Default  | Description                                                  |
| ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| [`-c, --compose-file`](https://docs.docker.com/reference/cli/docker/stack/deploy/#compose-file) |          | API 1.25+ Compose 文件路径，或使用 `-` 从标准输入读取 API 1.25+ Path to a Compose file, or `-` to read from stdin |
| `-d, --detach`                                               | `true`   | 立即退出，而不是等待栈服务收敛 Exit immediately instead of waiting for the stack services to converge |
| `--prune`                                                    |          | API 1.27+ 删除不再引用的服务 API 1.27+ Prune services that are no longer referenced |
| `-q, --quiet`                                                |          | 抑制进度输出 Suppress progress output                        |
| `--resolve-image`                                            | `always` | API 1.30+ 查询注册表以解析镜像摘要和支持的平台（`always`、`changed`、`never`） API 1.30+ Query the registry to resolve image digest and supported platforms (`always`, `changed`, `never`) |
| `--with-registry-auth`                                       |          | 将注册表认证信息发送到 Swarm 代理 Send registry authentication details to Swarm agents |

## Examples

### Compose file (--compose-file)

The `deploy` command supports Compose file version `3.0` and above.

​	`deploy` 命令支持 Compose 文件版本 `3.0` 及以上。



```console
$ docker stack deploy --compose-file docker-compose.yml vossibility

Ignoring unsupported options: links

Creating network vossibility_vossibility
Creating network vossibility_default
Creating service vossibility_nsqd
Creating service vossibility_logstash
Creating service vossibility_elasticsearch
Creating service vossibility_kibana
Creating service vossibility_ghollector
Creating service vossibility_lookupd
```

The Compose file can also be provided as standard input with `--compose-file -`:

​	Compose 文件也可以通过标准输入提供，使用 `--compose-file -`：



```console
$ cat docker-compose.yml | docker stack deploy --compose-file - vossibility

Ignoring unsupported options: links

Creating network vossibility_vossibility
Creating network vossibility_default
Creating service vossibility_nsqd
Creating service vossibility_logstash
Creating service vossibility_elasticsearch
Creating service vossibility_kibana
Creating service vossibility_ghollector
Creating service vossibility_lookupd
```

If your configuration is split between multiple Compose files, e.g. a base configuration and environment-specific overrides, you can provide multiple `--compose-file` flags.

​	如果配置分为多个 Compose 文件（例如，基本配置和特定环境覆盖），可以提供多个 `--compose-file` 标志。



```console
$ docker stack deploy --compose-file docker-compose.yml -c docker-compose.prod.yml vossibility

Ignoring unsupported options: links

Creating network vossibility_vossibility
Creating network vossibility_default
Creating service vossibility_nsqd
Creating service vossibility_logstash
Creating service vossibility_elasticsearch
Creating service vossibility_kibana
Creating service vossibility_ghollector
Creating service vossibility_lookupd
```

You can verify that the services were correctly created:

​	你可以验证服务是否已正确创建。

```console
$ docker service ls

ID            NAME                               MODE        REPLICAS  IMAGE
29bv0vnlm903  vossibility_lookupd                replicated  1/1       nsqio/nsq@sha256:eeba05599f31eba418e96e71e0984c3dc96963ceb66924dd37a47bf7ce18a662
4awt47624qwh  vossibility_nsqd                   replicated  1/1       nsqio/nsq@sha256:eeba05599f31eba418e96e71e0984c3dc96963ceb66924dd37a47bf7ce18a662
4tjx9biia6fs  vossibility_elasticsearch          replicated  1/1       elasticsearch@sha256:12ac7c6af55d001f71800b83ba91a04f716e58d82e748fa6e5a7359eed2301aa
7563uuzr9eys  vossibility_kibana                 replicated  1/1       kibana@sha256:6995a2d25709a62694a937b8a529ff36da92ebee74bafd7bf00e6caf6db2eb03
9gc5m4met4he  vossibility_logstash               replicated  1/1       logstash@sha256:2dc8bddd1bb4a5a34e8ebaf73749f6413c101b2edef6617f2f7713926d2141fe
axqh55ipl40h  vossibility_vossibility-collector  replicated  1/1       icecrime/vossibility-collector@sha256:f03f2977203ba6253988c18d04061c5ec7aab46bca9dfd89a9a1fa4500989fba
```

