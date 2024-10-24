+++
title = "Access tokens"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/security/for-developers/access-tokens/](https://docs.docker.com/security/for-developers/access-tokens/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Create and manage access tokens

You can create a personal access token (PAT) to use as an alternative to your password for Docker CLI authentication.

Compared to passwords, PATs provide the following advantages:

- You can investigate when the PAT was last used and then disable or delete it if you find any suspicious activity.
- When using an access token, you can't perform any admin activity on the account, including changing the password. It protects your account if your computer is compromised.

Access tokens are also valuable for building integrations, as you can issue multiple tokens, one for each integration, and revoke them at any time.

## Create an access token

> **Important**
>
> 
>
> Treat access tokens like your password and keep them secret. Store your tokens securely in a credential manager for example.

1. Sign in to your [Docker account](https://app.docker.com/login).
2. Select your avatar in the top-right corner and from the drop-down menu select **Account settings**.
3. In the **Security** section, select **Personal access tokens**.
4. Select **Generate new token**.
5. Add a description for your token. Use something that indicates the use case or purpose of the token.
6. Set the access permissions. The access permissions are scopes that set restrictions in your repositories. For example, for Read & Write permissions, an automation pipeline can build an image and then push it to a repository. However, it can't delete the repository.
7. Select **Generate** and then copy the token that appears on the screen and save it. You won't be able to retrieve the token once you close this prompt.

## Use an access token

You can use an access token in place of your password when you sign in using Docker CLI.

Sign in from your Docker CLI client with the following command, replacing `YOUR_USERNAME` with your Docker ID:



```console
$ docker login --username YOUR_USERNAME
```

When prompted for a password, enter your personal access token instead of a password.

> **Note**
>
> 
>
> If you have [two-factor authentication (2FA)]({{< ref "/manuals/Security/Fordevelopers/Two-factorauthentication" >}}) enabled, you must use a personal access token when logging in from the Docker CLI. 2FA is an optional, but more secure method of authentication.

## Modify existing tokens

You can rename, activate, deactivate, or delete a token as needed. You can manage your tokens in your account settings.

1. Sign in to your [Docker account](https://app.docker.com/login).
2. Select your avatar in the top-right corner and from the drop-down menu select **Account settings**.
3. In the **Security** section, select **Personal access tokens**. This page shows an overview of all your tokens, and lists if the token was generated manually or if it was [auto-generated](https://docs.docker.com/security/for-developers/access-tokens/#auto-generated-tokens). You can also view the number of tokens that are activated and deactivated in the toolbar.
4. Select the actions menu on the far right of a token row, then select **Deactivate**, **Edit**, or **Delete** to modify the token.
5. After modifying the token, select **Save token**.

## Auto-generated tokens

When you sign in to your Docker account with Docker Desktop, Docker Desktop generates an authentication token on your behalf. When you interact with Docker Hub using the Docker CLI, the CLI uses this token for authentication. The token scope has Read, Write, and Delete access. If your Docker Desktop session expires, the token is automatically removed locally.

You can have up to 5 auto-generated tokens associated with your account. These are deleted and created automatically based on usage and creation dates. You can also delete your auto-generated tokens as needed. See [Modify existing tokens](https://docs.docker.com/security/for-developers/access-tokens/#modify-existing-tokens).
