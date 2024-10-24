+++
title = "SCIM"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/security/for-admins/provisioning/scim/](https://docs.docker.com/security/for-admins/provisioning/scim/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# SCIM provisioning

This section is for administrators who want to enable System for Cross-domain Identity Management (SCIM) 2.0 for their business. It is available for Docker Business customers.

SCIM provides automated user provisioning and de-provisioning for your Docker organization or company through your identity provider (IdP). Once you enable SCIM in Docker and your IdP, any user assigned to the Docker application in the IdP is automatically provisioned in Docker and added to the organization or company.

Similarly, if a user gets unassigned from the Docker application in the IdP, this removes the user from the organization or company in Docker. SCIM also synchronizes changes made to a user's attributes in the IdP, for example the user’s first name and last name.

The following lists the supported provisioning features:

- Creating new users
- Push user profile updates
- Remove users
- Deactivate users
- Re-activate users
- Group mapping

## Supported attributes

The following table lists the supported attributes. Note that your attribute mappings must match for SSO to prevent duplicating your members.

| Attribute       | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| userName        | User's primary email address. This is the unique identifier of the user. |
| name.givenName  | User’s first name                                            |
| name.familyName | User’s surname                                               |
| active          | Indicates if a user is enabled or disabled. Can be set to false to de-provision the user. |

For additional details about supported attributes and SCIM, see [Docker Hub API SCIM reference](https://docs.docker.com/reference/api/hub/latest/#tag/scim).

> **Important**
>
> 
>
> SSO uses Just-in-Time (JIT) provisioning by default. If you [enable SCIM](https://docs.docker.com/security/for-admins/provisioning/scim/#set-up-scim), JIT values still overwrite the attribute values set by SCIM provisioning whenever users log in. To avoid conflicts, make sure your JIT values match your SCIM values. For more information, see [SSO attributes](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/#sso-attributes).

> **Tip**
>
> 
>
> Optional Just-in-Time (JIT) provisioning is available when you use the Admin Console and enable SCIM. With this feature, you can avoid conflicts between SCIM and JIT by disabling JIT provisioning in your SSO connection. See [SSO authentication with JIT provisioning disabled](https://docs.docker.com/security/for-admins/provisioning/just-in-time/#sso-authentication-with-jit-provisioning-disabled).

## Enable SCIM in Docker

You must make sure you have [configured SSO]({{< ref "/manuals/Security/Foradmins/Singlesign-on/Configure" >}}) before you enable SCIM. Enforcing SSO isn't required.

Docker Hub Admin Console

------

1. Sign in to [Docker Hub](https://hub.docker.com/).
2. Navigate to the SSO settings page for your organization or company.
   - Organization: Select **Organizations**, your organization, **Settings**, and then **Security**.
   - Company: Select **Organizations**, your company, and then **Settings**.
3. In the SSO connections table, select the **Actions** icon and **Setup SCIM**.
4. Copy the **SCIM Base URL** and **API Token** and paste the values into your IdP.

------

## Enable SCIM in your IdP

The user interface for your IdP may differ slightly from the following steps. You can refer to the documentation for your IdP to verify.

Okta Entra ID SAML 2.0

------

### Enable SCIM

1. Go to the Okta admin portal.
2. Go to the app you created when you configured your SSO connection.
3. On the app page, go to the **General** tab and select **Edit App Settings**.
4. Enable SCIM provisioning, then select **Save**.
5. Now you can access the **Provisioning** tab. Navigate to this tab, then select **Edit SCIM Connection**.
6. To configure SCIM in Okta, set up your connection like the following:
   - SCIM Base URL: SCIM connector base URL (copied from Docker Hub)
   - Unique identifier field for users: `email`
   - Supported provisioning actions: **Push New Users** and **Push Profile Updates**
   - Authentication Mode: HTTP Header
   - SCIM Bearer Token: HTTP Header Authorization Bearer Token (copied from Docker Hub)
7. Select **Test Connector Configuration**.
8. Review the test results.
9. Select **Save**.

### Enable synchronization

1. Go to **Provisioning > To App > Edit**.
2. Enable **Create Users**, **Update User Attributes**, and **Deactivate Users**.
3. Select **Save**.
4. Remove unnecessary mappings. The necessary mappings are:
   - Username
   - Given name
   - Family name
   - Email

------

See the documentation for your IdP for additional details:

- [Okta](https://help.okta.com/en-us/Content/Topics/Apps/Apps_App_Integration_Wizard_SCIM.htm)
- [Entra ID (formerly Azure AD)](https://learn.microsoft.com/en-us/azure/active-directory/app-provisioning/user-provisioning)

## Set up role mapping

You can assign [roles]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}) to members in your organization in the IdP. To set up a role, you can use optional user-level attributes for the person you want to assign a role. In addition to roles, you can set an organization or team to override the default provisioning values set by the SSO connection.

> **Note**
>
> 
>
> These mappings are supported for both SCIM and JIT provisioning. With JIT provisioning, role mapping only applies when a user is initially provisioned to the organization.

The following table lists the supported optional user-level attributes.

| Attribute    | Possible values                                              | Considerations                                               |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `dockerRole` | `member`, `editor`, or `owner`. For a list of permissions for each role, see [Roles and permissions]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}). | If you don't assign a role in the IdP, the value of the `dockerRole` attribute defaults to `member`. When you set the attribute, this overrides the default value. |
| `dockerOrg`  | `organizationName`. For example, an organization named "moby" would be `moby`. | Setting this attribute overrides the default organization configured by the SSO connection. Also, this won't add the user to the default team. If this attribute isn't set, the user is provisioned to the default organization and the default team. If set and `dockerTeam` is also set, this provisions the user to the team within that organization. |
| `dockerTeam` | `teamName`. For example, a team named "developers" would be `developers`. | Setting this attribute provisions the user to the default organization and to the specified team, instead of the SSO connection's default team. This also creates the team if it doesn't exist. You can still use group mapping to provision users to teams in multiple organizations. See [Group mapping]({{< ref "/manuals/Security/Foradmins/Provisioning/Groupmapping" >}}). |

After you set the role in the IdP, you need to sync to push the changes to Docker.

The external namespace to use to set up these attributes is `urn:ietf:params:scim:schemas:extension:docker:2.0:User`.

Okta Entra ID SAML 2.0

------

### Set up

1. Setup [SSO]({{< ref "/manuals/Security/Foradmins/Singlesign-on/Configure" >}}) and SCIM first.
2. In the Okta admin portal, go to **Directory > Profile Editor** and select **User (Default)**.
3. Select **Add Attribute** and configure the values for the role, org, or team you want to add. Exact naming isn't required.
4. Return to the **Profile Editor** and select your application.
5. Select **Add Attribute** and enter the required values. The **External Name** and **External Namespace** must be exact. The external name values for org/team/role mapping are `dockerOrg`, `dockerTeam`, and `dockerRole` respectively, as listed in the previous table. The external namespace is the same for all of them: `urn:ietf:params:scim:schemas:extension:docker:2.0:User`.
6. After creating the attributes, go to the top and select **Mappings > Okta User to YOUR APP**.
7. Go to the newly created attributes and map the variable names selected above to the external names, then select **Save Mappings**. If you’re using JIT provisioning, continue to the following step.
8. Go to **Applications > YOUR APP > General > SAML Settings > Edit > Step 2** and configure the mapping from the user attribute to the docker variables.

### Assign roles by user

1. Go to **Directory > People > YOUR USER > Profile**, then select **Edit** on **Attributes**.
2. Update the attributes to the desired values.

### Assign roles by group

1. Go to **Directory > People > YOUR GROUP > Applications > YOUR APPLICATION**, then select the **Edit** icon.
2. Update the attributes to the desired values.

If a user doesn't already have attributes set up, users who are added to the group will inherit these attributes upon provsioning.

------

See the documentation for your IdP for additional details:

- [Okta](https://help.okta.com/en-us/Content/Topics/users-groups-profiles/usgp-add-custom-user-attributes.htm)
- [Entra ID (formerly Azure AD)](https://learn.microsoft.com/en-us/azure/active-directory/app-provisioning/customize-application-attributes#provisioning-a-custom-extension-attribute-to-a-scim-compliant-application)

## Disable SCIM

If SCIM is disabled, any user provisioned through SCIM will remain in the organization. Future changes for your users will not sync from your IdP. User de-provisioning is only possible when manually removing the user from the organization.

Docker Hub Admin Console

------

1. Sign in to [Docker Hub](https://hub.docker.com/).
2. Navigate to the SSO settings page for your organization or company.
   - Organization: Select **Organizations**, your organization, **Settings**, and then **Security**.
   - Company: Select **Organizations**, your company, and then **Settings**.
3. In the SSO connections table, select the **Actions** icon.
4. Select **Disable SCIM**.

------

## More resources

The following videos demonstrate how to configure SCIM for your IdP.

- [Video: Configure SCIM with Okta](https://youtu.be/c56YECO4YP4?feature=shared&t=1314)
- [Video: Attribute mapping with Okta](https://youtu.be/c56YECO4YP4?feature=shared&t=1998)
- [Video: Configure SCIM with Entra ID (Azure)](https://youtu.be/bGquA8qR9jU?feature=shared&t=1668)
- [Video: Attribute and group mapping with Entra ID (Azure)](https://youtu.be/bGquA8qR9jU?feature=shared&t=2039)
