+++
title = "Organization"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/admin/faqs/organization-faqs/](https://docs.docker.com/admin/faqs/organization-faqs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# FAQs on organizations

### [What if the Docker ID I want for my organization or company is taken?](https://docs.docker.com/admin/faqs/organization-faqs/#what-if-the-docker-id-i-want-for-my-organization-or-company-is-taken)

All Docker IDs are first-come, first-served except for companies that have a US Trademark on a username. If you have a trademark for your namespace, [Docker Support](https://hub.docker.com/support/contact/) can retrieve the Docker ID for you.

### [How do I add an organization owner?](https://docs.docker.com/admin/faqs/organization-faqs/#how-do-i-add-an-organization-owner)

An existing owner can add additional team members as organization owners. You can [invite a member](https://docs.docker.com/admin/organization/members/#invite-members) and assign them the owner role in Docker Hub or the Docker Admin Console.

### [How do I know how many active users are part of my organization?](https://docs.docker.com/admin/faqs/organization-faqs/#how-do-i-know-how-many-active-users-are-part-of-my-organization)

If your organization uses a Software Asset Management tool, you can use it to find out how many users have Docker Desktop installed. If your organization doesn't use this software, you can run an internal survey to find out who is using Docker Desktop. See [Identify your Docker users and their Docker accounts](https://docs.docker.com/admin/organization/onboard/#step-1-identify-your-docker-users-and-their-docker-accounts). With a Docker Business subscription, you can manage members in your identity provider and automatically provision them to your Docker organization with [SSO](https://docs.docker.com/security/for-admins/single-sign-on/) or [SCIM](https://docs.docker.com/security/for-admins/provisioning/scim/).

### [Do users first need to authenticate with Docker before an owner can add them to an organization?](https://docs.docker.com/admin/faqs/organization-faqs/#do-users-first-need-to-authenticate-with-docker-before-an-owner-can-add-them-to-an-organization)

No. Organization owners can invite users with their email address, and also assign them to a team during the invite process.

### [Can I force my organization's members to authenticate before using Docker Desktop and are there any benefits?](https://docs.docker.com/admin/faqs/organization-faqs/#can-i-force-my-organizations-members-to-authenticate-before-using-docker-desktop-and-are-there-any-benefits)

Yes. You can [enforce sign-in](https://docs.docker.com/security/for-admins/enforce-sign-in/) and some benefits are:

- Administrators can enforce features like [Image Access Management](https://docs.docker.com/security/for-admins/hardened-desktop/image-access-management/) and [Registry Access Management](https://docs.docker.com/security/for-admins/hardened-desktop/registry-access-management/).
- Administrators can ensure compliance by blocking Docker Desktop usage for users who don't sign in as members of the organization.

### [If a user has their personal email associated with a user account in Docker Hub, do they have to convert to using the organization's domain before they can be invited to join an organization?](https://docs.docker.com/admin/faqs/organization-faqs/#if-a-user-has-their-personal-email-associated-with-a-user-account-in-docker-hub-do-they-have-to-convert-to-using-the-organizations-domain-before-they-can-be-invited-to-join-an-organization)

Yes. When SSO is enabled for your organization, each user must sign in with the company’s domain. However, the user can retain their personal credentials and create a new Docker ID associated with their organization's domain.

### [Can I convert my personal user account (Docker ID) to an organization account?](https://docs.docker.com/admin/faqs/organization-faqs/#can-i-convert-my-personal-user-account-docker-id-to-an-organization-account)

Yes. You can convert your user account to an organization account. Once you convert a user account into an organization, it's not possible to revert it to a personal user account. For prerequisites and instructions, see [Convert an account into an organization](https://docs.docker.com/admin/convert-account/).

### [Our users create Docker Hub accounts through self-service. How do we know when the total number of users for the requested licenses has been met? Is it possible to add more members to the organization than the total number of licenses?](https://docs.docker.com/admin/faqs/organization-faqs/#our-users-create-docker-hub-accounts-through-self-service-how-do-we-know-when-the-total-number-of-users-for-the-requested-licenses-has-been-met-is-it-possible-to-add-more-members-to-the-organization-than-the-total-number-of-licenses)

There isn't any automatic notification when the total number of users for the requested licenses has been met. However, if the number of team members exceed the number of licenses, you will receive an error informing you to contact the administrator due to lack of seats. You can [add seats](https://docs.docker.com/subscription/core-subscription/add-seats/) if needed.

### [How can I merge organization accounts?](https://docs.docker.com/admin/faqs/organization-faqs/#how-can-i-merge-organization-accounts)

You can downgrade a secondary organization and transition your users and data to a primary organization. See [Merge organizations](https://docs.docker.com/admin/organization/orgs/#merge-organizations).

### [Do organization invitees take up seats?](https://docs.docker.com/admin/faqs/organization-faqs/#do-organization-invitees-take-up-seats)

Yes. A user invited to an organization will take up one of the provisioned seats, even if that user hasn’t accepted their invitation yet. Organization owners can manage the list of invitees through the **Invitees** tab on the organization settings page in Docker Hub, or in the **Members** page in Admin Console.

### [Do organization owners take a seat?](https://docs.docker.com/admin/faqs/organization-faqs/#do-organization-owners-take-a-seat)

Yes. Organization owners will take up a seat.

### [What is the difference between user, invitee, seat, and member?](https://docs.docker.com/admin/faqs/organization-faqs/#what-is-the-difference-between-user-invitee-seat-and-member)

User may refer to a Docker user with a Docker ID.

An invitee is a user that an administrator has invited to join an organization, but has not yet accepted their invitation.

Seats is the number of planned members within an organization.

Member may refer to a user that has received and accepted an invitation to join an organization. Member can also refer to a member of a team within an organization.

### [If there are two organizations and a user belongs to both organizations, do they take up two seats?](https://docs.docker.com/admin/faqs/organization-faqs/#if-there-are-two-organizations-and-a-user-belongs-to-both-organizations-do-they-take-up-two-seats)

Yes. In a scenario where a user belongs to two organizations, they take up one seat in each organization.

### [Is it possible to set permissions for repositories within an organization?](https://docs.docker.com/admin/faqs/organization-faqs/#is-it-possible-to-set-permissions-for-repositories-within-an-organization)

Yes. You can configure repository access on a per-team basis. For example, you can specify that all teams within an organization have **Read and Write** access to repositories A and B, whereas only specific teams have **Admin** access. Org owners have full administrative access to all repositories within the organization. See [Configure repository permissions for a team](https://docs.docker.com/admin/organization/manage-a-team/#configure-repository-permissions-for-a-team). Administrators can also assign members the editor role, which grants administrative permissions for repositories across the namespace of the organization. See [Roles and permissions](https://docs.docker.com/security/for-admins/roles-and-permissions/).

### [Does my organization need to use Docker's registry?](https://docs.docker.com/admin/faqs/organization-faqs/#does-my-organization-need-to-use-dockers-registry)

A registry is a hosted service containing repositories of images that responds to the Registry API. Docker Hub is Docker's primary registry, but you can use Docker with other container image registries. You can access the default registry by browsing to [Docker Hub](https://hub.docker.com/) or using the `docker search` command.
