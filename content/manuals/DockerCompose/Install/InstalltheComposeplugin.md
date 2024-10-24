+++
title = "Install the Compose plugin"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/compose/install/linux/](https://docs.docker.com/compose/install/linux/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install the Compose plugin

On this page you can find instructions on how to install the Compose plugin on Linux from the command line.

To install the Compose plugin on Linux, you can either:

- [Set up Docker's repository on your Linux system](https://docs.docker.com/compose/install/linux/#install-using-the-repository).
- [Install Compose manually](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually).

> **Note**
>
> 
>
> These instructions assume you already have Docker Engine and Docker CLI installed and now want to install the Compose plugin.
> For Compose standalone, see [Install Compose Standalone]({{< ref "/manuals/DockerCompose/Install/InstallComposestandalone" >}}).

## Install using the repository

1. Set up the repository. Find distro-specific instructions in:

   [Ubuntu](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) | [CentOS](https://docs.docker.com/engine/install/centos/#set-up-the-repository) | [Debian](https://docs.docker.com/engine/install/debian/#install-using-the-repository) | [Raspberry Pi OS](https://docs.docker.com/engine/install/raspberry-pi-os/#install-using-the-repository) | [Fedora](https://docs.docker.com/engine/install/fedora/#set-up-the-repository) | [RHEL](https://docs.docker.com/engine/install/rhel/#set-up-the-repository) | [SLES](https://docs.docker.com/engine/install/sles/#set-up-the-repository).

2. Update the package index, and install the latest version of Docker Compose:

   - For Ubuntu and Debian, run:

     

     ```console
     $ sudo apt-get update
     $ sudo apt-get install docker-compose-plugin
     ```

   - For RPM-based distros, run:

     

     ```console
     $ sudo yum update
     $ sudo yum install docker-compose-plugin
     ```

3. Verify that Docker Compose is installed correctly by checking the version.

   

   ```console
   $ docker compose version
   ```

   Expected output:

   

   ```text
   Docker Compose version vN.N.N
   ```

   Where `vN.N.N` is placeholder text standing in for the latest version.

### Update Compose

To update the Compose plugin, run the following commands:

- For Ubuntu and Debian, run:

  

  ```console
  $ sudo apt-get update
  $ sudo apt-get install docker-compose-plugin
  ```

- For RPM-based distros, run:

  

  ```console
  $ sudo yum update
  $ sudo yum install docker-compose-plugin
  ```

## Install the plugin manually

> **Note**
>
> 
>
> This option requires you to manage upgrades manually. We recommend setting up Docker's repository for easier maintenance.

1. To download and install the Compose CLI plugin, run:

   

   ```console
   $ DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
   $ mkdir -p $DOCKER_CONFIG/cli-plugins
   $ curl -SL https://github.com/docker/compose/releases/download/v2.29.6/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
   ```

   This command downloads the latest release of Docker Compose (from the Compose releases repository) and installs Compose for the active user under `$HOME` directory.

   To install:

   - Docker Compose for *all users* on your system, replace `~/.docker/cli-plugins` with `/usr/local/lib/docker/cli-plugins`.
   - A different version of Compose, substitute `v2.29.6` with the version of Compose you want to use.

   - For a different architecture, substitute `x86_64` with the [architecture you want](https://github.com/docker/compose/releases).

2. Apply executable permissions to the binary:

   

   ```console
   $ chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
   ```

   or, if you chose to install Compose for all users:

   

   ```console
   $ sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
   ```

3. Test the installation.

   

   ```console
   $ docker compose version
   ```

   Expected output:

   

   ```text
   Docker Compose version v2.29.6
   ```
