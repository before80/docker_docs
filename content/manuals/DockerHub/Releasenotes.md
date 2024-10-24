+++
title = "Release notes"
date = 2024-10-23T14:54:40+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/docker-hub/release-notes/](https://docs.docker.com/docker-hub/release-notes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker Hub release notes

Here you can learn about the latest changes, new features, bug fixes, and known issues for each Docker Hub release.

Take a look at the [Docker Public Roadmap](https://github.com/orgs/docker/projects/51/views/1?filterQuery=) to see what's coming next.

## [2024-03-23](https://docs.docker.com/docker-hub/release-notes/#2024-03-23)

### [New](https://docs.docker.com/docker-hub/release-notes/#new)

- You can tag Docker Hub repositories with [categories](https://docs.docker.com/docker-hub/repos/categories/).

## [2023-12-11](https://docs.docker.com/docker-hub/release-notes/#2023-12-11)

- The Advanced Image Management feature, along with the corresponding API endpoints, has been retired. See [docker/roadmap#534](https://github.com/docker/roadmap/issues/534).

  The following API endpoints have been removed:

  

  ```text
  /namespaces/{namespace}/repositories/{repository}/images
  /namespaces/{namespace}/repositories/{repository}/images/{digest}/tags
  /namespaces/{namespace}/repositories/{repository}/images-summary
  /namespaces/{namespace}/delete-images
  ```

## [2023-08-28](https://docs.docker.com/docker-hub/release-notes/#2023-08-28)

- Organizations with SSO enabled can assign members to roles, organizations, and teams with [SCIM role mapping](https://docs.docker.com/security/for-admins/provisioning/scim/#set-up-role-mapping).

## [2023-07-26](https://docs.docker.com/docker-hub/release-notes/#2023-07-26)

### [New](https://docs.docker.com/docker-hub/release-notes/#new-1)

- Organizations can assign the [editor role](https://docs.docker.com/security/for-admins/roles-and-permissions/) to members to grant additional permissions without full administrative access.

## [2023-05-09](https://docs.docker.com/docker-hub/release-notes/#2023-05-09)

### [New](https://docs.docker.com/docker-hub/release-notes/#new-2)

- Docker Business subscribers can now [create a company](https://docs.docker.com/admin/company/new-company/) in Docker Hub to manage organizations and settings.

## [2023-03-07](https://docs.docker.com/docker-hub/release-notes/#2023-03-07)

### [New](https://docs.docker.com/docker-hub/release-notes/#new-3)

- You can now automatically sync user updates with your Docker organizations and teams with [Group Mapping](https://docs.docker.com/security/for-admins/provisioning/group-mapping/) for SSO and SCIM provisioning.

## [2022-12-12](https://docs.docker.com/docker-hub/release-notes/#2022-12-12)

### [New](https://docs.docker.com/docker-hub/release-notes/#new-4)

- The new domain audit feature lets you audit your domains for users who aren't a member of your organization.

## [2022-09-26](https://docs.docker.com/docker-hub/release-notes/#2022-09-26)

### [New](https://docs.docker.com/docker-hub/release-notes/#new-5)

- The new [autobuild feature](https://docs.docker.com/docker-hub/builds/manage-builds/#check-your-active-builds) lets you view your in-progress logs every 30 seconds instead of when the build is complete.

## [2022-09-21](https://docs.docker.com/docker-hub/release-notes/#2022-09-21)

### [Bug fixes and enhancements](https://docs.docker.com/docker-hub/release-notes/#bug-fixes-and-enhancements)

- In Docker Hub, you can now download a [registry.json](https://docs.docker.com/security/for-admins/enforce-sign-in/) file or copy the commands to create a registry.json file to enforce sign-in for your organization.

## [2022-09-19](https://docs.docker.com/docker-hub/release-notes/#2022-09-19)

### [Bug fixes and enhancements](https://docs.docker.com/docker-hub/release-notes/#bug-fixes-and-enhancements-1)

- You can now [export a CSV file of members](https://docs.docker.com/admin/organization/members/#export-members) from organizations that you own.

## [2022-07-22](https://docs.docker.com/docker-hub/release-notes/#2022-07-22)

### [Bug fixes and enhancements](https://docs.docker.com/docker-hub/release-notes/#bug-fixes-and-enhancements-2)

- You can now invite members to your organization with a CSV file containing their email addresses. The CSV file can either be a file you create for this specific purpose or one that’s extracted from another in-house system.

## [2022-07-19](https://docs.docker.com/docker-hub/release-notes/#2022-07-19)

### [Bug fixes and enhancements](https://docs.docker.com/docker-hub/release-notes/#bug-fixes-and-enhancements-3)

- SCIM is now available for organizations with a Docker Business subscription using an Entra ID (formerly Azure AD) identity provider.

## [2022-06-23](https://docs.docker.com/docker-hub/release-notes/#2022-06-23)

### [New](https://docs.docker.com/docker-hub/release-notes/#new-6)

- With [SCIM](https://docs.docker.com/security/for-admins/provisioning/scim/), you can manage users within your Okta identity provider (IdP). In addition, you can enable SCIM on organizations that are part of the Docker Business subscription.

## [2022-05-24](https://docs.docker.com/docker-hub/release-notes/#2022-05-24)

### [New](https://docs.docker.com/docker-hub/release-notes/#new-7)

- [Registry Access Management](https://docs.docker.com/security/for-admins/hardened-desktop/registry-access-management/) is now available for all Docker Business subscriptions. When enabled, your users can access specific registries in Docker Hub.

## [2022-05-03](https://docs.docker.com/docker-hub/release-notes/#2022-05-03)

### [New](https://docs.docker.com/docker-hub/release-notes/#new-8)

- Organization owners can [invite new members](https://docs.docker.com/admin/organization/members/#invite-members) to an organization via Docker ID or email address.

## [2021-11-15](https://docs.docker.com/docker-hub/release-notes/#2021-11-15)

### [New](https://docs.docker.com/docker-hub/release-notes/#new-9)

- You can now purchase or upgrade to a Docker Business subscription using a credit card. To learn more, see [Upgrade your subscription](https://docs.docker.com/subscription/core-subscription/upgrade/).

## [2021-08-31](https://docs.docker.com/docker-hub/release-notes/#2021-08-31)

### [New](https://docs.docker.com/docker-hub/release-notes/#new-10)

Docker has [announced](https://www.docker.com/blog/updating-product-subscriptions/) updates and extensions to the product subscriptions to increase productivity, collaboration, and added security for our developers and businesses. Docker subscription tiers now include Personal, Pro, Team, and Business.

The updated [Docker Subscription Service Agreement](https://www.docker.com/legal/docker-subscription-service-agreement) includes a change to the terms for **Docker Desktop**.

- Docker Desktop **remains free** for small businesses (fewer than 250 employees AND less than $10 million in annual revenue), personal use, education, and non-commercial open source projects.

- It requires a paid subscription (**Pro, Team, or Business**), for as little as $5 a month, for professional use in larger enterprises.

- The effective date of these terms is August 31, 2021. There is a grace period until January 31, 2022 for those that will require a paid subscription to use Docker Desktop.

- The Docker Pro and Docker Team subscriptions now **include commercial use** of Docker Desktop.

- The existing Docker Free subscription has been renamed **Docker Personal**.

- **No changes** to Docker Engine or any other upstream **open source** Docker or Moby project.

  To understand how these changes affect you, read the [FAQs](https://www.docker.com/pricing/faq). For more information, see [Docker subscription overview](https://docs.docker.com/subscription/).

## [2021-05-05](https://docs.docker.com/docker-hub/release-notes/#2021-05-05)

### [Enhancement](https://docs.docker.com/docker-hub/release-notes/#enhancement)

When managing the content of your repositories, you can now filter the results based on the currentness of the tags and more easily identify your untagged images.

For Docker Hub API documentation, see [Docker Hub API Reference](https://docs.docker.com/reference/api/hub/latest/#operation/GetNamespacesRepositoriesImages).

## [2021-04-13](https://docs.docker.com/docker-hub/release-notes/#2021-04-13)

### [Enhancement](https://docs.docker.com/docker-hub/release-notes/#enhancement-1)

The **Billing Details** page now shows any organizations you own, in addition to your personal account. This allows you to clearly identify the billing details of your chosen namespace, and enables you to switch between your personal and your organization accounts to view or update the details.

## [2021-04-09](https://docs.docker.com/docker-hub/release-notes/#2021-04-09)

### [Enhancement](https://docs.docker.com/docker-hub/release-notes/#enhancement-2)

You can now specify any email address to receive billing-related emails for your organization. The email address doesn't have to be associated with an organization owner account. You must be an owner of the organization to update any billing details.

To change the email address receiving billing-related emails, log into Docker Hub and navigate to the **Billing** tab of your organization. Select **Payment Methods** > **Billing Information**. Enter the new email address that you'd like to use in the **Email** field. Click **Update** for the changes to take effect.

For details on how to update your billing information, see [Update billing information](https://docs.docker.com/billing/).

## [2021-03-22](https://docs.docker.com/docker-hub/release-notes/#2021-03-22)

### [New feature](https://docs.docker.com/docker-hub/release-notes/#new-feature)

**Advanced Image Management dashboard**

Docker introduces the Advanced Image Management dashboard that enables you to view and manage Docker images in your repositories.

## [2021-01-25](https://docs.docker.com/docker-hub/release-notes/#2021-01-25)

### [New feature](https://docs.docker.com/docker-hub/release-notes/#new-feature-1)

Docker introduces Audit logs, a new feature that allows team owners to view a list of activities that occur at organization and repository levels. This feature begins tracking the activities from the release date, that is, **from 25 January 2021**.

For more information about this feature and for instructions on how to use it, see [Activity logs](https://docs.docker.com/admin/organization/activity-logs/).

## [2020-11-10](https://docs.docker.com/docker-hub/release-notes/#2020-11-10)

### [New feature](https://docs.docker.com/docker-hub/release-notes/#new-feature-2)

The **Repositories** view now shows which images have gone stale because they haven't been pulled or pushed recently. For more information, see [repository tags](https://docs.docker.com/docker-hub/repos/access/#view-repository-tags).

## [2020-10-07](https://docs.docker.com/docker-hub/release-notes/#2020-10-07)

### [New feature](https://docs.docker.com/docker-hub/release-notes/#new-feature-3)

Docker introduces Hub Vulnerability Scanning which enables you to automatically scan Docker images for vulnerabilities using Snyk. For more information, see [Hub Vulnerability Scanning](https://docs.docker.com/docker-hub/vulnerability-scanning/).

## [2020-05-14](https://docs.docker.com/docker-hub/release-notes/#2020-05-14)

### [New features](https://docs.docker.com/docker-hub/release-notes/#new-features)

- Docker has announced a new, per-seat pricing model to accelerate developer workflows for cloud-native development. The previous private repository/concurrent autobuild-based plans have been replaced with new **Pro** and **Team** plans that include unlimited private repositories. For more information, see [Docker subscription](https://docs.docker.com/subscription/).
- Docker has enabled download rate limits for downloads and pull requests on Docker Hub. This caps the number of objects that users can download within a specified timeframe. For more information, see [Download rate limit](https://docs.docker.com/docker-hub/download-rate-limit/).

## [2019-11-04](https://docs.docker.com/docker-hub/release-notes/#2019-11-04)

### [Enhancements](https://docs.docker.com/docker-hub/release-notes/#enhancements)

- The [repositories page](https://docs.docker.com/docker-hub/repos/) and all related settings and tabs have been updated and moved from `cloud.docker.com` to `hub.docker.com`. You can access the page at its new URL: https://hub.docker.com/repositories.

### [Known Issues](https://docs.docker.com/docker-hub/release-notes/#known-issues)

- Scan results don't appear for some official images.

## [2019-10-21](https://docs.docker.com/docker-hub/release-notes/#2019-10-21)

### [New features](https://docs.docker.com/docker-hub/release-notes/#new-features-1)

- **Beta:** Docker Hub now supports two-factor authentication (2FA). Enable it in your account settings, under the **[Security](https://hub.docker.com/settings/security)** section.

  > If you lose both your 2FA authentication device and recovery code, you may not be able to recover your account.

### [Enhancements](https://docs.docker.com/docker-hub/release-notes/#enhancements-1)

- As a security measure, when two-factor authentication is enabled, the Docker CLI requires a personal access token instead of a password to log in.

### [Known Issues](https://docs.docker.com/docker-hub/release-notes/#known-issues-1)

- Scan results don't appear for some official images.

## [2019-10-02](https://docs.docker.com/docker-hub/release-notes/#2019-10-02)

### [Enhancements](https://docs.docker.com/docker-hub/release-notes/#enhancements-2)

- You can now manage teams and members straight from your

   

  organization page

  . Each organization page now breaks down into these tabs:

  - **New:** Members - manage your members directly from this page (delete, add, or open their teams)
  - **New:** Teams - search by team or username, and open up any team page to manage the team
  - **New:** Invitees (conditional tab, only if an invite exists) - resend or remove invitiations from this tab
  - Repositories
  - Settings
  - Billing

### [Bug fixes](https://docs.docker.com/docker-hub/release-notes/#bug-fixes)

- Fixed an issue where Kinematic could not connect and log in to Docker Hub.

### [Known Issues](https://docs.docker.com/docker-hub/release-notes/#known-issues-2)

- Scan results don't appear for some official images.

## [2019-09-19](https://docs.docker.com/docker-hub/release-notes/#2019-09-19)

### [New features](https://docs.docker.com/docker-hub/release-notes/#new-features-2)

- You can now [create personal access tokens](https://docs.docker.com/security/for-developers/access-tokens/) in Docker Hub and use them to authenticate from the Docker CLI. Find them in your account settings, under the new **[Security](https://hub.docker.com/settings/security)** section.

### [Known Issues](https://docs.docker.com/docker-hub/release-notes/#known-issues-3)

- Scan results don't appear for some official images.

## [2019-09-16](https://docs.docker.com/docker-hub/release-notes/#2019-09-16)

### [Enhancements](https://docs.docker.com/docker-hub/release-notes/#enhancements-3)

- The [billing page](https://docs.docker.com/subscription/core-subscription/upgrade/) for personal accounts has been updated. You can access the page at its new URL: https://hub.docker.com/billing/plan.

### [Known Issues](https://docs.docker.com/docker-hub/release-notes/#known-issues-4)

- Scan results don't appear for some official images.

## [2019-09-05](https://docs.docker.com/docker-hub/release-notes/#2019-09-05)

### [Enhancements](https://docs.docker.com/docker-hub/release-notes/#enhancements-4)

- The `Tags` tab on an image page now provides additional information for each tag:
  - A list of digests associated with the tag
  - The architecture it was built on
  - The OS
  - The user who most recently updated an image for a specific tag
- The security scan summary for Docker Official Images has been updated.

### [Known Issues](https://docs.docker.com/docker-hub/release-notes/#known-issues-5)

- Scan results don't appear for some official images.
