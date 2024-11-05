+++
title = "docker swarm update"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/swarm/update/](https://docs.docker.com/reference/cli/docker/swarm/update/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker swarm update

| Description | Update the swarm                |
| :---------- | ------------------------------- |
| Usage       | `docker swarm update [OPTIONS]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 协调器。

## Description

Updates a swarm with new parameter values.

​	使用新的参数值更新 Swarm。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理器和工作节点，请参阅 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                   | Default     | Description                                                  |
| ------------------------ | ----------- | ------------------------------------------------------------ |
| `--autolock`             |             | 更改管理节点自动锁定设置(true\|false) Change manager autolocking setting (true\|false) |
| `--cert-expiry`          | `2160h0m0s` | 节点证书的有效期 Validity period for node certificates (ns\|us\|ms\|s\|m\|h) |
| `--dispatcher-heartbeat` | `5s`        | 调度器心跳周期 Dispatcher heartbeat period (ns\|us\|ms\|s\|m\|h) |
| `--external-ca`          |             | 一个或多个证书签名端点的规格 Specifications of one or more certificate signing endpoints |
| `--max-snapshots`        |             | API 1.25+ 要保留的额外 Raft 快照数量 API 1.25+ Number of additional Raft snapshots to retain |
| `--snapshot-interval`    | `10000`     | API 1.25+ Raft 快照之间的日志条目数 API 1.25+ Number of log entries between Raft snapshots |
| `--task-history-limit`   | `5`         | 任务历史保留限制 Task history retention limit                |

## Examples



```console
$ docker swarm update --cert-expiry 720h
```
