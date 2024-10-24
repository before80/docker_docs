+++
title = "Deactivate an account"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/accounts/deactivate-user-account/](https://docs.docker.com/accounts/deactivate-user-account/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Deactivate an account

You can deactivate an account at any time. This section describes the prerequisites and steps to deactivate a user account. For information on deactivating an organization, see [Deactivating an organization](https://docs.docker.com/admin/deactivate-account/).

> **Warning**
>
> 
>
> All Docker products and services that use your Docker account will be inaccessible after deactivating your account.

## [Prerequisites](https://docs.docker.com/accounts/deactivate-user-account/#prerequisites)

Before deactivating your Docker account, ensure that you meet the following requirements:

- For owners, you must leave your organization or company before deactivating your Docker account. To do this:
  1. Sign in to the [Docker Admin Console](https://app.docker.com/admin).
  2. Select the organization you need to leave from the top-left drop-down menu.
  3. Find your username in the **Members** tab.
  4. Select the **More options** menu and then select **Leave organization**.
- If you are the sole owner of an organization, you must assign the owner role to another member of the organization and then remove yourself from the organization, or deactivate the organization. Similarly, if you are the sole owner of a company, either add someone else as a company owner and then remove yourself, or deactivate the company.
- If you have an active Docker subscription, [downgrade it to a Docker Personal subscription](https://docs.docker.com/subscription/core-subscription/downgrade/).
- If you have an active Docker Build Cloud Paid subscription, [downgrade it to a Docker Build Cloud Starter subscription](https://docs.docker.com/billing/build-billing/#downgrade-your-subscription).
- If you have an active Docker Scout subscription, [downgrade it to a Docker Scout Free subscription](https://docs.docker.com/billing/scout-billing/#downgrade-your-subscription).
- Download any images and tags you want to keep. Use `docker pull -a <image>:<tag>`.
- Unlink your [GitHub and Bitbucket accounts](https://docs.docker.com/docker-hub/builds/link-source/#unlink-a-github-user-account).

## [Deactivate](https://docs.docker.com/accounts/deactivate-user-account/#deactivate)

Once you have completed all the previous steps, you can deactivate your account.

> **Warning**
>
> 
>
> This cannot be undone. Be sure you've gathered all the data you need from your account before deactivating it.

1. Sign in to [Docker Home](https://app.docker.com/login).
2. Select your avatar to open the drop-down menu.
3. Select **Account settings**.
4. In the **Account management** section, select **Deactivate account**.
5. To confirm, select **Deactivate account**.
