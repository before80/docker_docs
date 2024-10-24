+++
title = "Manage company owners"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/admin/company/owners/](https://docs.docker.com/admin/company/owners/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Manage company owners

A company can have multiple owners. Company owners have company-wide observability and can manage company-wide settings that apply to all associated organizations. In addition, company owners have the same access as organization owners for all associated organizations. Unlike organization owners, company owners don't need to be member of an organization. When company owners aren't a member in an organization, they don't occupy a seat.

**Early Access**

The Docker Admin Console is an [early access](https://docs.docker.com/release-lifecycle#early-access-ea) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

## [Add a company owner](https://docs.docker.com/admin/company/owners/#add-a-company-owner)

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. In the left navigation, select your company in the drop-down menu.
3. Select **Company owners**.
4. Select **Add owner**.
5. Specify the user's Docker ID to search for the user.
6. After you find the user, select **Add company owner**.

## [Remove a company owner](https://docs.docker.com/admin/company/owners/#remove-a-company-owner)

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. In the left navigation, select your company in the drop-down menu.
3. Select **Company owners**.
4. Select the **Action** icon in the row of the company owner that your want to remove.
5. Select **Remove as company owner**.
