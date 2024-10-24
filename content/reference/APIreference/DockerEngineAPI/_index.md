+++
title = "Docker Engine API"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/engine/](https://docs.docker.com/reference/api/engine/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Docker Engine API

Docker provides an API for interacting with the Docker daemon (called the Docker Engine API), as well as SDKs for Go and Python. The SDKs allow you to efficiently build and scale Docker apps and solutions. If Go or Python don't work for you, you can use the Docker Engine API directly.

For information about Docker Engine SDKs, see [Develop with Docker Engine SDKs]({{< ref "/reference/APIreference/DockerEngineAPI/SDK" >}}).

The Docker Engine API is a RESTful API accessed by an HTTP client such as `wget` or `curl`, or the HTTP library which is part of most modern programming languages.

## View the API reference

You can [view the reference for the latest version of the API]({{< ref "/reference/APIreference/DockerEngineAPI" >}}) or [choose a specific version]({{< ref "/reference/APIreference/DockerEngineAPI/EngineAPIversionhistory" >}}).

## Versioned API and SDK

The version of the Docker Engine API you should use depends upon the version of your Docker daemon and Docker client.

A given version of the Docker Engine SDK supports a specific version of the Docker Engine API, as well as all earlier versions. If breaking changes occur, they are documented prominently.

> **Note**
>
> 
>
> The Docker daemon and client don't necessarily need to be the same version at all times. However, keep the following in mind.
>
> - If the daemon is newer than the client, the client doesn't know about new features or deprecated API endpoints in the daemon.
> - If the client is newer than the daemon, the client can request API endpoints that the daemon doesn't know about.

A new version of the API is released when new features are added. The Docker API is backward-compatible, so you don't need to update code that uses the API unless you need to take advantage of new features.

To see the highest version of the API your Docker daemon and client support, use `docker version`:



```console
$ docker version
Client: Docker Engine - Community
 Version:           27.1.2
 API version:       1.46
 Go version:        go1.21.13
 Git commit:        d01f264
 Built:             Mon Aug 12 11:51:13 2024
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          27.1.2
  API version:      1.46 (minimum version 1.24)
  Go version:       go1.21.13
  Git commit:       f9522e5
  Built:            Mon Aug 12 11:51:13 2024
  OS/Arch:          linux/amd64
  Experimental:     false
  ...
```

You can specify the API version to use in any of the following ways:

- When using the SDK, use the latest version. At a minimum, use the version that incorporates the API version with the features you need.

- When using `curl` directly, specify the version as the first part of the URL. For instance, if the endpoint is `/containers/` you can use `/v1.46/containers/`.

- To force the Docker CLI or the Docker Engine SDKs to use an older version of the API than the version reported by `docker version`, set the environment variable `DOCKER_API_VERSION` to the correct version. This works on Linux, Windows, or macOS clients.

  

  ```console
  $ DOCKER_API_VERSION='1.44'
  ```

  While the environment variable is set, that version of the API is used, even if the Docker daemon supports a newer version. This environment variable disables API version negotiation, so you should only use it if you must use a specific version of the API, or for debugging purposes.

- The Docker Go SDK allows you to enable API version negotiation, automatically selects an API version that's supported by both the client and the Docker Engine that's in use.

- For the SDKs, you can also specify the API version programmatically as a parameter to the `client` object. See the [Go constructor](https://pkg.go.dev/github.com/docker/docker/client#NewClientWithOpts) or the [Python SDK documentation for `client`](https://docker-py.readthedocs.io/en/stable/client.html).

### API version matrix

| Docker version | Maximum API version                                          | Change log                                                   |
| :------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 27.2           | [1.47]({{< ref "/reference/APIreference/DockerEngineAPI/Latest" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v147-api-changes) |
| 27.1           | [1.46]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_46" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v146-api-changes) |
| 27.0           | [1.46]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_46" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v146-api-changes) |
| 26.1           | [1.45]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_45" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v145-api-changes) |
| 26.0           | [1.45]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_45" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v145-api-changes) |
| 25.0           | [1.44]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_44" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v144-api-changes) |
| 24.0           | [1.43]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_43" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v143-api-changes) |
| 23.0           | [1.42]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_42" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v142-api-changes) |
| 20.10          | [1.41]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_41" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v141-api-changes) |
| 19.03          | [1.40]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_40" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v140-api-changes) |
| 18.09          | [1.39]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_39" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v139-api-changes) |
| 18.06          | [1.38]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_38" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v138-api-changes) |
| 18.05          | [1.37]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_37" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v137-api-changes) |
| 18.04          | [1.37]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_37" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v137-api-changes) |
| 18.03          | [1.37]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_37" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v137-api-changes) |
| 18.02          | [1.36]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_36" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v136-api-changes) |
| 17.12          | [1.35]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_35" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v135-api-changes) |
| 17.11          | [1.34]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_34" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v134-api-changes) |
| 17.10          | [1.33]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_33" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v133-api-changes) |
| 17.09          | [1.32]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_32" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v132-api-changes) |
| 17.07          | [1.31]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_31" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v131-api-changes) |
| 17.06          | [1.30]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_30" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v130-api-changes) |
| 17.05          | [1.29]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_29" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v129-api-changes) |
| 17.04          | [1.28]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_28" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v128-api-changes) |
| 17.03.1        | [1.27]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_27" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v127-api-changes) |
| 17.03          | [1.26]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_27" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v126-api-changes) |
| 1.13.1         | [1.26]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_26" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v126-api-changes) |
| 1.13           | [1.25]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/v1_26" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v125-api-changes) |
| 1.12           | [1.24]({{< ref "/reference/APIreference/DockerEngineAPI/APIreferencebyversion/EngineAPIv1_24" >}}) | [changes](https://docs.docker.com/reference/api/engine/version-history/#v124-api-changes) |

### Deprecated API versions

API versions before v1.24 are [deprecated](https://docs.docker.com/engine/deprecated/#deprecate-legacy-api-versions). You can find archived documentation for deprecated versions of the API in the code repository on GitHub:

- [Documentation for API versions 1.23 and before](https://github.com/moby/moby/tree/v25.0.0/docs/api).
- [Documentation for API versions 1.17 and before](https://github.com/moby/moby/tree/v1.9.1/docs/reference/api).
