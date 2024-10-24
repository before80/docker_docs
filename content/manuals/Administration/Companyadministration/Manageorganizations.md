+++
title = "Manage organizations"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/admin/company/organizations/](https://docs.docker.com/admin/company/organizations/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Manage organizations

You can manage the organizations in a company in the Docker Admin Console.

**Early Access**

The Docker Admin Console is an [early access]({{< ref "/manuals/Releaselifecycle#early-access-ea" >}}) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

## View all organizations

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. In the left navigation, select your company in the drop-down menu.
3. Under **Organizations**, select **Overview**.

## Add seats to an organization

When you have a [self-serve](https://docs.docker.com/subscription/core-subscription/details/#self-serve) subscription that has no pending subscription changes, you can add seats using the following steps.

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. In the left navigation, select your company in the drop-down menu.
3. Under **Organizations**, select **Overview**.
4. Select the action icon in the organization's card, and then select **Get more seats**.

## Add organizations to a company

> **Important**
>
> 
>
> You must be a company owner to add an organization to a company. You must also be an organization owner of the organization you want to add.

There is no limit to the number of organizations you can have under a company layer. All organizations must have a Business subscription.

> **Important**
>
> 
>
> Once you add an organization to a company, you can't remove it from the company.

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. In the left navigation, select your company in the drop-down menu.
3. Select **Add organization**.
4. Choose the organization you want to add from the drop-down menu.
5. Select **Add organization** to confirm.

## Manage an organization

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. In the left navigation, select your company in the drop-down menu.
3. Select the organization that you want to manage.

For more details about managing an organization, see [Organization administration]({{< ref "/manuals/Administration/Organizationadministration" >}}).

## More resources

- [Video: Managing a company and nested organizations](https://youtu.be/XZ5_i6qiKho?feature=shared&t=229)
- [Video: Adding nested organizations to a company](https://youtu.be/XZ5_i6qiKho?feature=shared&t=454)
