+++
title = "Administration"
date = 2024-10-23T14:54:40+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/admin/](https://docs.docker.com/admin/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Administration overview

Administrators can manage companies and organizations using the Docker Admin Console, or manage organizations in Docker Hub.

The Docker Admin Console is available in [Early Access](https://docs.docker.com/release-lifecycle/#early-access-ea) to all company owners and organization owners. The [Docker Admin Console](https://admin.docker.com/) provides administrators with centralized observability, access management, and controls for their company and organizations. To provide these features, Docker uses the following hierarchy and roles.

![Docker hierarchy](_index_img/docker-admin-structure.webp)

- Company: A company simplifies the management of Docker organizations and settings. Creating a company is optional and only available to Docker Business subscribers.
  - Company owner: A company can have multiple owners. Company owners have company-wide observability and can manage company-wide settings that apply to all associated organizations. In addition, company owners have the same access as organization owners for all associated organizations.
- Organization: An organization is a collection of teams and repositories. Docker Team and Business subscribers must have at least one organization.
  - Organization owner: An organization can have multiple owners. Organization owners have observability into their organization and can manage its users and settings.
- Team: A team is a group of Docker members that belong to an organization. Organization and company owners can group members into additional teams to configure repository permissions on a per-team basis. Using teams to group members is optional.
- Member: A member is a Docker user that's a member of an organization. Organization and company owners can assign roles to members to define their permissions.



Company administration

Explore how to manage a company.



Organization administration

Learn about organization administration.



Onboard your organization

Learn how to onboard and secure your organization.



Company FAQ

Discover common questions and answers about companies.



Organization FAQ

Explore popular FAQ topics about organizations.



Security

Explore security features for administrators.
