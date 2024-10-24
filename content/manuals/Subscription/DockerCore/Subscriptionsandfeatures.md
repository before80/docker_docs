+++
title = "Subscriptions and features"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/subscription/core-subscription/details/](https://docs.docker.com/subscription/core-subscription/details/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker Core subscriptions and features

> **Important**
>
> 
>
> Docker is introducing enhanced subscription plans. Our new plans are packed with more features, higher usage limits, and simplified pricing. The new subscription plans take effect at your next renewal date that occurs on or after November 15, 2024. No charges on Docker Hub image pulls or storage will be incurred between November 15, 2024, and January 31, 2025. See [Announcing Upgraded Docker Plans](https://www.docker.com/blog/november-2024-updated-plans-announcement/) for more details and learn how your usage fits into these updates.

You can do more with Docker with a Docker Core subscription, such as add collaborators, create scoped access tokens, and create private repositories. This page provides an overview of each subscription tier. To compare features available in each tier, see [Docker Pricing](https://www.docker.com/pricing/).

The following describes some of the key features included with your Docker Core subscription:

![Docker Core subscription diagram](Subscriptionsandfeatures_img/subscription-diagram.webp)

3 Docker Scout-enabled repositories for advanced remote image analysis. You can purchase a Docker Scout subscription to enable more repositories. See [Docker Scout subscriptions and features]({{< ref "/manuals/Subscription/DockerScoutsubscriptionsandfeatures" >}}).

Docker Build Cloud minutes are also included. For more information, see [Docker Build Cloud subscriptions and features]({{< ref "/manuals/Subscription/DockerBuildCloud/Subscriptionsandfeatures" >}}).

Docker maintains a [public roadmap](https://github.com/docker/roadmap) so subscribers can see what new features are in development, as well as request new capabilities.

## Docker Personal

**Docker Personal** (formerly Docker Free) is ideal for open-source communities, individual developers, education, and small businesses, and includes the free use of Docker components including Docker Desktop and Docker Hub. Docker Personal includes:

- Unlimited public repositories
- Unlimited [Scoped Access Tokens]({{< ref "/manuals/Security/Fordevelopers/Accesstokens" >}})
- Unlimited [collaborators](https://docs.docker.com/docker-hub/repos/access/#collaborators-and-their-role) for public repositories at no cost per month.
- Access to [Docker Scout Free](https://docs.docker.com/subscription/scout-details/#docker-scout-free) to get started with software supply chain security.

Additionally, anonymous users get 100 pulls every 6 hours and users that sign in to Docker get 200 pulls every 6 hours.

For a list of features available in each tier, see [Docker Pricing](https://www.docker.com/pricing/).

## Docker Pro

**Docker Pro** enables individual developers to get more control of their development environment and provides an integrated and reliable developer experience. It reduces the amount of time developers spend on mundane and repetitive tasks and empowers developers to spend more time creating value for their customers.

Docker Pro includes:

- All the features available in Personal
- Unlimited private repositories
- 5000 image [pulls per day]({{< ref "/manuals/DockerHub/Usageandratelimits" >}})
- [Auto Builds]({{< ref "/manuals/DockerHub/Automatedbuilds" >}}) with 5 concurrent builds
- 300 [Vulnerability Scans]({{< ref "/manuals/DockerHub/Staticvulnerabilityscanning" >}})

For a list of features available in each tier, see [Docker Pricing](https://www.docker.com/pricing/).

## Docker Team

**Docker Team** offers capabilities for collaboration, productivity, and security across organizations. It enables groups of developers to unlock the full power of collaboration and sharing combined with essential security features and team management capabilities. A Docker Team subscription includes licensing for commercial use of Docker components including Docker Desktop and Docker Hub.

Docker Team includes:

- Everything included in Docker Pro
- Unlimited teams
- [Auto Builds]({{< ref "/manuals/DockerHub/Automatedbuilds" >}}) with 15 concurrent builds
- Unlimited [Vulnerability Scanning]({{< ref "/manuals/DockerHub/Staticvulnerabilityscanning" >}})
- 5000 image [pulls per day]({{< ref "/manuals/DockerHub/Usageandratelimits" >}}) for each team member

There are also advanced collaboration and management tools, including organization and team management with [Role Based Access Control (RBAC)]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}), [activity logs]({{< ref "/manuals/Administration/Organizationadministration/Activitylogs" >}}), and more.

For a list of features available in each tier, see [Docker Pricing](https://www.docker.com/pricing/).

## Docker Business

**Docker Business** offers centralized management and advanced security features for enterprises that use Docker at scale. It empowers leaders to manage their Docker development environments and speed up their secure software supply chain initiatives. A Docker Business subscription includes licensing for commercial use of Docker components including Docker Desktop and Docker Hub.

Docker Business includes:

- Everything included in Docker Team
- [Hardened Docker Desktop]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop" >}})
- [Image Access Management]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/ImageAccessManagement" >}}) which lets admins control what content developers can access
- [Registry Access Management]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/RegistryAccessManagement" >}}) which lets admins control what registries developers can access
- [Company layer]({{< ref "/manuals/Administration/Companyadministration" >}}) to manage multiple organizations and settings
- [Single Sign-On]({{< ref "/manuals/Security/Foradmins/Singlesign-on" >}})
- [System for Cross-domain Identity Management]({{< ref "/manuals/Security/Foradmins/Provisioning/SCIM" >}}) and more.

For a list of features available in each tier, see [Docker Pricing](https://www.docker.com/pricing/).

### Self-serve

A self-serve Docker Business subscription is where everything is set up by you. You can:

- Manage your own invoices
- Add or remove seats
- Update billing and payment information
- Downgrade your subscription at any time

### Sales-assisted

A sales-assisted Docker Business subscription where everything is set up and managed by a dedicated Docker account manager.

### Support for subscriptions

All Docker Pro, Team, and Business subscribers receive email support for their subscriptions. Additional premium support is available for Docker Business customers. [Contact sales](https://www.docker.com/pricing/contact-sales/) for more information about premium support programs.
