+++
title = "Docker Hub"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/docker-hub/](https://docs.docker.com/docker-hub/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overview of Docker Hub

Docker Hub is a service provided by Docker for finding and sharing container images.

It's the world’s largest repository of container images with an array of content sources including container community developers, open source projects, and independent software vendors (ISV) building and distributing their code in containers.

Docker Hub is also where you can go to [carry out administrative tasks for organizations]({{< ref "/manuals/Administration" >}}). If you have a Docker Team or Business subscription, you can also carry out administrative tasks in the [Docker Admin Console](https://admin.docker.com/).

{{< tabpane text=true persist=disabled >}}

{{% tab header="What key features are included in Docker Hub?" %}}

- [Repositories]({{< ref "/manuals/DockerHub/Managerepositories" >}}): Push and pull container images.

- [Builds]({{< ref "/manuals/DockerHub/Automatedbuilds" >}}): Automatically build container images from GitHub and Bitbucket and push them to Docker Hub.

- [Webhooks]({{< ref "/manuals/DockerHub/Webhooks" >}}): Trigger actions after a successful push to a repository to integrate Docker Hub with other services.

- Docker Hub CLI

   

  tool (currently experimental) and an API that allows you to interact with Docker Hub.

  - Browse through the [Docker Hub API]({{< ref "/reference/APIreference/DockerHubAPI/DockerHubAPI" >}}) documentation to explore the supported endpoints.

{{% /tab  %}}

{{% tab header="What administrative tasks can I perform in Docker Hub?" %}}

- [Create and manage teams and organizations]({{< ref "/manuals/Administration/Organizationadministration/Createyourorganization" >}})
- [Create a company]({{< ref "/manuals/Administration/Companyadministration/Createacompany" >}})
- [Enforce sign in]({{< ref "/manuals/Security/Foradmins/Enforcesign-in" >}})
- Set up [SSO]({{< ref "/manuals/Security/Foradmins/Singlesign-on" >}}) and [SCIM]({{< ref "/manuals/Security/Foradmins/Provisioning/SCIM" >}})
- Use [Group mapping]({{< ref "/manuals/Security/Foradmins/Provisioning/Groupmapping" >}})
- [Carry out domain audits]({{< ref "/manuals/Security/Foradmins/Domainaudit" >}})
- [Use Image Access Management]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/ImageAccessManagement" >}}) to control developers' access to certain types of images
- [Turn on Registry Access Management]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/RegistryAccessManagement" >}})

{{% /tab  %}}

{{< /tabpane >}}

 

Quickstart

Step-by-step instructions on getting started on Docker Hub.



Create a repository

Create a repository to share your images with your team, customers, or the Docker community.



Manage repository access

Manage access to push and pull to your repository and assign permissions.



Automated builds

Learn how you can automatically build images from source code to push to your repositories.



Release notes

Find out about new features, improvements, and bug fixes.
