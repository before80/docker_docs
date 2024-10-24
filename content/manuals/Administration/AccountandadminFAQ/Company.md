+++
title = "Company"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/admin/faqs/company-faqs/](https://docs.docker.com/admin/faqs/company-faqs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# FAQs on companies

### Are existing subscriptions affected when you create a company and add organizations to it?

You can manage subscriptions and related billing details at the organization level.

### Some of my organizations don’t have a Docker Business subscription. Can I still use a parent company?

Yes, but you can only add organizations with a Docker Business subscription to a company.

### What happens if one of my organizations downgrades from Docker Business, but I still need access as a company owner?

To access and manage child organizations, the organization must have a Docker Business subscription. If the organization isn’t included in this subscription, the owner of the organization must manage the organization outside of the company.

### Does my organization need to prepare for downtime during the migration process?

No, you can continue with business as usual.

### How many company owners can I add?

You can add a maximum of 10 company owners to a single company account.

### Do company owners occupy a subscription seat?

Company owners don't occupy a seat in any organization unless they are added as member of the organization. Since company owners have the same access as organization owners for all organizations associated with the company, it is not necessary to add company owners to an organization.

Note that when you first create a company, your account will be both a company owner and an organization owner. Your account will occupy a seat as long as you're an organization owner.

### What permissions does the company owner have in the associated/nested organizations?

Company owners can navigate to the **Organizations** page to view all their nested organizations in a single location. They can also view or edit organization members and change single sign-on (SSO) and System for Cross-domain Identity Management (SCIM) settings. Changes to company settings impact all users in each organization under the company. See [Roles and permissions]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}).

### What features are supported at the company level?

You can manage domain verification, SSO, and SCIM at the company level. The following features aren't supported at the company level, but you can manage them at the organization level:

- Image Access Management
- Registry Access Management
- User management
- Billing

To view and manage users across all the organizations under your company, you can [manage users at the company level]({{< ref "/manuals/Administration/Companyadministration/Managecompanyusers" >}}) when you use the [Admin Console](https://admin.docker.com/).

Domain audit isn't supported for companies or organizations within a company.

### What's required to create a company name?

A company name must be unique to that of its child organization. If a child organization requires the same name as a company, you should modify it slightly. For example, **Docker Inc** (parent company), **Docker** (child organization).

### How does a company owner add an organization to the company?

You can add organizations to a company in the [Admin Console](https://docs.docker.com/admin/company/organizations/#add-organizations-to-a-company.md).

### How does a company owner manage SSO/SCIM settings from a company?

See your [SCIM]({{< ref "/manuals/Security/Foradmins/Provisioning/SCIM" >}}) and [SSO]({{< ref "/manuals/Security/Foradmins/Singlesign-on/Configure" >}}) settings.

### How does a company owner enable group mapping in an IdP?

See [SCIM]({{< ref "/manuals/Security/Foradmins/Provisioning/SCIM" >}}) and [group mapping]({{< ref "/manuals/Security/Foradmins/Provisioning/Groupmapping" >}}) for more information.

### What's the definition of a company vs an organization?

A company is a collection of organizations that are managed together. An organization is a collection of repositories and teams that are managed together.
