+++
title = "3D Secure authentication"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/billing/3d-secure/](https://docs.docker.com/billing/3d-secure/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# 3D Secure authentication

> **Note**
>
> 
>
> [Docker Core subscription]({{< ref "/manuals/Billing/DockerCore/GetstartedwithDockerCore" >}}) payments support 3D secure authentication.

3D Secure (3DS) authentication incorporates an additional security layer for credit card transactions. If you’re making payments for your Docker billing in a region that requires 3DS, or using a payment method that requires 3DS, you’ll need to verify your identity to complete any transactions. The method used to verify your identity varies depending on your banking institution.

The following transactions will use 3DS authentication if your payment method requires it.

- Starting a [new paid subscription]({{< ref "/manuals/Billing/DockerCore/GetstartedwithDockerCore" >}})
- Changing your [billing cycle]({{< ref "/manuals/Billing/DockerCore/Changeyourbillingcycle" >}}) from monthly to annual
- [Upgrading your subscription]({{< ref "/manuals/Subscription/DockerCore/Upgrade" >}})
- [Adding seats]({{< ref "/manuals/Subscription/DockerCore/Addseats" >}}) to an existing subscription

## Troubleshooting

If you encounter errors completing payments due to 3DS, you can troubleshoot in the following ways.

1. Retry your transaction and verification of your identity.
2. Contact your bank to determine any errors on their end.
3. Try a different payment method that doesn’t require 3DS.

> **Tip**
>
> 
>
> Make sure you allow third-party scripts in your browser and that any ad blocker you may use is disabled when attempting to complete payments.
