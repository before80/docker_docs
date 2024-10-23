+++
title = "Connect"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/security/for-admins/single-sign-on/connect/](https://docs.docker.com/security/for-admins/single-sign-on/connect/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Complete your single sign-on connection

The steps to set up your SSO configuration are:

1. [Add and verify the domain or domains](https://docs.docker.com/security/for-admins/single-sign-on/configure#step-one-add-and-verify-your-domain) that your members use to sign in to Docker.
2. [Create your SSO connection](https://docs.docker.com/security/for-admins/single-sign-on/configure#step-two-create-an-sso-connection-in-docker) in Docker.
3. [Configure your IdP](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp#step-three-configure-your-idp-to-work-with-docker) to work with Docker.
4. [Complete your SSO connection](https://docs.docker.com/security/for-admins/single-sign-on/connect/#step-four-complete-your-sso-connection) in Docker.

This page walks you through the final steps of creating your SSO connection. You can then test your connection and optionally enforce SSO for your organization.

## [Prerequisites](https://docs.docker.com/security/for-admins/single-sign-on/connect/#prerequisites)

Make sure you have completed the following before you begin:

- Your domain is verified
- You have created your SSO connection in Docker
- You configured your IdP using the appropriate values from your Docker connection
- You have pasted the following from your IdP into the settings in the Docker console:
  - SAML: **SAML Sign-on URL**, **x509 Certificate**
  - Azure AD (OIDC): **Client ID**, **Client Secret**, **Azure AD Domain**

## [Step four: Complete your SSO connection](https://docs.docker.com/security/for-admins/single-sign-on/connect/#step-four-complete-your-sso-connection)

Admin Console Docker Hub

------

1. In the [Admin Console](https://admin.docker.com/), select the verified domains you want to apply the connection to.
2. To provision your users, select the organization(s) and/or team(s).
3. Review your summary and select **Create Connection**.

## [Test your SSO configuration](https://docs.docker.com/security/for-admins/single-sign-on/connect/#test-your-sso-configuration)

After you’ve completed the SSO configuration process in Docker, you can test the configuration when you sign in to the [Admin Console](https://admin.docker.com/) using an incognito browser. Sign in to the [Admin Console](https://admin.docker.com/) using your domain email address. You are then redirected to your IdP's login page to authenticate.

1. Authenticate through email instead of using your Docker ID, and test the login process.
2. To authenticate through CLI, your users must have a PAT before you enforce SSO for CLI users.

> **Important**
>
> 
>
> SSO has Just-in-Time (JIT) provisioning enabled by default, unless you have [disabled it](https://docs.docker.com/security/for-admins/provisioning/just-in-time/#sso-authentication-with-jit-provisioning-disabled). This means your users are auto-provisioned to your organization.
>
> You can change this on a per-app basis. To prevent auto-provisioning users, you can create a security group in your IdP and configure the SSO app to authenticate and authorize only those users that are in the security group. Follow the instructions provided by your IdP:
>
> - [Okta](https://help.okta.com/en-us/Content/Topics/Security/policies/configure-app-signon-policies.htm)
> - [Entra ID (formerly Azure AD)](https://learn.microsoft.com/en-us/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users)
>
> Alternatively, see [Manage how users are provisioned](https://docs.docker.com/security/for-admins/single-sign-on/manage/#manage-how-users-are-provisioned).

The SSO connection is now created. You can continue to set up SCIM without enforcing SSO log-in. For more information about setting up SCIM, see [Set up SCIM](https://docs.docker.com/security/for-admins/provisioning/scim/).

## [Optional: Enforce SSO](https://docs.docker.com/security/for-admins/single-sign-on/connect/#optional-enforce-sso)

1. Sign in to the [Admin Console](https://admin.docker.com/).

2. Select your organization or company in the left navigation drop-down menu, and then select **SSO and SCIM**. Note that when an organization is part of a company, you must select the company and configure SSO for that organization at the company level. Each organization can have its own SSO configuration and domain, but it must be configured at the company level.

3. In the SSO connections table, select the **Action** icon and then **Enable enforcement**.

   When SSO is enforced, your users are unable to modify their email address and password, convert a user account to an organization, or set up 2FA through Docker Hub. You must enable 2FA through your IdP.

4. Continue with the on-screen instructions and verify that you’ve completed the tasks.

5. Select **Turn on enforcement** to complete.

Your users must now sign in to Docker with SSO.

> **Important**
>
> 
>
> If SSO isn't enforced, users can choose to sign in with either their Docker ID or SSO.

------

## [More resources](https://docs.docker.com/security/for-admins/single-sign-on/connect/#more-resources)

The following videos demonstrate how to enforce SSO.

- [Video: Enforce SSO with Okta SAML](https://youtu.be/c56YECO4YP4?feature=shared&t=1072)
- [Video: Enforce SSO with Azure AD (OIDC)](https://youtu.be/bGquA8qR9jU?feature=shared&t=1087)

## [What's next](https://docs.docker.com/security/for-admins/single-sign-on/connect/#whats-next)

Learn how you can [manage your SSO connection](https://docs.docker.com/security/for-admins/single-sign-on/manage/), domain, and users for your organization or company.
