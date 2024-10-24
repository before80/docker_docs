+++
title = "Onboard your organization"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/admin/organization/onboard/](https://docs.docker.com/admin/organization/onboard/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Onboard your organization

**Early Access**

The Docker Admin Console is an [early access]({{< ref "/manuals/Releaselifecycle#early-access-ea" >}}) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

Learn how to onboard your organization using Docker Hub or the Docker Admin Console.

Onboarding your organization lets you gain visibility into the activity of your users and enforce security settings. In addition, members of your organization receive increased pull limits and other organization wide benefits. For more details, see [Docker subscriptions and features]({{< ref "/manuals/Subscription/DockerCore/Subscriptionsandfeatures" >}}).

In this guide, you'll learn how to get started with the following:

- Identify your users to help you efficiently allocate your subscription seats
- Invite members and owners to your organization
- Secure authentication and authorization for your organization using Single Sign-On (SSO) and System for Cross-domain Identity Management (SCIM)
- Enforce sign-on for Docker Desktop to ensure security best practices

## Prerequisites

Before you start to onboard your organization, ensure that you:

- Have a Docker Team or Business subscription. See [Pricing & Subscriptions](https://www.docker.com/pricing/) for details.

  > **Note**
  >
  > 
  >
  > When purchasing a self-serve subscription, the on-screen instructions guide you through creating an organization. If you have purchased a subscription through Docker Sales and you have not yet created an organization, see [Create an organization]({{< ref "/manuals/Administration/Organizationadministration/Createyourorganization" >}}).

- Familiarize yourself with Docker concepts and terminology in the [glossary](https://docs.docker.com/glossary/) and [FAQs](https://docs.docker.com/faq/admin/general-faqs/).

## Step 1: Identify your Docker users and their Docker accounts

Identifying your users will ensure that you allocate your subscription seats efficiently and that all your Docker users receive the benefits of your subscription.

1. Identify the Docker users in your organization.
   - If your organization uses device management software, like MDM or JAMF, you may use the device management software to help identify Docker users. See your device management software's documentation for details. You can identify Docker users by checking if Docker Desktop is installed at the following location on each user's machine:
     - Mac: `/Applications/Docker.app`
     - Windows: `C:\Program Files\Docker\Docker`
     - Linux: `/opt/docker-desktop`
   - If your organization doesn't use device management software or your users haven't installed Docker Desktop yet, you may survey your users.
2. Instruct all your Docker users in your organization to update their existing Docker account's email address to an address that's in your organization's domain, or to create a new account using an email address in your organization's domain.
   - To update an account's email address, instruct your users to sign in to [Docker Hub](https://hub.docker.com/), and update the email address to their email address in your organization's domain.
   - To create a new account, instruct your users to go [sign up](https://hub.docker.com/signup) using their email address in your organization's domain.
3. Ask your Docker sales representative or [contact sales](https://www.docker.com/pricing/contact-sales/) to get a list of Docker accounts that use an email address in your organization's domain.

## Step 2: Invite owners

When you create an organization, you are the only owner. You may optionally add additional owners. Owners can help you onboard and manage your organization.

To add an owner, invite a user and assign them the owner role. For more details, see [Invite members]({{< ref "/manuals/Administration/Organizationadministration/Manageorganizationmembers" >}}).

## Step 3: Invite members

When you add users to your organization, you gain visibility into their activity and you can enforce security settings. In addition, members of your organization receive increased pull limits and other organization wide benefits.

To add a member, invite a user and assign them the member role. For more details, see [Invite members]({{< ref "/manuals/Administration/Organizationadministration/Manageorganizationmembers" >}}).

## Step 4: Manage members with SSO and SCIM

Configuring SSO and SCIM is optional and only available to Docker Business subscribers. To upgrade a Docker Team subscription to a Docker Business subscription, see [Upgrade your subscription](https://docs.docker.com/subscription/upgrade/).

You can manage your members in your identity provider and automatically provision them to your Docker organization with SSO and SCIM. See the following for more details.

- [Configure SSO]({{< ref "/manuals/Security/Foradmins/Singlesign-on" >}}) to authenticate and add members when they sign in to Docker through your identity provider.

- Optional:

   

  Enforce SSO

   

  to ensure that when users sign in to Docker, they must use SSO.

  > **Note**
  >
  > 
  >
  > Enforcing single sign-on (SSO) and [Step 5: Enforce sign-in for Docker Desktop](https://docs.docker.com/admin/organization/onboard/#step-5-enforce-sign-in-for-docker-desktop) are different features. For more details, see [Enforcing sign-in versus enforcing single sign-on (SSO)](https://docs.docker.com/security/for-admins/enforce-sign-in/#enforcing-sign-in-versus-enforcing-single-sign-on-sso).

- [Configure SCIM]({{< ref "/manuals/Security/Foradmins/Provisioning/SCIM" >}}) to automatically provision, add, and de-provision members to Docker through your identity provider.

## Step 5: Enforce sign-in for Docker Desktop

By default, members of your organization can use Docker Desktop without signing in. When users don’t sign in as a member of your organization, they don’t receive the [benefits of your organization’s subscription]({{< ref "/manuals/Subscription/DockerCore/Subscriptionsandfeatures" >}}) and they can circumvent [Docker’s security features]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop" >}}) for your organization.

There are multiple ways you can enforce sign-in, depending on your company's set up and preferences:

- [Registry key method (Windows only)](https://docs.docker.com/security/for-admins/enforce-sign-in/methods/#registry-key-method-windows-only)
- [`.plist` method (Mac only)](https://docs.docker.com/security/for-admins/enforce-sign-in/methods/#plist-method-mac-only)
- [`registry.json` method (All)](https://docs.docker.com/security/for-admins/enforce-sign-in/methods/#registryjson-method-all)

## What's next

- [Create]({{< ref "/manuals/DockerHub/Managerepositories/Createrepositories" >}}) and [manage]({{< ref "/manuals/DockerHub/Managerepositories" >}}) repositories.
- Create [teams]({{< ref "/manuals/Administration/Organizationadministration/Createandmanageateam" >}}) for fine-grained repository access.
- Configure [Hardened Docker Desktop](https://docs.docker.com/desktop/hardened-desktop/) to improve your organization’s security posture for containerized development.
- [Audit your domains](https://docs.docker.com/docker-hub/domain-audit/) to ensure that all Docker users in your domain are part of your organization.

Your Docker subscription provides many more additional features. To learn more, see [Docker subscriptions and features](https://docs.docker.com/subscription/details/).
