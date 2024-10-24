+++
title = "Manage organization members"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/admin/organization/members/](https://docs.docker.com/admin/organization/members/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Manage organization members

Learn how to manage members for your organization in Docker Hub and the Docker Admin Console.

{{< tabpane text=true persist=disabled >}}

{{% tab header="Docker Hub" %}}

## Invite members

Owners can invite new members to an organization via Docker ID, email address, or via a CSV file containing email addresses. If an invitee does not have a Docker account, they must create an account and verify their email address before they can accept the invitation to join the organization. When inviting members, their pending invitation occupies a seat.

### Invite members via Docker ID or email address

Use the following steps to invite members to your organization via Docker ID or email address. To invite a large amount of members to your organization via CSV file, see the next section.

1. Sign in to [Docker Hub](https://hub.docker.com/).

2. Select **Organizations**, your organization, and then **Members**.

3. Select **Invite members**.

4. Select **Emails or usernames**.

5. Follow the on-screen instructions to invite members. Invite a maximum of 1000 members and separate multiple entries by comma, semicolon, or space.

   > **Note**
   >
   > When you invite members, you assign them a role. See [Roles and permissions]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}) for details about the access permissions for each role.

   Pending invitations appear in the table. The invitees receive an email with a link to Docker Hub where they can accept or decline the invitation.

### Invite members via CSV file

To invite multiple members to an organization via a CSV file containing email addresses:

1. Sign in to [Docker Hub](https://hub.docker.com/).

2. Select **Organizations**, your organization, and then **Members**.

3. Select **Invite members**.

4. Select **CSV upload**.

5. Select **Download the template CSV file** to optionally download an example CSV file. The following is an example of the contents of a valid CSV file.

   

   ```text
   email
   docker.user-0@example.com
   docker.user-1@example.com
   ```

   CSV file requirements:

   - The file must contain a header row with at least one heading named `email`. Additional columns are allowed and are ignored in the import.
   - The file must contain a maximum of 1000 email addresses (rows). To invite more than 1000 users, create multiple CSV files and perform all steps in this task for each file.

6. Create a new CSV file or export a CSV file from another application.

   - To export a CSV file from another application, see the application’s documentation.
   - To create a new CSV file, open a new file in a text editor, type `email` on the first line, type the user email addresses one per line on the following lines, and then save the file with a .csv extension.

7. Select **Browse files** and then select your CSV file, or drag and drop the CSV file into the **Select a CSV file to upload** box. You can only select one CSV file at a time.

   > **Note**
   >
   > If the amount of email addresses in your CSV file exceeds the number of available seats in your organization, you cannot continue to invite members. To invite members, you can purchase more seats, or remove some email addresses from the CSV file and re-select the new file. To purchase more seats, see [Add seats to your subscription](https://docs.docker.com/subscription/add-seats/) or [Contact sales](https://www.docker.com/pricing/contact-sales/).

8. After the CSV file has been uploaded, select **Review**.

   Valid email addresses and any email addresses that have issues appear. Email addresses may have the following issues:

   - **Invalid email**: The email address is not a valid address. The email address will be ignored if you send invites. You can correct the email address in the CSV file and re-import the file.
   - **Already invited**: The user has already been sent an invite email and another invite email will not be sent.
   - **Member**: The user is already a member of your organization and an invite email will not be sent.
   - **Duplicate**: The CSV file has multiple occurrences of the same email address. The user will be sent only one invite email.

9. Follow the on-screen instructions to invite members.

   > **Note**
   >
   > When you invite members, you assign them a role. See [Roles and permissions]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}) for details about the access permissions for each role.

Pending invitations appear in the table. The invitees receive an email with a link to Docker Hub where they can accept or decline the invitation.

## Resend invitations

To resend an invitation if the invite is pending or declined:

1. Sign in to [Docker Hub](https://hub.docker.com/).
2. Select **Organizations**, your organization, and then **Members**.
3. In the table, locate the invitee, select the **Action** icon, and then select **Resend invitation**.
4. Select **Invite** to confirm.

## Remove a member or invitee

To remove a member from an organization:

1. Sign in to [Docker Hub](https://hub.docker.com/).
2. Select **Organizations**, your organization, and then **Members**.
3. In the table, select the **Action** icon, and then select **Remove member** or **Remove invitee**.
4. Follow the on-screen instructions to remove the member or invitee.

## Update a member role

Organization owners can manage [roles]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}) within an organization. If an organization is part of a company, the company owner can also manage that organization's roles. If you have SSO enabled, you can use [SCIM for role mapping]({{< ref "/manuals/Security/Foradmins/Provisioning/SCIM" >}}).

