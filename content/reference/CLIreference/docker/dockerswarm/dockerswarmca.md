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

## Description

View or rotate the current swarm CA certificate.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Options

| Option                                                       | Default     | Description                                                  |
| ------------------------------------------------------------ | ----------- | ------------------------------------------------------------ |
| `--ca-cert`                                                  |             | Path to the PEM-formatted root CA certificate to use for the new cluster |
| `--ca-key`                                                   |             | Path to the PEM-formatted root CA key to use for the new cluster |
| `--cert-expiry`                                              | `2160h0m0s` | Validity period for node certificates (ns\|us\|ms\|s\|m\|h)  |
| [`-d, --detach`](https://docs.docker.com/reference/cli/docker/swarm/ca/#detach) |             | Exit immediately instead of waiting for the root rotation to converge |
| `--external-ca`                                              |             | Specifications of one or more certificate signing endpoints  |
| `-q, --quiet`                                                |             | Suppress progress output                                     |
| [`--rotate`](https://docs.docker.com/reference/cli/docker/swarm/ca/#rotate) |             | Rotate the swarm CA - if no certificate or key are provided, new ones will be generated |

## Examples

Run the `docker swarm ca` command without any options to view the current root CA certificate in PEM format.



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



```console
$ docker swarm ca --rotate
desired root digest: sha256:05da740cf2577a25224c53019e2cce99bcc5ba09664ad6bb2a9425d9ebd1b53e
  rotated TLS certificates:  [=========================>                         ] 1/2 nodes
  rotated CA certificates:   [>                                                  ] 0/2 nodes
```

Once the rotation os finished (all the progress bars have completed) the now-current CA certificate will be printed:



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

### Root CA rotation (--rotate)

> **Note**
>
> Mirantis Kubernetes Engine (MKE), formerly known as Docker UCP, provides an external certificate manager service for the swarm. If you run swarm on MKE, you shouldn't rotate the CA certificates manually. Instead, contact Mirantis support if you need to rotate a certificate.

Root CA Rotation is recommended if one or more of the swarm managers have been compromised, so that those managers can no longer connect to or be trusted by any other node in the cluster.

Alternately, root CA rotation can be used to give control of the swarm CA to an external CA, or to take control back from an external CA.

The `--rotate` flag does not require any parameters to do a rotation, but you can optionally specify a certificate and key, or a certificate and external CA URL, and those will be used instead of an automatically-generated certificate/key pair.

Because the root CA key should be kept secret, if provided it will not be visible when viewing swarm any information via the CLI or API.

The root CA rotation will not be completed until all registered nodes have rotated their TLS certificates. If the rotation is not completing within a reasonable amount of time, try running `docker node ls --format '{{.ID}} {{.Hostname}} {{.Status}} {{.TLSStatus}}'` to see if any nodes are down or otherwise unable to rotate TLS certificates.

### Run root CA rotation in detached mode (--detach)

Initiate the root CA rotation, but do not wait for the completion of or display the progress of the rotation.
