+++
title = "Group mapping"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/security/for-admins/provisioning/group-mapping/](https://docs.docker.com/security/for-admins/provisioning/group-mapping/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Group mapping

With directory group-to-team provisioning from your IdP, user updates will automatically sync with your Docker organizations and teams. You can use group mapping once you have configured [single sign-on (SSO)]({{< ref "/manuals/Security/Foradmins/Singlesign-on" >}}).

> **Tip**
>
> 
>
> Group mapping is ideal for adding a user to multiple organizations or multiple teams within one organization. If you don't need to set up multi-organization or multi-team assignment, you can use [user-level attributes](https://docs.docker.com/security/for-admins/provisioning/scim/#set-up-role-mapping).

## How group mapping works

IdPs share with Docker the main attributes of every authorized user through SSO, such as email address, name, surname, and groups. Just-in-Time (JIT) Provisioning uses these attributes to create or update the user’s Docker profile and their associations with organizations and teams on Docker Hub.

Docker uses the email address of the user to identify them on the platform. Every Docker account must have a unique email address at all times.

## Use group mapping

To correctly assign your users to Docker teams, you must create groups in your IdP following the naming pattern `organization:team`. For example, if you want to manage provisioning for the team "developers", and your organization name is "moby", you must create a group in your IdP with the name `moby:developers`.

Once you enable group mappings in your connection, users assigned to that group in your IdP will automatically be added to the team "developers" in Docker.

You can use this format to add a user to multiple organizations. For example, if you want to add a user to the "backend" team in the "moby" organization as well as the "desktop" team in the "whale" organization, the format would be: `moby:backend` and `whale:desktop`.

> **Tip**
>
> Use the same names for the Docker teams as your group names in the IdP to prevent further configuration. When you sync groups, this creates a group if it doesn’t already exist.

The following lists the supported group mapping attributes:

| Attribute        | Description                                                  |
| :--------------- | :----------------------------------------------------------- |
| id               | Unique ID of the group in UUID format. This attribute is read-only. |
| displayName      | Name of the group following the group mapping format: `organization:team`. |
| members          | A list of users that are members of this group.              |
| members(x).value | Unique ID of the user that is a member of this group. Members are referenced by ID. |

The general steps to use group mapping are:

1. In your IdP, create groups with the `organization:team` format.
2. Add users to the group.
3. Add the Docker application that you created in your IdP to the group.
4. Add attributes in the IdP.
5. Push groups to Docker.

The exact configuration may vary depending on your IdP. You can use [group mapping with SSO](https://docs.docker.com/security/for-admins/provisioning/group-mapping/#use-group-mapping-with-sso), or with SSO and [SCIM enabled](https://docs.docker.com/security/for-admins/provisioning/group-mapping/#use-group-mapping-with-scim).

### Use group mapping with SSO

The following steps describe how to set up and use group mapping with SSO connections that use the SAML authentication method. Note that group mapping with SSO isn't supported with the Azure AD (OIDC) authentication method. Additionally, SCIM isn't required for these configurations.

Okta Entra ID

------

The user interface for your IdP may differ slightly from the following steps. You can refer to the [Okta documentation](https://help.okta.com/oie/en-us/content/topics/apps/define-group-attribute-statements.htm) to verify.

To set up group mapping:

1. Sign in to the Okta Console to go to your application.

2. Go to the **SAML Settings** for your application.

3. In the

    

   Group Attribute Statements (optional)

    

   section, configure like the following:

   - **Name**: `groups`
   - **Name format**: `Unspecified`
   - **Filter**: `Starts with` + `organization:` where `organization` is the name of your organization The filter option will filter out the groups that aren't affiliated with your Docker organization.

4. Create your groups by navigating to **Directory > Groups**.

5. Add your groups using the format `organization:team` that matches the names of your organization(s) and team(s) in Docker.

6. Assign users to the group(s) that you create.

The next time you sync your groups with Docker, your users will map to the Docker groups you defined.

------

### Use group mapping with SCIM

The following steps describe how to set up and use group mapping with SCIM. Before you begin, make sure you [set up SCIM](https://docs.docker.com/security/for-admins/provisioning/scim/#enable-scim) first.

Okta Entra ID

------

The user interface for your IdP may differ slightly from the following steps. You can refer to the [Okta documentation](https://help.okta.com/en-us/Content/Topics/users-groups-profiles/usgp-enable-group-push.htm) to verify.

To set up your groups:

1. Sign in to the Okta Console to go to your application.
2. Select **Applications > Provisioning > Integration**.
3. Select **Edit** to enable groups on your connection, then select **Push groups**.
4. Select **Save**. Saving this configuration will add the **Push Groups** tab to your application.
5. Create your groups by navigating to **Directory > Groups**.
6. Add your groups using the format `organization:team` that matches the names of your organization(s) and team(s) in Docker.
7. Assign users to the group(s) that you create.
8. Return to **Applications > Provisioning > Integration**, then select the **Push Groups** tab to open the view where you can control and manage how groups are provisioned.
9. Select **Push Groups > Find groups by rule**.
10. Configure the groups by rule like the following:
    - Enter a rule name, for example `Sync groups with Docker Hub`
    - Match group by name, for example starts with `docker:` or contains `:` for multi-organization
    - If you enable **Immediately push groups by rule**, sync will happen as soon as there's a change to the group or group assignments. Enable this if you don't want to manually push groups.

Find your new rule under **By rule** in the **Pushed Groups** column. The groups that match that rule are listed in the groups table on the right-hand side.

To push the groups from this table:

1. Select **Group in Okta**.
2. Select the **Push Status** drop-down.
3. Select **Push Now**.

------

Once complete, a user who signs in to Docker through SSO is automatically added to the organizations and teams mapped in the IdP.

> **Tip**
>
> 
>
> [Enable SCIM]({{< ref "/manuals/Security/Foradmins/Provisioning/SCIM" >}}) to take advantage of automatic user provisioning and de-provisioning. If you don't enable SCIM users are only automatically provisioned. You have to de-provision them manually.

## More resources

The following videos demonstrate how to use group mapping with your IdP with SCIM enabled.

- [Video: Group mapping with Okta](https://youtu.be/c56YECO4YP4?feature=shared&t=3023)
- [Video: Attribute and group mapping with Entra ID (Azure)](https://youtu.be/bGquA8qR9jU?feature=shared&t=2039)
