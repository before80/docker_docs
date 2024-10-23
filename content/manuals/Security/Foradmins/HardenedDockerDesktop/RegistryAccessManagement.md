+++
title = "Registry Access Management"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/security/for-admins/hardened-desktop/registry-access-management/](https://docs.docker.com/security/for-admins/hardened-desktop/registry-access-management/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Registry Access Management

> **Note**
>
> 
>
> Registry Access Management is available to [Docker Business](https://docs.docker.com/subscription/core-subscription/details/) customers only.

With Registry Access Management (RAM), administrators can ensure that their developers using Docker Desktop only access allowed registries. This is done through the Registry Access Management dashboard in Docker Hub or the Docker Admin Console.

Registry Access Management supports both cloud and on-prem registries. This feature operates at the DNS level and therefore is compatible with all registries. You can add any hostname or domain name you’d like to include in the list of allowed registries. However, if the registry redirects to other domains such as `s3.amazon.com`, then you must add those domains to the list.

Example registries administrators can allow include:

- Docker Hub. This is enabled by default.
- Amazon ECR
- GitHub Container Registry
- Google Container Registry
- GitLab Container Registry
- Nexus
- Artifactory

## [Prerequisites](https://docs.docker.com/security/for-admins/hardened-desktop/registry-access-management/#prerequisites)

You need to [enforce sign-in](https://docs.docker.com/security/for-admins/enforce-sign-in/). For Registry Access Management to take effect, Docker Desktop users must authenticate to your organization. Enforcing sign-in ensures that your Docker Desktop developers always authenticate to your organization, even though they can authenticate without it and the feature will take effect. Enforcing sign-in guarantees the feature always takes effect.

## [Configure Registry Access Management permissions](https://docs.docker.com/security/for-admins/hardened-desktop/registry-access-management/#configure-registry-access-management-permissions)

Docker Hub Admin Console

------

To configure Registry Access Management permissions, perform the following steps:

1. Sign in to [Docker Hub](https://hub.docker.com/).

2. Select **Organizations**, your organization, **Settings**, and then select **Registry Access**.

3. Enable Registry Access Management to set the permissions for your registry.

   > **Note**
   >
   > 
   >
   > When enabled, the Docker Hub registry is set by default, however you can also restrict this registry for your developers.

4. Select **Add registry** and enter your registry details in the applicable fields, and then select **Create** to add the registry to your list.

5. Verify that the registry appears in your list and select **Save changes**.

   > **Note**
   >
   > 
   >
   > Once you add a registry, it can take up to 24 hours for the changes to be enforced on your developers’ machines. If you want to apply the changes sooner, you must force a Docker logout on your developers’ machine and have the developers re-authenticate for Docker Desktop. Also, there is no limit on the number of registries you can add. See the Caveats section below to learn more about limitations when using this feature.

   > **Tip**
   >
   > 
   >
   > Since RAM sets policies about where content can be fetched from, the [ADD](https://docs.docker.com/reference/dockerfile/#add) instruction of the Dockerfile, when the parameter of the ADD instruction is a URL, is also subject to registry restrictions. It's recommended that you add the domains of URL parameters to the list of allowed registry addresses under the Registry Access Management settings of your organization.

------

## [Verify the restrictions](https://docs.docker.com/security/for-admins/hardened-desktop/registry-access-management/#verify-the-restrictions)

The new Registry Access Management policy takes effect after the developer successfully authenticates to Docker Desktop using their organization credentials. If a developer attempts to pull an image from a disallowed registry via the Docker CLI, they receive an error message that the organization has disallowed this registry.

## [Caveats](https://docs.docker.com/security/for-admins/hardened-desktop/registry-access-management/#caveats)

There are certain limitations when using Registry Access Management:

- Windows image pulls and image builds are not restricted by default. For Registry Access Management to take effect on Windows Container mode, you must allow the Windows Docker daemon to use Docker Desktop's internal proxy by selecting the [Use proxy for Windows Docker daemon](https://docs.docker.com/desktop/settings/#proxies) setting.
- Builds such as `docker buildx` using a Kubernetes driver are not restricted
- Builds such as `docker buildx` using a custom docker-container driver are not restricted
- Blocking is DNS-based; you must use a registry's access control mechanisms to distinguish between “push” and “pull”
- WSL 2 requires at least a 5.4 series Linux kernel (this does not apply to earlier Linux kernel series)
- Under the WSL 2 network, traffic from all Linux distributions is restricted (this will be resolved in the updated 5.15 series Linux kernel)
- Images pulled by Docker Desktop when Docker Debug or Kubernetes is enabled, are not restricted by default even if Docker Hub is blocked by RAM.

Also, Registry Access Management operates on the level of hosts, not IP addresses. Developers can bypass this restriction within their domain resolution, for example by running Docker against a local proxy or modifying their operating system's `sts` file. Blocking these forms of manipulation is outside the remit of Docker Desktop.

## [More resources](https://docs.docker.com/security/for-admins/hardened-desktop/registry-access-management/#more-resources)

- [Video: Hardened Desktop Registry Access Management](https://www.youtube.com/watch?v=l9Z6WJdJC9A)
