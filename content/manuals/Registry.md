+++
title = "Registry"
date = 2024-10-23T14:54:40+08:00
weight = 170
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/registry/](https://docs.docker.com/registry/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Registry

> **Important**
>
> 
>
> The ability to push [deprecated Docker image manifest version 2, schema 1](https://distribution.github.io/distribution/spec/deprecated-schema-v1/) images to Docker Hub will be deprecated on November 4th, 2024.

Registry, the open source implementation for storing and distributing container images and other content, has been donated to the CNCF. Registry now goes under the name of Distribution, and the documentation has moved to [distribution/distribution](https://distribution.github.io/distribution/).

The Docker Hub registry implementation is based on Distribution. Docker Hub implements version 1.0.1 OCI distribution [specification](https://github.com/opencontainers/distribution-spec/blob/v1.0.1/spec.md). For reference documentation on the API protocol that Docker Hub implements, refer to the OCI distribution specification.

## Supported media types

Docker Hub supports the following image manifest formats for pulling images:

- [OCI image manifest](https://github.com/opencontainers/image-spec/blob/main/manifest.md)
- [Docker image manifest version 2, schema 2](https://distribution.github.io/distribution/spec/manifest-v2-2/)
- Docker image manifest version 2, schema 1
- Docker image manifest version 1

You can push images with the following formats:

- [OCI image manifest](https://github.com/opencontainers/image-spec/blob/main/manifest.md)
- [Docker image manifest version 2, schema 2](https://distribution.github.io/distribution/spec/manifest-v2-2/)

Docker Hub also supports OCI artifacts. See [OCI artifacts]({{< ref "/manuals/DockerHub/OCIartifacts" >}}).

## Authentication

For documentation related to authentication to the Docker Hub registry, see:

- [Token authentication specification](https://distribution.github.io/distribution/spec/auth/token/)
- [OAuth 2.0 token authentication](https://distribution.github.io/distribution/spec/auth/oauth/)
- [JWT authentication](https://distribution.github.io/distribution/spec/auth/jwt/)
- [Token scope and access](https://distribution.github.io/distribution/spec/auth/scope/)
