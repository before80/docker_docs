+++
title = "docker node inspect"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/node/inspect/](https://docs.docker.com/reference/cli/docker/node/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker node inspect

| Description | Display detailed information on one or more nodes   |
| :---------- | --------------------------------------------------- |
| Usage       | `docker node inspect [OPTIONS] self|NODE [NODE...]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 协调器。

## Description

Returns information about a node. By default, this command renders all results in a JSON array. You can specify an alternate format to execute a given template for each result. Go's [text/template](https://pkg.go.dev/text/template) package describes all the details of the format.

​	返回有关节点的信息。默认情况下，该命令以 JSON 数组格式呈现所有结果。您可以指定其他格式来为每个结果执行指定的模板。Go 的 [text/template](https://pkg.go.dev/text/template) 包描述了格式的所有详细信息。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。有关管理节点和工作节点的更多信息，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --format`](https://docs.docker.com/reference/cli/docker/node/inspect/#format) |         | 使用自定义模板格式化输出：'json'：以 JSON 格式打印 'TEMPLATE'：使用指定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/   Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `--pretty`                                                   |         | 以人性化的格式打印信息 Print the information in a human friendly format |

## Examples

### 检查节点信息 Inspect a node



```console
$ docker node inspect swarm-manager
```



```json
[
  {
    "ID": "e216jshn25ckzbvmwlnh5jr3g",
    "Version": {
      "Index": 10
    },
    "CreatedAt": "2017-05-16T22:52:44.9910662Z",
    "UpdatedAt": "2017-05-16T22:52:45.230878043Z",
    "Spec": {
      "Role": "manager",
      "Availability": "active"
    },
    "Description": {
      "Hostname": "swarm-manager",
      "Platform": {
        "Architecture": "x86_64",
        "OS": "linux"
      },
      "Resources": {
        "NanoCPUs": 1000000000,
        "MemoryBytes": 1039843328
      },
      "Engine": {
        "EngineVersion": "17.06.0-ce",
        "Plugins": [
          {
            "Type": "Volume",
            "Name": "local"
          },
          {
            "Type": "Network",
            "Name": "overlay"
          },
          {
            "Type": "Network",
            "Name": "null"
          },
          {
            "Type": "Network",
            "Name": "host"
          },
          {
            "Type": "Network",
            "Name": "bridge"
          },
          {
            "Type": "Network",
            "Name": "overlay"
          }
        ]
      },
      "TLSInfo": {
        "TrustRoot": "-----BEGIN CERTIFICATE-----\nMIIBazCCARCgAwIBAgIUOzgqU4tA2q5Yv1HnkzhSIwGyIBswCgYIKoZIzj0EAwIw\nEzERMA8GA1UEAxMIc3dhcm0tY2EwHhcNMTcwNTAyMDAyNDAwWhcNMzcwNDI3MDAy\nNDAwWjATMREwDwYDVQQDEwhzd2FybS1jYTBZMBMGByqGSM49AgEGCCqGSM49AwEH\nA0IABMbiAmET+HZyve35ujrnL2kOLBEQhFDZ5MhxAuYs96n796sFlfxTxC1lM/2g\nAh8DI34pm3JmHgZxeBPKUURJHKWjQjBAMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMB\nAf8EBTADAQH/MB0GA1UdDgQWBBS3sjTJOcXdkls6WSY2rTx1KIJueTAKBggqhkjO\nPQQDAgNJADBGAiEAoeVWkaXgSUAucQmZ3Yhmx22N/cq1EPBgYHOBZmHt0NkCIQC3\nzONcJ/+WA21OXtb+vcijpUOXtNjyHfcox0N8wsLDqQ==\n-----END CERTIFICATE-----\n",
        "CertIssuerSubject": "MBMxETAPBgNVBAMTCHN3YXJtLWNh",
        "CertIssuerPublicKey": "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAExuICYRP4dnK97fm6OucvaQ4sERCEUNnkyHEC5iz3qfv3qwWV/FPELWUz/aACHwMjfimbcmYeBnF4E8pRREkcpQ=="
      }
    },
    "Status": {
      "State": "ready",
      "Addr": "168.0.32.137"
    },
    "ManagerStatus": {
      "Leader": true,
      "Reachability": "reachable",
      "Addr": "168.0.32.137:2377"
    }
  }
]
```

### Format the output (`--format`)



```console
$ docker node inspect --format '{{ .ManagerStatus.Leader }}' self

false
```

Use `--format=pretty` or the `--pretty` shorthand to pretty-print the output:

​	使用 `--format=pretty` 或 `--pretty` 简写，以美观格式显示输出：

```console
$ docker node inspect --format=pretty self

ID:                     e216jshn25ckzbvmwlnh5jr3g
Hostname:               swarm-manager
Joined at:              2017-05-16 22:52:44.9910662 +0000 utc
Status:
 State:                 Ready
 Availability:          Active
 Address:               172.17.0.2
Manager Status:
 Address:               172.17.0.2:2377
 Raft Status:           Reachable
 Leader:                Yes
Platform:
 Operating System:      linux
 Architecture:          x86_64
Resources:
 CPUs:                  4
 Memory:                7.704 GiB
Plugins:
  Network:              overlay, bridge, null, host, overlay
  Volume:               local
Engine Version:         17.06.0-ce
TLS Info:
 TrustRoot:
-----BEGIN CERTIFICATE-----
MIIBazCCARCgAwIBAgIUOzgqU4tA2q5Yv1HnkzhSIwGyIBswCgYIKoZIzj0EAwIw
EzERMA8GA1UEAxMIc3dhcm0tY2EwHhcNMTcwNTAyMDAyNDAwWhcNMzcwNDI3MDAy
NDAwWjATMREwDwYDVQQDEwhzd2FybS1jYTBZMBMGByqGSM49AgEGCCqGSM49AwEH
A0IABMbiAmET+HZyve35ujrnL2kOLBEQhFDZ5MhxAuYs96n796sFlfxTxC1lM/2g
Ah8DI34pm3JmHgZxeBPKUURJHKWjQjBAMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMB
Af8EBTADAQH/MB0GA1UdDgQWBBS3sjTJOcXdkls6WSY2rTx1KIJueTAKBggqhkjO
PQQDAgNJADBGAiEAoeVWkaXgSUAucQmZ3Yhmx22N/cq1EPBgYHOBZmHt0NkCIQC3
zONcJ/+WA21OXtb+vcijpUOXtNjyHfcox0N8wsLDqQ==
-----END CERTIFICATE-----

 Issuer Public Key: MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAExuICYRP4dnK97fm6OucvaQ4sERCEUNnkyHEC5iz3qfv3qwWV/FPELWUz/aACHwMjfimbcmYeBnF4E8pRREkcpQ==
 Issuer Subject:    MBMxETAPBgNVBAMTCHN3YXJtLWNh
```
