+++
title = "FAQs"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/billing/faqs/](https://docs.docker.com/billing/faqs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Billing FAQs

### What credit and debit cards are supported?

- Visa
- MasterCard
- American Express
- Discover
- JCB
- Diners
- UnionPay

### What currency is supported?

United States dollar (USD).

### What happens if my subscription payment fails?

If your subscription payment fails, there is a grace period of 15 days, including the due date. Docker retries to collect the payment 3 times using the following schedule:

- 3 days after the due date
- 5 days after the previous attempt
- 7 days after the previous attempt

Docker also sends an email notification `Action Required - Credit Card Payment Failed` with an attached unpaid invoice after each failed payment attempt.

Once the grace period is over and the invoice is still not paid, the subscription downgrades to a free plan and all paid features are disabled.

### Does Docker collect sales tax and/or VAT?

Starting July 1, 2024, Docker will begin collecting sales tax on subscription fees in compliance with state regulations for customers in the United States. For global customers subject to VAT, the implementation will start rolling out on July 1, 2024. Note that while the rollout begins on this date, VAT charges may not apply to all applicable subscriptions immediately.

To ensure that tax assessments are correct, make sure that your billing information and VAT/Tax ID, if applicable, are updated. See [Update the billing information]({{< ref "/manuals/Billing/DockerCore/Updatethebillinginformation" >}}).

### How do I certify my tax exempt status?

If you're exempt from sales tax, you can [register a valid tax exemption certificate]({{< ref "/manuals/Billing/Registerataxcertificate" >}}) with Docker's Support team. [Contact Support](https://hub.docker.com/support/contact) to get started.

### Does Docker offer academic pricing?

Contact the [Docker Sales Team](https://www.docker.com/company/contact).

### Do I need to do anything at the end of my subscription term?

No. All monthly and annual subscriptions are automatically renewed at the end of the term using the original form of payment.
