+++
title = "Deactivating an organization"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/admin/deactivate-account/](https://docs.docker.com/admin/deactivate-account/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Deactivating an organization

You can deactivate an account at any time. This section describes the prerequisites and steps to deactivate an organization account. For information on deactivating a user account, see [Deactivate a user account](https://docs.docker.com/accounts/deactivate-user-account/).

> **Warning**
>
> 
>
> All Docker products and services that use your Docker account or organization account will be inaccessible after deactivating your account.

## [Prerequisites](https://docs.docker.com/admin/deactivate-account/#prerequisites)

Before deactivating an organization, complete the following:

- Download any images and tags you want to keep: `docker pull -a <image>:<tag>`.
- If you have an active Docker subscription, [downgrade it to a free subscription](https://docs.docker.com/subscription/core-subscription/downgrade/).
- If you have an active Docker Scout subscription, [downgrade it to a Docker Scout Free subscription](https://docs.docker.com/billing/scout-billing/#downgrade-your-subscription).
- Remove all other members within the organization.
- Unlink your [Github and Bitbucket accounts](https://docs.docker.com/docker-hub/builds/link-source/#unlink-a-github-user-account).

## [Deactivate](https://docs.docker.com/admin/deactivate-account/#deactivate)

Once you have completed all the previous steps, you can deactivate your organization.

> **Warning**
>
> 
>
> This cannot be undone. Be sure you've gathered all the data you need from your organization before deactivating it.



{{< tabpane text=true persist=disabled >}}

{{% tab header="Admin" %}}

1. In Admin Console, choose the organization you want to deactivate.
2. Under **Organization settings**, select **Deactivate**.
3. Enter the organization name to confirm deactivation.
4. Select **Deactivate organization**.

{{% /tab  %}}

{{% tab header="Console Hub" %}}

1. On Docker Hub, select **Organizations**.
2. Choose the organization you want to deactivate.
3. In **Settings**, select the **Deactivate Org** tab and then **Deactivate organization**.

{{% /tab  %}}

{{< /tabpane >}}

