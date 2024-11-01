+++
title = "登录"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/get-started/](https://docs.docker.com/desktop/get-started/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Sign in to Docker Desktop - 登录 Docker Desktop

Docker recommends that you authenticate using the **Sign in** option in the top-right corner of the Docker Dashboard.

​	Docker 推荐使用 Docker Dashboard 右上角的 **Sign in** 选项进行身份验证。

In large enterprises where admin access is restricted, administrators can [enforce sign-in]({{< ref "/manuals/Security/Foradmins/Enforcesign-in" >}}).

​	在大型企业中，管理员可以 [强制登录]({{< ref "/manuals/Security/Foradmins/Enforcesign-in" >}})。

> **Tip**
>
> 
>
> Explore [Docker's core subscriptions](https://www.docker.com/pricing/) to see what else Docker can offer you.
>
> ​	了解 [Docker 的核心订阅](https://www.docker.com/pricing/) 以查看 Docker 可以为您提供的其他服务。

## 登录的好处 Benefits of signing in

- You can access your Docker Hub repositories directly from Docker Desktop.
  - 您可以直接从 Docker Desktop 访问 Docker Hub 存储库。

- Authenticated users also get a higher pull rate limit compared to anonymous users. For example, if you are authenticated, you get 200 pulls per 6 hour period, compared to 100 pulls per 6 hour period per IP address for anonymous users. For more information, see [Download rate limit]({{< ref "/manuals/DockerHub/Usageandratelimits" >}}).
  - 已认证用户的拉取速率限制高于匿名用户。例如，已认证用户每 6 小时可以拉取 200 次，而匿名用户每 6 小时每 IP 限制为 100 次。详情参见 [下载速率限制]({{< ref "/manuals/DockerHub/Usageandratelimits" >}})。

- Improve your organization’s security posture for containerized development by taking advantage of [Hardened Desktop]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop" >}}).
  - 通过利用 [Hardened Desktop]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop" >}})，提高贵组织的容器化开发安全性。


> **Note**
>
> 
>
> Docker Desktop automatically signs you out after 90 days, or after 30 days of inactivity.
>
> ​	Docker Desktop 会在 90 天后或 30 天不活动后自动将您登出。

## 在 Linux 版 Docker Desktop 上登录 Signing in with Docker Desktop for Linux

Docker Desktop for Linux relies on [`pass`](https://www.passwordstore.org/) to store credentials in gpg2-encrypted files. Before signing in to Docker Desktop with your [Docker ID]({{< ref "/manuals/Dockeraccounts/Createanaccount" >}}), you must initialize `pass`. Docker Desktop displays a warning if you've not initialized `pass`.

​	Linux 版 Docker Desktop 依赖 [`pass`](https://www.passwordstore.org/) 将凭证存储在 gpg2 加密文件中。使用 [Docker ID]({{< ref "/manuals/Dockeraccounts/Createanaccount" >}}) 登录 Docker Desktop 前，您必须初始化 `pass`。如果尚未初始化 `pass`，Docker Desktop 会显示警告。

You can initialize pass by using a gpg key. To generate a gpg key, run:

​	您可以使用 gpg 密钥初始化 pass。生成 gpg 密钥，请运行：

```console
$ gpg --generate-key
```

The following is an example similar to what you see once you run the previous command:

​	运行上述命令后，您会看到类似的内容：

```console
...
GnuPG needs to construct a user ID to identify your key.
GnuPG 需要构建用户 ID 以标识您的密钥。

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

​	使用生成的公钥初始化 `pass`，运行以下命令：



```console
$ pass init <your_generated_gpg-id_public_key>
```

The following is an example similar to what you see once you run the previous command:

​	运行上述命令后，您会看到类似的内容：



```console
mkdir: created directory '/home/molly/.password-store/'
Password store initialized for <generated_gpg-id_public_key>
```

Once you initialize `pass`, you can sign in and pull your private images. When Docker CLI or Docker Desktop use credentials, a user prompt may pop up for the password you set during the gpg key generation.

​	初始化 `pass` 后，您可以登录并拉取您的私有镜像。当 Docker CLI 或 Docker Desktop 使用凭证时，可能会弹出用户提示要求输入您在 gpg 密钥生成时设置的密码。



```console
$ docker pull molly/privateimage
Using default tag: latest
latest: Pulling from molly/privateimage
3b9cc81c3203: Pull complete 
Digest: sha256:3c6b73ce467f04d4897d7a7439782721fd28ec9bf62ea2ad9e81a5fb7fb3ff96
Status: Downloaded newer image for molly/privateimage:latest
docker.io/molly/privateimage:latest
```

## What's next?

- [Explore Docker Desktop]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop" >}}) and its features.
  - [探索 Docker Desktop]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop" >}}) 及其功能。
- Change your Docker Desktop settings
  - 更改您的 Docker Desktop 设置。
- [Browse common FAQs]({{< ref "/manuals/DockerDesktop/FAQs/General" >}})
  - [浏览常见 FAQs]({{< ref "/manuals/DockerDesktop/FAQs/General" >}})。
