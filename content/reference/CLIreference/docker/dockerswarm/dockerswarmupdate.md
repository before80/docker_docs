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

## Description

Updates a swarm with new parameter values.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Options

| Option                   | Default     | Description                                                 |
| ------------------------ | ----------- | ----------------------------------------------------------- |
| `--autolock`             |             | Change manager autolocking setting (true\|false)            |
| `--cert-expiry`          | `2160h0m0s` | Validity period for node certificates (ns\|us\|ms\|s\|m\|h) |
| `--dispatcher-heartbeat` | `5s`        | Dispatcher heartbeat period (ns\|us\|ms\|s\|m\|h)           |
| `--external-ca`          |             | Specifications of one or more certificate signing endpoints |
| `--max-snapshots`        |             | API 1.25+ Number of additional Raft snapshots to retain     |
| `--snapshot-interval`    | `10000`     | API 1.25+ Number of log entries between Raft snapshots      |
| `--task-history-limit`   | `5`         | Task history retention limit                                |

## Examples



```console
$ docker swarm update --cert-expiry 720h
```
