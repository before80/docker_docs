+++
title = "Access repositories"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/docker-hub/repos/access/](https://docs.docker.com/docker-hub/repos/access/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Access repositories

Within your repository, you can give others access to push and pull to your repository, and you can assign permissions. You can also view your repository tags and the associated images.

## Collaborators and their role

A collaborator is someone you want to give access to a private repository. Once designated, they can `push` and `pull` to your repositories. They're not allowed to perform any administrative tasks such as deleting the repository or changing its status from private to public.

Only personal account repositories can use collaborators. You can add unlimited collaborators to public repositories, and [Docker Pro](https://docs.docker.com/subscription/core-subscription/details/#docker-pro) accounts can add up to 1 collaborator on private repositories. Organization repositories can't use collaborators. Organization owners can control repository access with [member roles]({{< ref "/manuals/Security/Foradmins/Rolesandpermissions" >}}) and [teams]({{< ref "/manuals/Administration/Organizationadministration/Createandmanageateam" >}}).

You can choose collaborators and manage their access to a private repository from that repository's **Settings** page.

> **Note**
>
> 
>
> A collaborator can't add other collaborators. Only the owner of the repository has administrative access.

You can also assign more granular collaborator rights ("Read", "Write", or "Admin") on Docker Hub by using organizations and teams. For more information see the [organizations documentation](https://docs.docker.com/admin/organization/orgs/#create-an-organization).

## View repository tags

You can view the available tags and the size of the associated image. Go to the **Repositories** view and select a repository to see its tags. To view individual tags, select the **Tags** tab.

To delete a tag, select the corresponding checkbox and select **Delete** from the **Action** drop-down list.

> **Note**
>
> 
>
> Only a user with administrative access (owner or team member with Admin permission) over the repository can delete tags.

You can select a tag's digest to access more details.

Image sizes are the cumulative space taken up by the image and all its parent images. This is also the disk space used by the contents of the `.tar` file created when you `docker save` an image.

An image is stale if there has been no push or pull activity for more than one month. A multi-architecture image is stale if all single-architecture images part of its manifest are stale.

## Search for repositories

You can search the [Docker Hub](https://hub.docker.com/) registry through its search interface or by using the command line interface. You can search by image name, username, or description:



```console
$ docker search centos

NAME                                 DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
centos                               The official build of CentOS.                   1034      [OK]
ansible/centos7-ansible              Ansible on Centos7                              43                   [OK]
tutum/centos                         Centos image with SSH access. For the root...   13                   [OK]
...
```

In the previous example, you can see two example results, `centos` and `ansible/centos7-ansible`.

The second result shows that it comes from the public repository of a user, named `ansible/`, while the first result, `centos`, doesn't explicitly list a repository which means that it comes from the top-level namespace for [Docker Official Images]({{< ref "/manuals/Trustedcontent/DockerOfficialImages" >}}). The `/` character separates a user's repository from the image name.

Once you've found the image you want, you can download it with `docker pull <imagename>`:



```console
$ docker pull centos

latest: Pulling from centos
6941bfcbbfca: Pull complete
41459f052977: Pull complete
fd44297e2ddb: Already exists
centos:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
Digest: sha256:d601d3b928eb2954653c59e65862aabb31edefa868bd5148a41fa45004c12288
Status: Downloaded newer image for centos:latest
```

You now have an image from which you can run containers.

## Star repositories

Stars are a way to show that you like a repository. They're also an easy way of bookmarking your favorites.
