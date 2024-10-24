+++
title = "Downgrade"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/subscription/core-subscription/downgrade/](https://docs.docker.com/subscription/core-subscription/downgrade/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Downgrade your subscription

You can downgrade your Docker subscription at anytime before the renewal date. The unused portion of the subscription isn't refundable or creditable.

When you downgrade your subscription, access to paid features is available until the next billing cycle. The downgrade takes effect on the next billing cycle.

> **Important**
>
> 
>
> If you downgrade your personal account from a Pro subscription to a Personal subscription, note that [Personal subscriptions](https://docs.docker.com/subscription/core-subscription/details/#docker-personal) don't include collaborators for private repositories, and only one private repository is included. When you downgrade, all collaborators will be removed and additional private repositories are locked.

## [Before you downgrade](https://docs.docker.com/subscription/core-subscription/downgrade/#before-you-downgrade)

Consider the following aspects before you downgrade your subscription.

### [Team size](https://docs.docker.com/subscription/core-subscription/downgrade/#team-size)

You may need to reduce the number of team members and convert any private repositories to public repositories or delete them. For information on features available in each tier, see [Docker Pricing](https://www.docker.com/pricing).

### [SSO and SCIM](https://docs.docker.com/subscription/core-subscription/downgrade/#sso-and-scim)

If you want to downgrade a Docker Business subscription and your organization uses single sign-on (SSO) for user authentication, you need to remove your SSO connection and verified domains before downgrading. After removing the SSO connection, any organization members that were auto-provisioned (for example, with SCIM) need to set up a password to sign in without SSO. To do this, users can [reset their password at sign in](https://docs.docker.com/accounts/create-account/#reset-your-password-at-sign-in).

## [Downgrade your Docker subscription](https://docs.docker.com/subscription/core-subscription/downgrade/#downgrade-your-docker-subscription)

> **Important**
>
> 
>
> If you have a [sales-assisted Docker Business subscription](https://docs.docker.com/subscription/core-subscription/details/#sales-assisted), contact your account manager to downgrade your subscription.

1. Sign in to [Docker Home](https://app.docker.com/).
2. Select your **avatar** and from the drop-down menu select **Billing**.
3. Choose either your personal account or an organization to downgrade.
4. In the plan section, select **Change plan**.
5. Select the plan you'd like to downgrade to.
6. Review the downgrade warning and select **Continue**.
7. Optional. Select a reason for your downgrade from the list and select **Send**. The **Billing** page displays a confirmation of the downgrade with details on when the downgrade changes take effect.

If you want to cancel the downgrade, select **Cancel the downgrade** on the **Plan** tab.

## [Pause a subscription](https://docs.docker.com/subscription/core-subscription/downgrade/#pause-a-subscription)

You can't pause or delay a subscription. If a subscription invoice hasn't been paid on the due date, there's a 15 day grace period, including the due date.
