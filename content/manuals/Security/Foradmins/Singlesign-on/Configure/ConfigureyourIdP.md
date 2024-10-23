+++
title = "Configure your IdP"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Configure your IdP

The steps to set up your SSO configuration are:

1. [Add and verify the domain or domains](https://docs.docker.com/security/for-admins/single-sign-on/configure#step-one-add-and-verify-your-domain) that your members use to sign in to Docker.
2. [Create your SSO connection](https://docs.docker.com/security/for-admins/single-sign-on/configure#step-two-create-an-sso-connection-in-docker) in Docker.
3. [Configure your IdP](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/#step-three-configure-your-idp-to-work-with-docker) to work with Docker.
4. [Complete your SSO connection](https://docs.docker.com/security/for-admins/single-sign-on/connect/) in Docker.

This page walks through step 3 for common IdPs.

## [Prerequisites](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/#prerequisites)

Make sure you have completed the following before you begin:

- Your domain is verified
- You have created your SSO connection in Docker
- You have copied the necessary fields from Docker to paste in your IdP:
  - SAML: **Entity ID**, **ACS URL**
  - Azure AD (OIDC): **Redirect URL**

## [SSO attributes](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/#sso-attributes)

When a user signs in using SSO, Docker obtains the following attributes from the IdP:

- **Email address** - unique identifier of the user
- **Full name** - name of the user
- **Groups (optional)** - list of groups to which the user belongs
- **Docker Org (optional)** - the organization to which the user belongs
- **Docker Team (optional)** - the team within an organization that a user has been added to
- **Docker Role (optional)** - the role for the user that grants their permissions in an organization

If you use SAML for your SSO connection, Docker obtains these attributes from the SAML assertion message. Your IdP may use different naming for SAML attributes than those in the previous list. The following table lists the possible SAML attributes that can be present in order for your SSO connection to work.

> **Important**
>
> 
>
> SSO uses Just-in-Time (JIT) provisioning by default. If you [enable SCIM](https://docs.docker.com/security/for-admins/provisioning/scim/), JIT values still overwrite the attribute values set by SCIM provisioning whenever users log in. To avoid conflicts, make sure your JIT values match your SCIM values. For example, to make sure that the full name of a user displays in your organization, you would set a `name` attribute in your SAML attributes and ensure the value includes their first name and last name. The exact method for setting these values (for example, constructing it with `user.firstName + " " + user.lastName`) varies depending on your IdP.

> **Tip**
>
> 
>
> Optional Just-in-Time (JIT) provisioning is available when you use the Admin Console and enable SCIM. With this feature, you can avoid conflicts between SCIM and JIT by disabling JIT provisioning in your SSO connection. See [SSO authentication with JIT provisioning disabled](https://docs.docker.com/security/for-admins/provisioning/just-in-time/#sso-authentication-with-jit-provisioning-disabled).

You can also configure attributes to override default values, such as default team or organization. See [role mapping](https://docs.docker.com/security/for-admins/provisioning/scim/#set-up-role-mapping).

| SSO attribute          | SAML assertion message attributes                            |
| ---------------------- | ------------------------------------------------------------ |
| Email address          | `"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"`, `"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"`, `"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"`, `email` |
| Full name              | `"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"`, `name`, `"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"`, `"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"` |
| Groups (optional)      | `"http://schemas.xmlsoap.org/claims/Group"`, `"http://schemas.microsoft.com/ws/2008/06/identity/claims/groups"`, `Groups`, `groups` |
| Docker Org (optional)  | `dockerOrg`                                                  |
| Docker Team (optional) | `dockerTeam`                                                 |
| Docker Role (optional) | `dockerRole`                                                 |

> **Important**
>
> 
>
> If none of the email address attributes listed in the previous table are found, SSO returns an error. Also, if the `Full name` attribute isn't set, then the name will be displayed as the value of the `Email address`.

## [Step three: Configure your IdP to work with Docker](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/#step-three-configure-your-idp-to-work-with-docker)

The user interface for your IdP may differ slightly from the following steps. You can refer to the documentation for your IdP to verify.

Okta Entra ID SAML 2.0 Azure Connect (OIDC)

------

See [More resources](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/#more-resources) for a video overview on how to set up SSO with SAML in Okta.

1. Go to the Okta admin portal.

2. Go to **Applications > Applications > Create App Integration**.

3. Select **SAML 2.0**, then select **Next**.

4. Enter App Name "Docker Hub" and optionally upload a logo for the app, then select **Next**.

5. To configure SAML, enter the following into Okta:

   - ACS URL: Single Sign On URL
   - Entity ID: Audience URI (SP Entity ID)
   - Name ID format: `EmailAddress`
   - Application username: `Email`
   - Update application on: `Create or Update`
   - Attribute Statements: `add`. You can define your attribute statement like the following:

   | Attribute name | Name format | Value                                    |
   | :------------- | :---------- | :--------------------------------------- |
   | name           | Unspecified | username.firstName + " " + user.lastName |

6. Select **Next**.

7. Select **I'm an Okta customer adding an internal app**.

8. Select **Finish**.

9. After you create the app, go to your app and select **View SAML setup instructions**.

10. Here you can find the **SAML Sign-in URL** and the **x509 Certificate**. Open the certificate file in a text editor and paste the contents of the file in the **x509 Certificate** field in Docker Hub or Admin Console. Then, copy the value of the **SAML Sign-in URL** and paste it into the corresponding field in Docker Hub or Admin Console.

------

## [More resources](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/#more-resources)

The following videos demonstrate how to configure your IdP with your Docker SSO connection.

- [Video: SSO connection with Okta](https://youtu.be/c56YECO4YP4?feature=shared&t=633)
- [Video: SSO connection with Azure Connect (OIDC)](https://youtu.be/bGquA8qR9jU?feature=shared&t=630)
- [Video: SSO connection with Entra ID (Azure) SAML](https://youtu.be/bGquA8qR9jU?feature=shared&t=1246)

## [What's next?](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/#whats-next)

[Complete your connection](https://docs.docker.com/security/for-admins/single-sign-on/connect/) in the Docker console, then test your connection.
