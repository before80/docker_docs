+++
title = "Docker Scout environment variables"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/scout/how-tos/configure-cli/](https://docs.docker.com/scout/how-tos/configure-cli/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Configure Docker Scout with environment variables

The following environment variables are available to configure the Docker Scout CLI commands, and the corresponding `docker/scout-cli` container image:

| Name                                    | Format  | Description                                                  |
| :-------------------------------------- | ------- | :----------------------------------------------------------- |
| DOCKER_SCOUT_CACHE_FORMAT               | String  | Format of the local image cache; can be `oci` or `tar` (default: `oci`) |
| DOCKER_SCOUT_CACHE_DIR                  | String  | Directory where the local SBOM cache is stored (default: `$HOME/.docker/scout`) |
| DOCKER_SCOUT_NO_CACHE                   | Boolean | When set to `true`, disables the use of local SBOM cache     |
| DOCKER_SCOUT_OFFLINE                    | Boolean | Use [offline mode](https://docs.docker.com/scout/how-tos/configure-cli/#offline-mode) when indexing SBOM |
| DOCKER_SCOUT_REGISTRY_TOKEN             | String  | Token for authenticating to a registry when pulling images   |
| DOCKER_SCOUT_REGISTRY_USER              | String  | Username for authenticating to a registry when pulling images |
| DOCKER_SCOUT_REGISTRY_PASSWORD          | String  | Password or personal access token for authenticating to a registry when pulling images |
| DOCKER_SCOUT_HUB_USER                   | String  | Docker Hub username for authenticating to the Docker Scout backend |
| DOCKER_SCOUT_HUB_PASSWORD               | String  | Docker Hub password or personal access token for authenticating to the Docker Scout backend |
| DOCKER_SCOUT_NEW_VERSION_WARN           | Boolean | Warn about new versions of the Docker Scout CLI              |
| DOCKER_SCOUT_EXPERIMENTAL_WARN          | Boolean | Warn about experimental features                             |
| DOCKER_SCOUT_EXPERIMENTAL_POLICY_OUTPUT | Boolean | Disable experimental output for policy evaluation            |

## Offline mode

Under normal operation, Docker Scout cross-references external systems, such as npm, NuGet, or proxy.golang.org, to retrieve additional information about packages found in your image.

When `DOCKER_SCOUT_OFFLINE` is set to `true`, Docker Scout image analysis runs in offline mode. Offline mode means Docker Scout doesn't make outbound requests to external systems.

To use offline mode:



```console
$ export DOCKER_SCOUT_OFFLINE=true
```
