+++
title = "Activity logs"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/admin/organization/activity-logs/](https://docs.docker.com/admin/organization/activity-logs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Activity logs

Activity logs display a chronological list of activities that occur at organization and repository levels. It provides a report to owners on all their member activities.

With activity logs, owners can view and track:

- What changes were made
- The date when a change was made
- Who initiated the change

For example, activity logs display activities such as the date when a repository was created or deleted, the member who created the repository, the name of the repository, and when there was a change to the privacy settings.

Owners can also see the activity logs for their repository if the repository is part of the organization subscribed to a Docker Business or Team plan.

> **Note**
>
> 
>
> Activity logs requires a [Docker Team or Business subscription](https://docs.docker.com/subscription/).

## [Manage activity logs](https://docs.docker.com/admin/organization/activity-logs/#manage-activity-logs)

{{< tabpane text=true persist=disabled >}}

{{% tab header="Docker Hub" %}}

### [View the activity logs](https://docs.docker.com/admin/organization/activity-logs/#view-the-activity-logs)

To view the activity logs:

1. Sign in to [Docker Hub](https://hub.docker.com/).
2. Select **Organizations**, your organization, and then **Activity**.

> **Note**
>
> Docker retains the activity data for a period of three months.

### [Customize the activity logs](https://docs.docker.com/admin/organization/activity-logs/#customize-the-activity-logs)

By default, all activities that occur are displayed on the **Activity** tab. Use the calendar option to select a date range and customize your results. After you have selected a date range, the activity logs of all the activities that occurred during that period are displayed.

> **Note**
>
> Activities created by the Docker Support team as part of resolving customer issues appear in the activity logs as **dockersupport**.

Select the **All Activities** drop-down to view activities that are specific to an organization, repository, or billing. In Docker Hub, if you select the **Activities** tab from the **Repository** view, you can only filter repository-level activities.

After choosing **Organization**, **Repository**, or **Billing**, you can further refine the results using the **All Actions** drop-down.

{{% /tab  %}}

{{% tab header=" Admin Console" %}}

**Early Access**

The Docker Admin Console is an [early access](https://docs.docker.com/release-lifecycle#early-access-ea) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

### [View the activity logs](https://docs.docker.com/admin/organization/activity-logs/#view-the-activity-logs)

To view the activity logs:

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your organization in the left navigation drop-down menu, and then select **Activity logs**.

> **Note**
>
> Docker retains the activity data for a period of three months.

### [Customize the activity logs](https://docs.docker.com/admin/organization/activity-logs/#customize-the-activity-logs)

By default, all activities that occur are displayed on the **Activity** tab. Use the calendar option to select a date range and customize your results. After you have selected a date range, the activity logs of all the activities that occurred during that period are displayed.

> **Note**
>
> Activities created by the Docker Support team as part of resolving customer issues appear in the activity logs as **dockersupport**.

Select the **All Activities** drop-down to view activities that are specific to an organization, repository, or billing. In Docker Hub, if you select the **Activities** tab from the **Repository** view, you can only filter repository-level activities.

After choosing **Organization**, **Repository**, or **Billing**, you can further refine the results using the **All Actions** drop-down.

{{% /tab  %}}

{{< /tabpane >}}



## [Event definitions](https://docs.docker.com/admin/organization/activity-logs/#event-definitions)

Refer to the following section for a list of events and their descriptions:

### [Organization events](https://docs.docker.com/admin/organization/activity-logs/#organization-events)

| Event                                       | Description                                                  |
| :------------------------------------------ | :----------------------------------------------------------- |
| Team Created                                | Activities related to the creation of a team                 |
| Team Updated                                | Activities related to the modification of a team             |
| Team Deleted                                | Activities related to the deletion of a team                 |
| Team Member Added                           | Details of the member added to your team                     |
| Team Member Removed                         | Details of the member removed from your team                 |
| Team Member Invited                         | Details of the member invited to your team                   |
| Organization Member Added                   | Details of the member added to your organization             |
| Organization Member Removed                 | Details about the member removed from your organization      |
| Member Role Changed                         | Details about the role changed for a member in your organization |
| Organization Created                        | Activities related to the creation of a new organization     |
| Organization Settings Updated               | Details related to the organization setting that was updated |
| Registry Access Management enabled          | Activities related to enabling Registry Access Management    |
| Registry Access Management disabled         | Activities related to disabling Registry Access Management   |
| Registry Access Management registry added   | Activities related to the addition of a registry             |
| Registry Access Management registry removed | Activities related to the removal of a registry              |
| Registry Access Management registry updated | Details related to the registry that was updated             |
| Single Sign-On domain added                 | Details of the single sign-on domain added to your organization |
| Single Sign-On domain removed               | Details of the single sign-on domain removed from your organization |
| Single Sign-On domain verified              | Details of the single sign-on domain verified for your organization |
| Access token created                        | Access token created in organization                         |
| Access token updated                        | Access token updated in organization                         |
| Access token deleted                        | Access token deleted in organization                         |

### [Repository events](https://docs.docker.com/admin/organization/activity-logs/#repository-events)

| Event              | Description                                                  |
| :----------------- | :----------------------------------------------------------- |
| Repository Created | Activities related to the creation of a new repository       |
| Repository Deleted | Activities related to the deletion of a repository           |
| Repository Updated | Activities related to updating the description, full description, or status of a repository |
| Privacy Changed    | Details related to the privacy policies that were updated    |
| Tag Pushed         | Activities related to the tags pushed                        |
| Tag Deleted        | Activities related to the tags deleted                       |
| Categories Updated | Activities related to setting or updating categories of a repository |

### [Billing events](https://docs.docker.com/admin/organization/activity-logs/#billing-events)

| Event                                 | Description                                                  |
| :------------------------------------ | :----------------------------------------------------------- |
| Plan Upgraded                         | Occurs when your organization’s billing plan is upgraded to a higher tier plan. |
| Plan Downgraded                       | Occurs when your organization’s billing plan is downgraded to a lower tier plan. |
| Seat Added                            | Occurs when a seat is added to your organization’s billing plan. |
| Seat Removed                          | Occurs when a seat is removed from your organization’s billing plan. |
| Billing Cycle Changed                 | Occurs when there is a change in the recurring interval that your organization is charged. |
| Plan Downgrade Canceled               | Occurs when a scheduled plan downgrade for your organization is canceled. |
| Seat Removal Canceled                 | Occurs when a scheduled seat removal for an organization’s billing plan is canceled. |
| Plan Upgrade Requested                | Occurs when a user in your organization requests a plan upgrade. |
| Plan Downgrade Requested              | Occurs when a user in your organization requests a plan downgrade. |
| Seat Addition Requested               | Occurs when a user in your organization requests an increase in the number of seats. |
| Seat Removal Requested                | Occurs when a user in your organization requests a decrease in the number of seats. |
| Billing Cycle Change Requested        | Occurs when a user in your organization requests a change in the billing cycle. |
| Plan Downgrade Cancellation Requested | Occurs when a user in your organization requests a cancellation of a scheduled plan downgrade. |
| Seat Removal Cancellation Requested   | Occurs when a user in your organization requests a cancellation of a scheduled seat removal. |