> **Note**
>
> If you're the only owner of an organization, you need to assign a new owner before you can edit your role.

To update a member role:

1. Sign in to [Docker Hub](https://hub.docker.com/).
2. Select **Organizations**, your organization, and then **Members**.
3. Find the username of the member whose role you want to edit. In the table, select the **Actions** icon.
4. Select **Edit role**.
5. Select the role you want to assign, then select **Save**.

## Export members

Owners can export a CSV file containing all members. The CSV file for an organization contains the following fields:

- **Name**: The user's name.
- **Username**: The user's Docker ID.
- **Email**: The user's email address.
- **Type**: The type of user. For example, **Invitee** for users who have not accepted the organization's invite, or **User** for users who are members of the organization.
- **Role**: The user's role in the organization. For example, **Member** or **Owner**.
- **Teams**: The teams where the user is a member. A team is not listed for invitees.
- **Date Joined**: The time and date when the user was invited to the organization.

To export a CSV file of the members:

1. Sign in to [Docker Hub](https://hub.docker.com/).
2. Select **Organizations**, your organization, and then **Members**.
3. Select **Export members**.

{{% /tab  %}}

{{% tab header=" Admin Console" %}}

**Early Access**

The Docker Admin Console is an [early access]({{< ref "/manuals/Releaselifecycle#early-access-ea" >}}) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

## Invite members

Owners can invite new members to an organization via Docker ID, email address, or via a CSV file containing email addresses. If an invitee does not have a Docker account, they must create an account and verify their email address before they can accept the invitation to join the organization. When inviting members, their pending invitation occupies a seat.

### Invite members via Docker ID or email address

Use the following steps to invite members to your organization via Docker ID or email address. To invite a large amount of members to your organization via CSV file, see the next section.

1. Sign in to the [Admin Console](https://admin.docker.com/).

2. Select your organization in the left navigation drop-down menu, and then select **Members**.

3. Select **Invite**.

4. Select **Emails or usernames**.

5. Follow the on-screen instructions to invite members. Invite a maximum of 1000 members and separate multiple entries by comma, semicolon, or space.

   > **Note**
   >
   > When you invite members, you assign them a role. See [Roles and permissions]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}) for details about the access permissions for each role.

   Pending invitations appear in the table. The invitees receive an email with a link to Docker Hub where they can accept or decline the invitation.

### Invite members via CSV file

To invite multiple members to an organization via a CSV file containing email addresses:

1. Sign in to the [Admin Console](https://admin.docker.com/).

2. Select your organization in the left navigation drop-down menu, and then select **Members**.

3. Select **Invite**.

4. Select **CSV upload**.

5. Select **Download the template CSV file** to optionally download an example CSV file. The following is an example of the contents of a valid CSV file.

   

   ```text
   email
   docker.user-0@example.com
   docker.user-1@example.com
   ```

   CSV file requirements:

   - The file must contain a header row with at least one heading named `email`. Additional columns are allowed and are ignored in the import.
   - The file must contain a maximum of 1000 email addresses (rows). To invite more than 1000 users, create multiple CSV files and perform all steps in this task for each file.

6. Create a new CSV file or export a CSV file from another application.

   - To export a CSV file from another application, see the application’s documentation.
   - To create a new CSV file, open a new file in a text editor, type `email` on the first line, type the user email addresses one per line on the following lines, and then save the file with a .csv extension.

7. Select **Browse files** and then select your CSV file, or drag and drop the CSV file into the **Select a CSV file to upload** box. You can only select one CSV file at a time.

   > **Note**
   >
   > If the amount of email addresses in your CSV file exceeds the number of available seats in your organization, you cannot continue to invite members. To invite members, you can purchase more seats, or remove some email addresses from the CSV file and re-select the new file. To purchase more seats, see [Add seats to your subscription](https://docs.docker.com/subscription/add-seats/) or [Contact sales](https://www.docker.com/pricing/contact-sales/).

8. After the CSV file has been uploaded, select **Review**.

   Valid email addresses and any email addresses that have issues appear. Email addresses may have the following issues:

   - **Invalid email**: The email address is not a valid address. The email address will be ignored if you send invites. You can correct the email address in the CSV file and re-import the file.
   - **Already invited**: The user has already been sent an invite email and another invite email will not be sent.
   - **Member**: The user is already a member of your organization and an invite email will not be sent.
   - **Duplicate**: The CSV file has multiple occurrences of the same email address. The user will be sent only one invite email.

9. Follow the on-screen instructions to invite members.

   > **Note**
   >
   > When you invite members, you assign them a role. See [Roles and permissions]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}) for details about the access permissions for each role.

Pending invitations appear in the table. The invitees receive an email with a link to Docker Hub where they can accept or decline the invitation.

## Resend invitations

To resend an invitation if the invite is pending or declined:

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your organization in the left navigation drop-down menu, and then select **Members**.
3. In the table, locate the invitee, select the **Action** icon, and then select **Resend invitation**.
4. Select **Invite** to confirm.

## Remove a member or invitee

To remove a member from an organization:

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your organization in the left navigation drop-down menu, and then select **Members**.
3. In the table, select the **Action** icon, and then select **Remove member** or **Remove invitee**.
4. Follow the on-screen instructions to remove the member or invitee.

## Update a member role

Organization owners can manage [roles]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}) within an organization. If an organization is part of a company, the company owner can also manage that organization's roles. If you have SSO enabled, you can use [SCIM for role mapping]({{< ref "/manuals/Security/Foradmins/Provisioning/SCIM" >}}).

