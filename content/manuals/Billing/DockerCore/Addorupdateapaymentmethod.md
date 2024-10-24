+++
title = "Add or update a payment method"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/billing/core-billing/payment-method/](https://docs.docker.com/billing/core-billing/payment-method/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Add or update a payment method

This page describes how to add or update a payment method for your personal account or for an organization.

You can add a payment method or update your account's existing payment method at any time.

> **Important**
>
> 
>
> If you want to remove all payment methods, you must first downgrade your subscription to a free plan. See [Downgrade]({{< ref "/manuals/Subscription/DockerCore/Downgrade" >}}).

The following payment methods are supported:

- Visa
- MasterCard
- American Express
- Discover
- JCB
- Diners
- UnionPay

All currency, for example the amount listed on your billing invoice, is in United States dollar (USD).

> **Important**
>
> 
>
> Starting July 1, 2024, Docker will begin collecting sales tax on subscription fees in compliance with state regulations for customers in the United States. For our global customers subject to VAT, the implementation will start rolling out on July 1, 2024. Note that while the rollout begins on this date, VAT charges may not apply to all applicable subscriptions immediately.
>
> To ensure that tax assessments are correct, make sure that your [billing information]({{< ref "/manuals/Billing/DockerCore/Updatethebillinginformation" >}}) and VAT/Tax ID, if applicable, are updated. If you're exempt from sales tax, see [Register a tax certificate]({{< ref "/manuals/Billing/Registerataxcertificate" >}}).

## Manage payment method

### Personal account

1. Select your avatar in the top-right corner of Docker Hub.
2. From the drop-down menu select **Billing**.
3. Select the **Payment methods and billing history** link.
4. In the **Payment method** section, select **Add payment method**.
5. Enter your new payment information, then select **Add**.
6. Select the **Actions** icon, then select **Make default** to ensure that your new payment method applies to all purchases and subscriptions.
7. Optional. You can remove non-default payment methods by selecting the **Actions** icon. Then, select **Delete**.

### Organization

> **Note**
>
> 
>
> You must be an organization owner to make changes to the payment information.

1. Select your avatar in the top-right corner of Docker Hub.
2. From the drop-down menu select **Billing**.
3. Select the organization account you want to update.
4. Select the **Payment methods and billing history** link.
5. In the **Payment Method** section, select **Add payment method**.
6. Enter your new payment information, then select **Add**.
7. Select the **Actions** icon, then select **Make default** to ensure that your new payment method applies to all purchases and subscriptions.
8. Optional. You can remove non-default payment methods by selecting the **Actions** icon. Then, select **Delete**.

## Failed payments

If your subscription payment fails, there is a grace period of 15 days, including the due date. Docker retries to collect the payment 3 times using the following schedule:

- 3 days after the due date
- 5 days after the previous attempt
- 7 days after the previous attempt

Docker also sends an email notification `Action Required - Credit Card Payment Failed` with an attached unpaid invoice after each failed payment attempt.

Once the grace period is over and the invoice is still not paid, the subscription downgrades to a free plan and all paid features are disabled.

## Redeem a coupon

You can redeem a coupon for any paid Docker subscription.

A coupon can be used when you:

- Sign up to a new paid subscription from a free subscription
- Upgrade an existing paid subscription

You are asked to enter your coupon code when you confirm or enter your payment method.

If you use a coupon to pay for a subscription, when the coupon expires, your payment method is charged the full cost of your subscription. If you don't have a saved payment method, your account downgrades to a free subscription.
