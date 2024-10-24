+++
title = "Sign in"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/get-started/](https://docs.docker.com/desktop/get-started/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Sign in to Docker Desktop

Docker recommends that you authenticate using the **Sign in** option in the top-right corner of the Docker Dashboard.

In large enterprises where admin access is restricted, administrators can [enforce sign-in](https://docs.docker.com/security/for-admins/enforce-sign-in/).

> **Tip**
>
> 
>
> Explore [Docker's core subscriptions](https://www.docker.com/pricing/) to see what else Docker can offer you.

## [Benefits of signing in](https://docs.docker.com/desktop/get-started/#benefits-of-signing-in)

- You can access your Docker Hub repositories directly from Docker Desktop.
- Authenticated users also get a higher pull rate limit compared to anonymous users. For example, if you are authenticated, you get 200 pulls per 6 hour period, compared to 100 pulls per 6 hour period per IP address for anonymous users. For more information, see [Download rate limit](https://docs.docker.com/docker-hub/download-rate-limit/).
- Improve your organization’s security posture for containerized development by taking advantage of [Hardened Desktop](https://docs.docker.com/security/for-admins/hardened-desktop/).

> **Note**
>
> 
>
> Docker Desktop automatically signs you out after 90 days, or after 30 days of inactivity.

## [Signing in with Docker Desktop for Linux](https://docs.docker.com/desktop/get-started/#signing-in-with-docker-desktop-for-linux)

Docker Desktop for Linux relies on [`pass`](https://www.passwordstore.org/) to store credentials in gpg2-encrypted files. Before signing in to Docker Desktop with your [Docker ID](https://docs.docker.com/accounts/create-account/), you must initialize `pass`. Docker Desktop displays a warning if you've not initialized `pass`.

You can initialize pass by using a gpg key. To generate a gpg key, run:



```console
$ gpg --generate-key
```

The following is an example similar to what you see once you run the previous command:



```console
...
GnuPG needs to construct a user ID to identify your key.

Real name: Molly
Email address: molly@example.com
You selected this USER-ID:
   "Molly <molly@example.com>"

Change (N)ame, (E)mail, or (O)kay/(Q)uit? O
...
pubrsa3072 2022-03-31 [SC] [expires: 2024-03-30]
 <generated gpg-id public key>
uid          Molly <molly@example.com>
subrsa3072  2022-03-31 [E] [expires: 2024-03-30]
```

To initialize `pass`, run the following command using the public key generated from the previous command:



```console
$ pass init <your_generated_gpg-id_public_key>
```

The following is an example similar to what you see once you run the previous command:



```console
mkdir: created directory '/home/molly/.password-store/'
Password store initialized for <generated_gpg-id_public_key>
```

Once you initialize `pass`, you can sign in and pull your private images. When Docker CLI or Docker Desktop use credentials, a user prompt may pop up for the password you set during the gpg key generation.



```console
$ docker pull molly/privateimage
Using default tag: latest
latest: Pulling from molly/privateimage
3b9cc81c3203: Pull complete 
Digest: sha256:3c6b73ce467f04d4897d7a7439782721fd28ec9bf62ea2ad9e81a5fb7fb3ff96
Status: Downloaded newer image for molly/privateimage:latest
docker.io/molly/privateimage:latest
```

## [What's next?](https://docs.docker.com/desktop/get-started/#whats-next)

- [Explore Docker Desktop](https://docs.docker.com/desktop/use-desktop/) and its features.
- Change your Docker Desktop settings
- [Browse common FAQs](https://docs.docker.com/desktop/faqs/general/)
