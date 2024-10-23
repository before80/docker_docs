+++
title = "Manage"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/security/for-admins/single-sign-on/manage/](https://docs.docker.com/security/for-admins/single-sign-on/manage/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Manage single sign-on

## [Manage organizations](https://docs.docker.com/security/for-admins/single-sign-on/manage/#manage-organizations)

> **Note**
>
> 
>
> You must have a [company](https://docs.docker.com/admin/company/) to manage more than one organization.

**Early Access**

The Docker Admin Console is an [early access](https://docs.docker.com/release-lifecycle#early-access-ea) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

### [Connect an organization](https://docs.docker.com/security/for-admins/single-sign-on/manage/#connect-an-organization)

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your company in the left navigation drop-down menu, and then select **SSO and SCIM**.
3. In the SSO connections table, select the **Action** icon and then **Edit connection**.
4. Select **Next** to navigate to the section where connected organizations are listed.
5. In the **Organizations** drop-down, select the organization to add to the connection.
6. Select **Next** to confirm or change the default organization and team provisioning.
7. Review the **Connection Summary** and select **Save**.

### [Remove an organization](https://docs.docker.com/security/for-admins/single-sign-on/manage/#remove-an-organization)

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your company in the left navigation drop-down menu, and then select **SSO and SCIM**.
3. In the SSO connections table, select the **Action** icon and then **Edit connection**.
4. Select **Next** to navigate to the section where connected organizations are listed.
5. In the **Organizations** drop-down, select **Remove** to remove the connection.
6. Select **Next** to confirm or change the default organization and team provisioning.
7. Review the **Connection Summary** and select **Save**.

## [Manage domains](https://docs.docker.com/security/for-admins/single-sign-on/manage/#manage-domains)

Admin Console Docker Hub

------

**Early Access**

The Docker Admin Console is an [early access](https://docs.docker.com/release-lifecycle#early-access-ea) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

### [Remove a domain from an SSO connection](https://docs.docker.com/security/for-admins/single-sign-on/manage/#remove-a-domain-from-an-sso-connection)

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your organization or company in the left navigation drop-down menu, and then select **SSO and SCIM**.
3. In the SSO connections table, select the **Action** icon and then **Edit connection**.
4. Select **Next** to navigate to the section where the connected domains are listed.
5. In the **Domain** drop-down, select the **x** icon next to the domain that you want to remove.
6. Select **Next** to confirm or change the connected organization(s).
7. Select **Next** to confirm or change the default organization and team provisioning selections.
8. Review the **Connection Summary** and select **Save**.

> **Note**
>
> If you want to re-add the domain, a new TXT record value is assigned. You must then complete the verification steps with the new TXT record value.

------

## [Manage SSO connections](https://docs.docker.com/security/for-admins/single-sign-on/manage/#manage-sso-connections)

Admin Console Docker Hub

------

**Early Access**

The Docker Admin Console is an [early access](https://docs.docker.com/release-lifecycle#early-access-ea) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

### [Edit a connection](https://docs.docker.com/security/for-admins/single-sign-on/manage/#edit-a-connection)

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your organization or company in the left navigation drop-down menu, and then select **SSO and SCIM**. Note that when an organization is part of a company, you must select the company and configure SSO for that organization at the company level. Each organization can have its own SSO configuration and domain, but it must be configured at the company level.
3. In the SSO connections table, select the **Action** icon.
4. Select **Edit connection** to edit your connection.
5. Follow the on-screen instructions to edit the connection.

### [Delete a connection](https://docs.docker.com/security/for-admins/single-sign-on/manage/#delete-a-connection)

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your organization or company in the left navigation drop-down menu, and then select **SSO and SCIM**. Note that when an organization is part of a company, you must select the company and configure SSO for that organization at the company level. Each organization can have its own SSO configuration and domain, but it must be configured at the company level.
3. In the SSO connections table, select the **Action** icon.
4. Select **Delete connection**.
5. Follow the on-screen instructions to delete a connection.

### [Deleting SSO](https://docs.docker.com/security/for-admins/single-sign-on/manage/#deleting-sso)

When you disable SSO, you can delete the connection to remove the configuration settings and the added domains. Once you delete this connection, it can't be undone. Users must authenticate with their Docker ID and password or create a password reset if they don't have one.

------

## [Manage users](https://docs.docker.com/security/for-admins/single-sign-on/manage/#manage-users)

Admin Console Docker Hub

------

**Early Access**

The Docker Admin Console is an [early access](https://docs.docker.com/release-lifecycle#early-access-ea) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

> **Important**
>
> 
>
> SSO has Just-In-Time (JIT) Provisioning enabled by default unless you have [disabled it](https://docs.docker.com/security/for-admins/provisioning/just-in-time/#sso-authentication-with-jit-provisioning-disabled). This means your users are auto-provisioned to your organization.
>
> You can change this on a per-app basis. To prevent auto-provisioning users, you can create a security group in your IdP and configure the SSO app to authenticate and authorize only those users that are in the security group. Follow the instructions provided by your IdP:
>
> - [Okta](https://help.okta.com/en-us/Content/Topics/Security/policies/configure-app-signon-policies.htm)
> - [Entra ID (formerly Azure AD)](https://learn.microsoft.com/en-us/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users)
>
> Alternatively, see [Manage how users are provisioned](https://docs.docker.com/security/for-admins/single-sign-on/manage/#manage-how-users-are-provisioned).

### [Add guest users when SSO is enabled](https://docs.docker.com/security/for-admins/single-sign-on/manage/#add-guest-users-when-sso-is-enabled)

To add a guest that isn't verified through your IdP:

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Navigate to the user management page for your organization or company.
   - Organization: Select your organization in the left navigation drop-down menu, and then select **Members**.
   - Company: Select your company in the left navigation drop-down menu, and then select **Users**.
3. Select **Invite**.
4. Follow the on-screen instructions to invite the user.

### [Remove users from the SSO company](https://docs.docker.com/security/for-admins/single-sign-on/manage/#remove-users-from-the-sso-company)

To remove a user:

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Navigate to the user management page for your organization or company.
   - Organization: Select your organization in the left navigation drop-down menu, and then select **Members**.
   - Company: Select your company in the left navigation drop-down menu, and then select **Users**.
3. Select the action icon next to a user’s name, and then select **Remove member**, if you're an organization, or **Remove user**, if you're a company.
4. Follow the on-screen instructions to remove the user.

### [Manage how users are provisioned](https://docs.docker.com/security/for-admins/single-sign-on/manage/#manage-how-users-are-provisioned)

Users are provisioned with JIT provisioning by default. If you enable SCIM, you can disable JIT:

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your organization or company in the left navigation drop-down menu, and then select **SSO and SCIM**.
3. In the SSO connections table, select the **Action** icon and then **Disable JIT provisioning**.
4. Select **Disable** to confirm.

------

## [What's next?](https://docs.docker.com/security/for-admins/single-sign-on/manage/#whats-next)

- [Set up SCIM](https://docs.docker.com/security/for-admins/provisioning/scim/)
- [Enable Group mapping](https://docs.docker.com/security/for-admins/provisioning/group-mapping/)