> **Note**
>
> If you're the only owner of an organization, you need to assign a new owner before you can edit your role.

To update a member role:

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your organization in the left navigation drop-down menu, and then select **Members**.
3. Find the username of the member whose role you want to edit. In the table, select the **Actions** icon.
4. Select **Edit role**.
5. Select the role you want to assign, then select **Save**.

## Export members

Owners can export a CSV file containing all members. The CSV file for an organization contains the following fields:

- **Name**: The user's name.
- **Username**: The user's Docker ID.
- **Email**: The user's email address.
- **Type**: The type of user. For example, **Invitee** for users who have not accepted the organization's invite, or **User** for users who are members of the organization.
- **Role**: The user's role in the organization. For example, **Member** or **Owner**.
- **Teams**: The teams where the user is a member. A team is not listed for invitees.
- **Date Joined**: The time and date when the user was invited to the organization.

To export a CSV file of the members:

1. Sign in to the [Admin Console](https://admin.docker.com/).
2. Select your organization in the left navigation drop-down menu, and then select **Members**.
3. Select the **Action** icon and then select **Export users as CSV**.

{{% /tab  %}}

{{< /tabpane >}}



------

## Manage members on a team

Use Docker Hub to add a member to a team or remove a member from a team.

### Add a member to a team

Organization owners can add a member to one or more teams within an organization.

{{< tabpane text=true persist=disabled >}}

{{% tab header="Docker Hub" %}}

To add a member to a team:

1. Sign in to [Docker Hub](https://hub.docker.com/).

2. Select **Organizations**, your organization, and then **Members**.

3. Select the **Action** icon, and then select **Add to team**.

   > **Note**
   >
   > 
   >
   > You can also navigate to **Organizations** > **Your Organization** > **Teams** > **Your Team Name** and select **Add Member**. Select a member from the drop-down list to add them to the team or search by Docker ID or email.

4. Select the team and then select **Add**.

   > **Note**
   >
   > 
   >
   > An invitee must first accept the invitation to join the organization before being added to the team.

{{% /tab  %}}

{{% tab header=" Admin Console" %}}

**Early Access**

The Docker Admin Console is an [early access]({{< ref "/manuals/Releaselifecycle#early-access-ea" >}}) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

To add a member to a team:

1. In the Admin Console, select your organization.

2. Select the team name.

3. Select **Add member**. You can add the member by searching for their email address or username.

   > **Note**
   >
   > 
   >
   > An invitee must first accept the invitation to join the organization before being added to the team.

{{% /tab  %}}

{{< /tabpane >}}



------

### Remove a member from a team

Organization owners can remove a member from a team in Docker Hub or Admin Console. Removing the member from the team will revoke their access to the permitted resources.

{{< tabpane text=true persist=disabled >}}

{{% tab header="Docker Hub" %}}

To add a member to a team:

To remove a member from a specific team:

1. Sign in to [Docker Hub](https://hub.docker.com/).
2. Select **Organizations**, your organization, **Teams**, and then the team.
3. Select the **X** next to the user’s name to remove them from the team.
4. When prompted, select **Remove** to confirm.

{{% /tab  %}}

{{% tab header=" Admin Console" %}}

**Early Access**

The Docker Admin Console is an [early access]({{< ref "/manuals/Releaselifecycle#early-access-ea" >}}) product.

It's available to all company owners and organization owners. You can still manage organizations in Docker Hub, but the Admin Console includes company-level management and enhanced features for organization management.

To remove a member from a specific team:

1. In the Admin Console, select your organization.
2. Select the team name.
3. Select the **X** next to the user's name to remove them from the team.
4. When prompted, select **Remove** to confirm.

{{% /tab  %}}

{{< /tabpane >}}

