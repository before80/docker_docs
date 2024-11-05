+++
title = "docker swarm ca"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/swarm/ca/](https://docs.docker.com/reference/cli/docker/swarm/ca/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker swarm ca

| Description | Display and rotate the root CA |
| :---------- | ------------------------------ |
| Usage       | `docker swarm ca [OPTIONS]`    |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 协调器。

## Description

View or rotate the current swarm CA certificate.

​	查看或轮换当前 Swarm CA 证书。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理器和工作节点，请参阅 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default     | Description                                                  |
| ------------------------------------------------------------ | ----------- | ------------------------------------------------------------ |
| `--ca-cert`                                                  |             | 用于新集群的 PEM 格式的根 CA 证书路径 Path to the PEM-formatted root CA certificate to use for the new cluster |
| `--ca-key`                                                   |             | 用于新集群的 PEM 格式的根 CA 密钥路径 Path to the PEM-formatted root CA key to use for the new cluster |
| `--cert-expiry`                                              | `2160h0m0s` | 节点证书的有效期 Validity period for node certificates (ns\|us\|ms\|s\|m\|h) |
| [`-d, --detach`](https://docs.docker.com/reference/cli/docker/swarm/ca/#detach) |             | 立即退出而不等待根轮换完成 Exit immediately instead of waiting for the root rotation to converge |
| `--external-ca`                                              |             | 一个或多个证书签发端点的规范 Specifications of one or more certificate signing endpoints |
| `-q, --quiet`                                                |             | 抑制进度输出 Suppress progress output                        |
| [`--rotate`](https://docs.docker.com/reference/cli/docker/swarm/ca/#rotate) |             | 轮换 Swarm CA - 如果未提供证书或密钥，则将生成新的证书和密钥  Rotate the swarm CA - if no certificate or key are provided, new ones will be generated |

## Examples

Run the `docker swarm ca` command without any options to view the current root CA certificate in PEM format.

​	运行 `docker swarm ca` 命令而不带任何选项，以 PEM 格式查看当前根 CA 证书。

```console
$ docker swarm ca

-----BEGIN CERTIFICATE-----
MIIBazCCARCgAwIBAgIUJPzo67QC7g8Ebg2ansjkZ8CbmaswCgYIKoZIzj0EAwIw
EzERMA8GA1UEAxMIc3dhcm0tY2EwHhcNMTcwNTAzMTcxMDAwWhcNMzcwNDI4MTcx
MDAwWjATMREwDwYDVQQDEwhzd2FybS1jYTBZMBMGByqGSM49AgEGCCqGSM49AwEH
A0IABKL6/C0sihYEb935wVPRA8MqzPLn3jzou0OJRXHsCLcVExigrMdgmLCC+Va4
+sJ+SLVO1eQbvLHH8uuDdF/QOU6jQjBAMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMB
Af8EBTADAQH/MB0GA1UdDgQWBBSfUy5bjUnBAx/B0GkOBKp91XvxzjAKBggqhkjO
PQQDAgNJADBGAiEAnbvh0puOS5R/qvy1PMHY1iksYKh2acsGLtL/jAIvO4ACIQCi
lIwQqLkJ48SQqCjG1DBTSBsHmMSRT+6mE2My+Z3GKA==
-----END CERTIFICATE-----
```

Pass the `--rotate` flag (and optionally a `--ca-cert`, along with a `--ca-key` or `--external-ca` parameter flag), in order to rotate the current swarm root CA.

​	使用 `--rotate` 标志（可选地添加 `--ca-cert`，连同 `--ca-key` 或 `--external-ca` 参数标志）轮换当前 Swarm 根 CA。

```console
$ docker swarm ca --rotate
desired root digest: sha256:05da740cf2577a25224c53019e2cce99bcc5ba09664ad6bb2a9425d9ebd1b53e
  rotated TLS certificates:  [=========================>                         ] 1/2 nodes
  rotated CA certificates:   [>                                                  ] 0/2 nodes
```

Once the rotation os finished (all the progress bars have completed) the now-current CA certificate will be printed:

​	一旦轮换完成（所有进度条已完成），将显示当前 CA 证书：

```console
$ docker swarm ca --rotate
desired root digest: sha256:05da740cf2577a25224c53019e2cce99bcc5ba09664ad6bb2a9425d9ebd1b53e
  rotated TLS certificates:  [==================================================>] 2/2 nodes
  rotated CA certificates:   [==================================================>] 2/2 nodes
-----BEGIN CERTIFICATE-----
MIIBazCCARCgAwIBAgIUFynG04h5Rrl4lKyA4/E65tYKg8IwCgYIKoZIzj0EAwIw
EzERMA8GA1UEAxMIc3dhcm0tY2EwHhcNMTcwNTE2MDAxMDAwWhcNMzcwNTExMDAx
MDAwWjATMREwDwYDVQQDEwhzd2FybS1jYTBZMBMGByqGSM49AgEGCCqGSM49AwEH
A0IABC2DuNrIETP7C7lfiEPk39tWaaU0I2RumUP4fX4+3m+87j0DU0CsemUaaOG6
+PxHhGu2VXQ4c9pctPHgf7vWeVajQjBAMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMB
Af8EBTADAQH/MB0GA1UdDgQWBBSEL02z6mCI3SmMDmITMr12qCRY2jAKBggqhkjO
PQQDAgNJADBGAiEA263Eb52+825EeNQZM0AME+aoH1319Zp9/J5ijILW+6ACIQCg
gyg5u9Iliel99l7SuMhNeLkrU7fXs+Of1nTyyM73ig==
-----END CERTIFICATE-----
```

### 根 CA 轮换 (`--rotate`) Root CA rotation (`--rotate`)

> **Note**
>
> Mirantis Kubernetes Engine (MKE), formerly known as Docker UCP, provides an external certificate manager service for the swarm. If you run swarm on MKE, you shouldn't rotate the CA certificates manually. Instead, contact Mirantis support if you need to rotate a certificate.
>
> ​	Mirantis Kubernetes Engine (MKE)，原称 Docker UCP，为 Swarm 提供了外部证书管理服务。如果在 MKE 上运行 Swarm，则不应手动轮换 CA 证书。需要轮换证书时，请联系 Mirantis 支持。

Root CA Rotation is recommended if one or more of the swarm managers have been compromised, so that those managers can no longer connect to or be trusted by any other node in the cluster.

​	当一个或多个 Swarm 管理节点被入侵时，建议进行根 CA 轮换，以便这些管理节点不再能够连接或被集群中的任何其他节点信任。

Alternately, root CA rotation can be used to give control of the swarm CA to an external CA, or to take control back from an external CA.

​	或者，根 CA 轮换可以用于将 Swarm CA 的控制权移交给外部 CA，或者从外部 CA 手中收回控制权。

The `--rotate` flag does not require any parameters to do a rotation, but you can optionally specify a certificate and key, or a certificate and external CA URL, and those will be used instead of an automatically-generated certificate/key pair.

​	`--rotate` 标志无需任何参数即可进行轮换，但您可以选择指定证书和密钥，或证书和外部 CA URL，系统将使用这些而非自动生成的证书/密钥对。

Because the root CA key should be kept secret, if provided it will not be visible when viewing swarm any information via the CLI or API.

​	由于根 CA 密钥应保持机密性，如果提供了该密钥，在通过 CLI 或 API 查看 Swarm 信息时将不会显示该密钥。

The root CA rotation will not be completed until all registered nodes have rotated their TLS certificates. If the rotation is not completing within a reasonable amount of time, try running `docker node ls --format '{{.ID}} {{.Hostname}} {{.Status}} {{.TLSStatus}}'` to see if any nodes are down or otherwise unable to rotate TLS certificates.

​	在所有注册节点完成 TLS 证书轮换之前，根 CA 轮换将不会完成。如果轮换未在合理的时间内完成，请尝试运行 `docker node ls --format '{{.ID}} {{.Hostname}} {{.Status}} {{.TLSStatus}}'` 以查看是否有节点处于关闭状态或无法轮换 TLS 证书。

### 在分离模式下运行根 CA 轮换 (`--detach`) Run root CA rotation in detached mode (`--detach`)

Initiate the root CA rotation, but do not wait for the completion of or display the progress of the rotation.

​	启动根 CA 轮换，但不等待轮换完成，也不显示轮换进度。
