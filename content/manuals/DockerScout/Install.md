+++
title = "Install"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/scout/install/](https://docs.docker.com/scout/install/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Scout

The Docker Scout CLI plugin comes pre-installed with Docker Desktop.

If you run Docker Engine without Docker Desktop, Docker Scout doesn't come pre-installed, but you can install it as a standalone binary.

## [Installation script](https://docs.docker.com/scout/install/#installation-script)

To install the latest version of the plugin, run the following commands:



```console
$ curl -fsSL https://raw.githubusercontent.com/docker/scout-cli/main/install.sh -o install-scout.sh
$ sh install-scout.sh
```

> **Note**
>
> 
>
> Always examine scripts downloaded from the internet before running them locally. Before installing, make yourself familiar with potential risks and limitations of the convenience script.

## [Manual installation](https://docs.docker.com/scout/install/#manual-installation)

{{< tabpane text=true persist=disabled >}}

{{% tab header="Linux" %}}

1. Download the latest release from the [releases page](https://github.com/docker/scout-cli/releases).

2. Create a subdirectory under `$HOME/.docker` called `scout`.

   

   ```console
   $ mkdir -p $HOME/.docker/scout
   ```

3. Extract the archive and move the `docker-scout` binary to the `$HOME/.docker/scout` directory.

4. Make the binary executable: `chmod +x $HOME/.docker/scout/docker-scout`.

5. Add the `scout` subdirectory to your `.docker/config.json` as a plugin directory:

   

   ```json
   {
     "cliPluginsExtraDirs": [
       "$HOME/.docker/scout"
     ]
   }
   ```

{{% /tab  %}}

{{% tab header="macOS" %}}

1. Download the latest release from the [releases page](https://github.com/docker/scout-cli/releases).

2. Create a subdirectory under `$HOME/.docker` called `scout`.

   

   ```console
   $ mkdir -p $HOME/.docker/scout
   ```

3. Extract the archive and move the `docker-scout` binary to the `$HOME/.docker/scout` directory.

4. Make the binary executable:

   

   ```console
   $ chmod +x $HOME/.docker/scout/docker-scout`
   ```

5. Authorize the binary to be executable on macOS:

   

   ```console
   xattr -d com.apple.quarantine $HOME/.docker/scout/docker-scout`.
   ```

6. Add the `scout` subdirectory to your `.docker/config.json` as a plugin directory:

   

   ```json
   {
     "cliPluginsExtraDirs": [
       "$HOME/.docker/scout"
     ]
   }
   ```

{{% /tab  %}}

{{% tab header="Windows" %}}

1. Download the latest release from the [releases page](https://github.com/docker/scout-cli/releases).

2. Create a subdirectory under `%USERPROFILE%/.docker` called `scout`.

   

   ```console
   % mkdir %USERPROFILE%\.docker\scout
   ```

3. Extract the archive and move the `docker-scout.exe` binary to the `%USERPROFILE%\.docker\scout` directory.

4. Add the `scout` subdirectory to your `.docker\config.json` as a plugin directory:

   

   ```json
   {
     "cliPluginsExtraDirs": [
       "C:\Users\MobyWhale\.docker\scout"
     ]
   }
   ```

{{% /tab  %}}

{{< /tabpane >}}

  

------

## [Container image](https://docs.docker.com/scout/install/#container-image)

The Docker Scout CLI plugin is also available as a [container image](https://hub.docker.com/r/docker/scout-cli). Use the `docker/scout-cli` to run `docker scout` commands without installing the CLI plugin on your host.



```console
$ docker run -it \
  -e DOCKER_SCOUT_HUB_USER=<your Docker Hub user name> \
  -e DOCKER_SCOUT_HUB_PASSWORD=<your Docker Hub PAT>  \
  docker/scout-cli <command>
```

## [GitHub Action](https://docs.docker.com/scout/install/#github-action)

The Docker Scout CLI plugin is also available as a [GitHub action](https://github.com/docker/scout-action). You can use it in your GitHub workflows to automatically analyze images and evaluate policy compliance with each push.

Docker Scout also integrates with many more CI/CD tools, such as Jenkins, GitLab, and Azure DevOps. Learn more about the [integrations](https://docs.docker.com/scout/integrations/) available for Docker Scout.
