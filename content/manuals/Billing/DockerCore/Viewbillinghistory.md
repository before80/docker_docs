+++
title = "View billing history"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/billing/core-billing/history/](https://docs.docker.com/billing/core-billing/history/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# View billing history

In this section, learn how you can view your billing history, manage your invoices, and verify your renewal date. All monthly and annual subscriptions are automatically renewed at the end of the term using the original form of payment.

> **Important**
>
> 
>
> Starting July 1, 2024, Docker will begin collecting sales tax on subscription fees in compliance with state regulations for customers in the United States. For our global customers subject to VAT, the implementation will start rolling out on July 1, 2024. Note that while the rollout begins on this date, VAT charges may not apply to all applicable subscriptions immediately.
>
> To ensure that tax assessments are correct, make sure that your [billing information]({{< ref "/manuals/Billing/DockerCore/Updatethebillinginformation" >}}) and VAT/Tax ID, if applicable, are updated. If you're exempt from sales tax, see [Register a tax certificate]({{< ref "/manuals/Billing/Registerataxcertificate" >}}).

## Invoices

Your invoice includes the following:

- Invoice number
- Date of issue
- Date due
- Your bill to information
- Amount due
- Description of your order, quantity if applicable, unit price, and amount

Amounts are in USD.

The information listed in the **Bill** to section is based on your billing information. Not all fields are required. The billing information includes the following:

- Name (required) - the name of the administrator or company.
- Email address (required) - the email address that receives all billing-related emails for the account.
- Address (required)
- Phone number
- Tax ID or VAT

You can’t make changes to a paid or unpaid billing invoice. When you update your billing information, this change won't update an existing invoice. If you need to update your billing information, make sure you do so before your subscription renewal date when your invoice is finalized. For more information, see [Update the billing information]({{< ref "/manuals/Billing/DockerCore/Updatethebillinginformation" >}}).

### View renewal date

You receive your invoice when the subscription renews. To verify your renewal date, sign in to Hub, then:

1. Select your user avatar to open the drop-down menu.
2. Select **Billing**.
3. Select the user or organization account to view the billing details. Here you can find your renewal date and the renewal amount.

### Include your VAT number on your invoice

Update your billing information to include your VAT number:

1. Sign in to Docker Hub.
2. For user accounts, Select your avatar in the top-right corner, then **Billing**. For organizations, select the name of the organization.
3. Select the **Payment methods and billing history** link.
4. In the **Billing Information** section, select **Update information**.
5. In the **Tax ID** section, select the ID type and enter your VAT number.
6. Select **Save**.

Your VAT number will be included on your next invoice.

## View billing history

You can view the billing history and download past invoices for a personal account or organization.

### Personal account

1. Select your avatar in the top-right corner of Docker Hub.
2. From the drop-down menu select **Billing**.
3. Select the **Payment methods and billing history** link. You can find your past invoices in the **Invoice History** section.

From here you can download an invoice.

### Organization

> **Note**
>
> 
>
> You must be an owner of the organization to view the billing history.

1. Select your avatar in the top-right corner of Docker Hub.
2. From the drop-down menu select **Billing**.
3. Select the organization that you want to change the payment method for.
4. Select the **Payment methods and billing history** link. You can find your past invoices in the **Invoice History** section.

From here you can download an invoice.
