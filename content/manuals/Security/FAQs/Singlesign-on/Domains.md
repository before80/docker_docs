+++
title = "Domains"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/security/faqs/single-sign-on/domain-faqs/](https://docs.docker.com/security/faqs/single-sign-on/domain-faqs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Domains

### [Can I add sub-domains?](https://docs.docker.com/security/faqs/single-sign-on/domain-faqs/#can-i-add-sub-domains)

Yes, you can add sub-domains to your SSO connection, however all email addresses should also be on that domain. Verify that your DNS provider supports multiple TXT records for the same domain.

### [Can the DNS provider configure it once for one-time verification and remove it later or will it be needed permanently?](https://docs.docker.com/security/faqs/single-sign-on/domain-faqs/#can-the-dns-provider-configure-it-once-for-one-time-verification-and-remove-it-later-or-will-it-be-needed-permanently)

You can do it one time to add the domain to a connection. If your organization ever changes IdPs and has to set up SSO again, your DNS provider will need to verify again.

### [Is adding domain required to configure SSO? What domains should I be adding? And how do I add it?](https://docs.docker.com/security/faqs/single-sign-on/domain-faqs/#is-adding-domain-required-to-configure-sso-what-domains-should-i-be-adding-and-how-do-i-add-it)

Adding and verifying a domain is required to enable and enforce SSO. See [Step one: Add and verify your domain](https://docs.docker.com/security/for-admins/single-sign-on/configure/#step-one-add-and-verify-your-domain) to learn how to specify the email domains that are allowed to authenticate through your server. This should include all email domains users will use to access Docker. Public domains, for example `gmail.com` or `outlook.com`, are not permitted. Also, the email domain should be set as the primary email.

### [Is IdP-initiated authentication supported?](https://docs.docker.com/security/faqs/single-sign-on/domain-faqs/#is-idp-initiated-authentication-supported)

IdP-initiated authentication isn't supported by Docker SSO. Users must initiate sign-in through Docker Desktop or Hub.
