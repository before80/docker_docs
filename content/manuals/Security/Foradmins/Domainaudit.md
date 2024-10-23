+++
title = "Domain audit"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/security/for-admins/domain-audit/](https://docs.docker.com/security/for-admins/domain-audit/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Domain audit

Domain audit identifies uncaptured users in an organization. Uncaptured users are Docker users who have authenticated to Docker using an email address associated with one of your verified domains, but they're not a member of your organization in Docker. You can audit domains on organizations that are part of the Docker Business subscription. To upgrade your existing account to a Docker Business subscription, see [Upgrade your subscription](https://docs.docker.com/subscription/upgrade/).

Uncaptured users who access Docker Desktop in your environment may pose a security risk because your organization's security settings, like Image Access Management and Registry Access Management, aren't applied to a user's session. In addition, you won't have visibility into the activity of uncaptured users. You can add uncaptured users to your organization to gain visibility into their activity and apply your organization's security settings.

Domain audit can't identify the following Docker users in your environment:

- Users who access Docker Desktop without authenticating
- Users who authenticate using an account that doesn't have an email address associated with one of your verified domains

Although domain audit can't identify all Docker users in your environment, you can enforce sign-in to prevent unidentifiable users from accessing Docker Desktop in your environment. For more details about enforcing sign-in, see [Configure registry.json to enforce sign-in](https://docs.docker.com/security/for-admins/enforce-sign-in/).

> **Tip**
>
> 
>
> You can use endpoint management (MDM) software to identify the number of Docker Desktop instances and their versions within your environment. This can provide accurate license reporting, help ensure your machines use the latest version of Docker Desktop, and enable you to [enforce sign-in](https://docs.docker.com/security/for-admins/enforce-sign-in/).
>
> - [Intune](https://learn.microsoft.com/en-us/mem/intune/apps/app-discovered-apps)
> - [Jamf](https://docs.jamf.com/10.25.0/jamf-pro/administrator-guide/Application_Usage.html)
> - [Kandji](https://support.kandji.io/support/solutions/articles/72000559793-view-a-device-application-list)
> - [Kolide](https://www.kolide.com/features/device-inventory/properties/mac-apps)
> - [Workspace One](https://blogs.vmware.com/euc/2022/11/how-to-use-workspace-one-intelligence-to-manage-app-licenses-and-reduce-costs.html)

## [Prerequisites](https://docs.docker.com/security/for-admins/domain-audit/#prerequisites)

Before you audit your domains, review the following required prerequisites:

- Your organization must be part of a Docker Business subscription. To upgrade your existing account to a Docker Business subscription, see [Upgrade your subscription](https://docs.docker.com/subscription/core-subscription/upgrade/).
- You must [add and verify your domains](https://docs.docker.com/security/for-admins/single-sign-on/configure/#step-one-add-and-verify-your-domain).

> **Important**
>
> 
>
> Domain audit is not supported for companies or organizations within a company.

## [Audit your domains for uncaptured users](https://docs.docker.com/security/for-admins/domain-audit/#audit-your-domains-for-uncaptured-users)

Docker Hub Admin Console

------

To audit your domains:

1. Sign in to [Docker Hub](https://hub.docker.com/).
2. Select **Organizations**, your organization, **Settings**, and then **Security**.
3. In **Domain Audit**, select **Export Users** to export a CSV file of uncaptured users with the following columns:
   - Name: The name of the user.
   - Username: The Docker ID of the user.
   - Email: The email address of the user.

You can invite all the uncaptured users to your organization using the exported CSV file. For more details, see [Invite members](https://docs.docker.com/admin/organization/members/). Optionally, enforce single sign-on or enable SCIM to add users to your organization automatically. For more details, see [SSO](https://docs.docker.com/security/for-admins/single-sign-on/) or [SCIM](https://docs.docker.com/security/for-admins/provisioning/scim/).

> **Note**
>
> Domain audit may identify accounts of users who are no longer a part of your organization. If you don't want to add a user to your organization and you don't want the user to appear in future domain audits, you must deactivate the account or update the associated email address.
>
> Only someone with access to the Docker account can deactivate the account or update the associated email address. For more details, see [Deactivating an account](https://docs.docker.com/admin/deactivate-account/).
