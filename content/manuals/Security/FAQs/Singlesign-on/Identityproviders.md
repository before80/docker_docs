+++
title = "Identity providers"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Identity providers

### [Is it possible to use more than one IdP with Docker SSO?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#is-it-possible-to-use-more-than-one-idp-with-docker-sso)

No. You can only configure Docker SSO to work with a single IdP. A domain can only be associated with a single IdP. Docker supports Entra ID (formerly Azure AD) and identity providers that support SAML 2.0.

### [Is it possible to change my identity provider after configuring SSO?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#is-it-possible-to-change-my-identity-provider-after-configuring-sso)

Yes. You must delete your existing IdP configuration in your Docker SSO connection and then [configure SSO using your new IdP](https://docs.docker.com/security/for-admins/single-sign-on/configure/configure-idp/). If you had already turned on enforcement, you should turn off enforcement before updating the provider SSO connection.

### [What information do I need from my identity provider to configure SSO?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#what-information-do-i-need-from-my-identity-provider-to-configure-sso)

To enable SSO in Docker, you need the following from your IdP:

- **SAML**: Entity ID, ACS URL, Single Logout URL and the public X.509 certificate
- **Entra ID (formerly Azure AD)**: Client ID, Client Secret, AD Domain.

### [What happens if my existing certificate expires?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#what-happens-if-my-existing-certificate-expires)

If your existing certificate has expired, you may need to contact your identity provider to retrieve a new X.509 certificate. Then, you need to update the certificate in the [SSO configuration settings](https://docs.docker.com/security/for-admins/single-sign-on/manage/#manage-sso-connections) in Docker Hub or Docker Admin Console.

### [What happens if my IdP goes down when SSO is enabled?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#what-happens-if-my-idp-goes-down-when-sso-is-enabled)

If SSO is enforced, then it is not possible to access Docker Hub when your IdP is down. You can still access Docker Hub images from the CLI using your Personal Access Token.

If SSO is enabled but not enforced, then users could fallback to authenticate with username/password and trigger a reset password flow (if necessary).

### [How do I handle accounts using Docker Hub as a secondary registry? Do I need a bot account?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#how-do-i-handle-accounts-using-docker-hub-as-a-secondary-registry-do-i-need-a-bot-account)

You can add a bot account to your IdP and create an access token for it to replace the other credentials.

### [Does a bot account need a seat to access an organization using SSO?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#does-a-bot-account-need-a-seat-to-access-an-organization-using-sso)

Yes, bot accounts need a seat, similar to a regular end user, having a non-aliased domain email enabled in the IdP and using a seat in Hub.

### [Does SAML SSO use Just-in-Time provisioning?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#does-saml-sso-use-just-in-time-provisioning)

The SSO implementation uses Just-in-Time (JIT) provisioning by default. You can optionally disable JIT in the Admin Console if you enable auto-provisioning using SCIM. See [Just-in-Time provisioning](https://docs.docker.com/security/for-admins/provisioning/just-in-time/).

### [Is IdP-initiated sign-in available?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#is-idp-initiated-sign-in-available)

Docker SSO doesn't support IdP-initiated sign-in, only Service Provider Initiated (SP-initiated) sign-in.

### [Is it possible to connect Docker Hub directly with a Microsoft Entra (formerly Azure AD) group?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#is-it-possible-to-connect-docker-hub-directly-with-a-microsoft-entra-formerly-azure-ad-group)

Yes, Entra ID (formerly Azure AD) is supported with SSO for Docker Business, both through a direct integration and through SAML.

### [My SSO connection with Entra ID isn't working and I receive an error that the application is misconfigured. How can I troubleshoot this?](https://docs.docker.com/security/faqs/single-sign-on/idp-faqs/#my-sso-connection-with-entra-id-isnt-working-and-i-receive-an-error-that-the-application-is-misconfigured-how-can-i-troubleshoot-this)

Confirm that you've configured the necessary API permissions in Entra ID (formerly Azure AD) for your SSO connection. You need to grant admin consent within your Entra ID (formerly Azure AD) tenant. See [Entra ID (formerly Azure AD) documentation](https://learn.microsoft.com/en-us/azure/active-directory/manage-apps/grant-admin-consent?pivots=portal#grant-admin-consent-in-app-registrations).
