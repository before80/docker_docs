+++
title = "Two-factor authentication"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/security/for-developers/2fa/](https://docs.docker.com/security/for-developers/2fa/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Enable two-factor authentication for your Docker account

Two-factor authentication adds an extra layer of security to your Docker account by requiring a unique security code when you sign in to your account. The security code is required in addition to your password.

When you enable two-factor authentication, you are also provided with a recovery code. Each recovery code is unique and specific to your account. You can use this code to recover your account in case you lose access to your authenticator app. See [Recover your Docker account](https://docs.docker.com/security/for-developers/2fa/recover-hub-account/).

## [Prerequisites](https://docs.docker.com/security/for-developers/2fa/#prerequisites)

You need a mobile phone with a time-based one-time password (TOTP) authenticator application installed. Common examples include Google Authenticator or Yubico Authenticator with a registered YubiKey.

## [Enable two-factor authentication](https://docs.docker.com/security/for-developers/2fa/#enable-two-factor-authentication)

1. Sign in to your [Docker account](https://app.docker.com/login).
2. Select your avatar and then from the drop-down menu, select **Account settings**.
3. Navigate to the **Security** section, then select **Two-factor authentication**.
4. Enter your account password, then select **Confirm**.
5. Save your recovery code and store it somewhere safe. You can use your recovery code to recover your account in the event you lose access to your authenticator app.
6. Use a TOTP mobile app to scan the QR code or enter the text code.
7. Once you've linked your authenticator app, enter the six-digit code in the text-field.
8. Select **Enable 2FA**.

Two-factor authentication is now enabled. The next time you sign in to your Docker account, you will need to enter a security code.
