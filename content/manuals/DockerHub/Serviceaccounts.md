+++
title = "Service accounts"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/docker-hub/service-accounts/](https://docs.docker.com/docker-hub/service-accounts/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Service accounts

> **Important**
>
> 
>
> Docker is introducing enhanced subscription plans. Our new plans are packed with more features, higher usage limits, and simplified pricing. The new subscription plans take effect at your next renewal date that occurs on or after November 15, 2024. No charges on Docker Hub image pulls or storage will be incurred between November 15, 2024, and January 31, 2025. See [Announcing Upgraded Docker Plans](https://www.docker.com/blog/november-2024-updated-plans-announcement/) for more details and learn how your usage fits into these updates.

> **Note**
>
> 
>
> Service accounts require a [Docker Team, or Business subscription]({{< ref "/manuals/Subscription" >}}).
>
> Docker recommends that you use organization access tokens instead of service accounts. For more details, see [Organization access tokens]({{< ref "/manuals/Security/Foradmins/OrganizationaccesstokensBeta" >}}).

A service account is a Docker ID used for automated management of container images or containerized applications. Service accounts are typically used in automated workflows, and don't share Docker IDs with the members in the organization. Common use cases for service accounts include mirroring content on Docker Hub, or tying in image pulls from your CI/CD process.

> **Note**
>
> 
>
> All paid Docker subscriptions include up to 5000 pulls per day per authenticated user. If you require a higher number of pulls, you can purchase an Enhanced Service Account add-on.

## Enhanced Service Account add-on pricing

Refer to the following table for details on the Enhanced Service Account add-on pricing:

| Tier | Pull Rates Per Day* | Annual Fee                                                   |
| ---- | ------------------- | ------------------------------------------------------------ |
| 1    | 5,000-10,000        | $9,950/yr                                                    |
| 2    | 10,000-25,000       | $17,950/yr                                                   |
| 3    | 25,000-50,000       | $32,950/yr                                                   |
| 4    | 50,000-100,000      | $58,950/yr                                                   |
| 5    | 100,000+            | [Contact Sales](https://www.docker.com/pricing/contact-sales/) |

*Once you establish the initial Tier, that's the minimum fee for the year. Annual commitment required. The service account may exceed Pulls by up to 25% for up to 20 days during the year without incurring additional fees. Reports on consumption are available upon request. At the end of the initial 1-year term, the appropriate Tier will be established for the following year.

## How a pull is defined

- Pulls are accounted to the user doing the pull, not to the owner of the image.
- A pull request is defined as up to two `GET` requests on registry manifest URLs (`/v2/*/manifests/*`).
- A normal image pull makes a single manifest request.
- A pull request for a multi-arch image makes two manifest requests.
- `HEAD` requests aren't counted.
- Some images are unlimited through our [Docker Sponsored Open Source](https://www.docker.com/blog/expanded-support-for-open-source-software-projects/) and [Docker Verified Publisher](https://www.docker.com/partners/programs) programs.

## Creating a new service account

To create a new service account for your Team account:

1. Create a new Docker ID.
2. Create a [team]({{< ref "/manuals/Administration/Organizationadministration/Createandmanageateam" >}}) in your organization and grant it read-only access to your private repositories.
3. Add the new Docker ID to your [organization]({{< ref "/manuals/Administration/Organizationadministration/Createyourorganization" >}}).
4. Add the new Docker ID to the [team]({{< ref "/manuals/Administration/Organizationadministration/Createandmanageateam" >}}) you created earlier.
5. Create a new [personal access token (PAT)]({{< ref "/manuals/Security/Fordevelopers/Accesstokens" >}}) from the user account and use it for CI.

> **Note**
>
> 
>
> If you want a read-only PAT just for your open-source repositories, or to access official images and other public images, you don't have to grant any access permissions to the new Docker ID.

## Additional information

Refer to the following topics for additional information:

- [Mirroring Docker Hub]({{< ref "/manuals/DockerHub/Mirroring" >}})
- [Docker pricing FAQs](https://www.docker.com/pricing/faq/)
