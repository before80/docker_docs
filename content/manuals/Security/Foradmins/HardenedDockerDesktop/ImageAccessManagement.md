+++
title = "Image Access Management"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/security/for-admins/hardened-desktop/image-access-management/](https://docs.docker.com/security/for-admins/hardened-desktop/image-access-management/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Image Access Management

> **Note**
>
> 
>
> Image Access Management is available to [Docker Business](https://docs.docker.com/subscription/core-subscription/details/#docker-business) customers only.

Image Access Management gives administrators control over which types of images, such as Docker Official Images, Docker Verified Publisher Images, or community images, their developers can pull from Docker Hub.

For example, a developer, who is part of an organization, building a new containerized application could accidentally use an untrusted, community image as a component of their application. This image could be malicious and pose a security risk to the company. Using Image Access Management, the organization owner can ensure that the developer can only access trusted content like Docker Official Images, Docker Verified Publisher Images, or the organization’s own images, preventing such a risk.

## Prerequisites

You need to [enforce sign-in]({{< ref "/manuals/Security/Foradmins/Enforcesign-in" >}}). For Image Access Management to take effect, Docker Desktop users must authenticate to your organization. Enforcing sign-in ensures that your Docker Desktop developers always authenticate to your organization, even though they can authenticate without it and the feature will take effect. Enforcing sign-in guarantees the feature always takes effect.

## Configure Image Access Management permissions

Docker Hub Admin Console

------

1. Sign in to [Docker Hub](https://hub.docker.com/).
2. Select **Organizations**, your organization, **Settings**, and then select **Image Access**.
3. Enable Image Access Management to set the permissions for the following categories of images you can manage:

- **Organization images**: Images from your organization are always allowed by default. These images can be public or private created by members within your organization.
- **Docker Official Images**: A curated set of Docker repositories hosted on Hub. They provide OS repositories, best practices for Dockerfiles, drop-in solutions, and applies security updates on time.
- **Docker Verified Publisher Images**: Images published by Docker partners that are part of the Verified Publisher program and are qualified to be included in the developer secure supply chain.
- **Community images**: These images are disabled by default when Image Access Management is enabled because various users contribute them and they may pose security risks. This category includes Docker-Sponsored Open Source images.

> **Note**
>
> Image Access Management is turned off by default. However, owners in your organization have access to all images regardless of the settings.

1. Select the category restrictions for your images by selecting **Allowed**. Once the restrictions are applied, your members can view the organization permissions page in a read-only format.

## Verify the restrictions

The new Image Access Management policy takes effect after the developer successfully authenticates to Docker Desktop using their organization credentials. If a developer attempts to pull a disallowed image type using Docker, they receive an error message.

------

## More resources

- [Video: Hardened Desktop Image Access Management](https://www.youtube.com/watch?v=r3QRKHA1A5U)
