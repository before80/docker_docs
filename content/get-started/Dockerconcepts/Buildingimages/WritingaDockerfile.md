+++
title = "Writing a Dockerfile"
date = 2024-10-23T14:54:35+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/](https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Writing a Dockerfile

<iframe id="youtube-player-Jx8zoIhiP4c" data-video-id="Jx8zoIhiP4c" class="youtube-video aspect-video h-fit w-full py-2" frameborder="0" allowfullscreen="" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" title="Docker concepts - Writing a Dockerfile" width="100%" height="100%" src="https://www.youtube.com/embed/Jx8zoIhiP4c?rel=0&amp;iv_load_policy=3&amp;enablejsapi=1&amp;origin=https%3A%2F%2Fdocs.docker.com&amp;widgetid=1" data-gtm-yt-inspected-21="true" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border-width: 0px; border-style: solid; border-color: initial; display: block; vertical-align: middle; aspect-ratio: 16 / 9; height: fit-content; width: 634.672px; padding-top: 0.5rem; padding-bottom: 0.5rem; color: rgb(0, 0, 0); font-family: &quot;Roboto Flex&quot;, system-ui, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Oxygen, Ubuntu, Cantarell, &quot;Open Sans&quot;, &quot;Helvetica Neue&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>

## Explanation

A Dockerfile is a text-based document that's used to create a container image. It provides instructions to the image builder on the commands to run, files to copy, startup command, and more.

As an example, the following Dockerfile would produce a ready-to-run Python application:



```dockerfile
FROM python:3.12
WORKDIR /usr/local/app

# Install the application dependencies
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy in the source code
COPY src ./src
EXPOSE 5000

# Setup an app user so the container doesn't run as the root user
RUN useradd app
USER app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

### Common instructions

Some of the most common instructions in a `Dockerfile` include:

- `FROM <image>` - this specifies the base image that the build will extend.
- `WORKDIR <path>` - this instruction specifies the "working directory" or the path in the image where files will be copied and commands will be executed.
- `COPY <host-path> <image-path>` - this instruction tells the builder to copy files from the host and put them into the container image.
- `RUN <command>` - this instruction tells the builder to run the specified command.
- `ENV <name> <value>` - this instruction sets an environment variable that a running container will use.
- `EXPOSE <port-number>` - this instruction sets configuration on the image that indicates a port the image would like to expose.
- `USER <user-or-uid>` - this instruction sets the default user for all subsequent instructions.
- `CMD ["<command>", "<arg1>"]` - this instruction sets the default command a container using this image will run.

To read through all of the instructions or go into greater detail, check out the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/).

## Try it out

Just as you saw with the previous example, a Dockerfile typically follows these steps:

1. Determine your base image
2. Install application dependencies
3. Copy in any relevant source code and/or binaries
4. Configure the final image

In this quick hands-on guide, you'll write a Dockerfile that builds a simple Node.js application. If you're not familiar with JavaScript-based applications, don't worry. It isn't necessary for following along with this guide.

### Set up

[Download this ZIP file](https://github.com/docker/getting-started-todo-app/raw/build-image-from-scratch/app.zip) and extract the contents into a directory on your machine.

### Creating the Dockerfile

Now that you have the project, you’re ready to create the `Dockerfile`.

1. [Download and install](https://www.docker.com/products/docker-desktop/) Docker Desktop.

2. Create a file named `Dockerfile` in the same folder as the file `package.json`.

   > **Dockerfile file extensions**
   >
   > It's important to note that the `Dockerfile` has *no* file extension. Some editors will automatically add an extension to the file (or complain it doesn't have one).

3. In the `Dockerfile`, define your base image by adding the following line:

   

   ```dockerfile
   FROM node:20-alpine
   ```

4. Now, define the working directory by using the `WORKDIR` instruction. This will specify where future commands will run and the directory files will be copied inside the container image.

   

   ```dockerfile
   WORKDIR /app
   ```

5. Copy all of the files from your project on your machine into the container image by using the `COPY` instruction:

   

   ```dockerfile
   COPY . .
   ```

6. Install the app's dependencies by using the `yarn` CLI and package manager. To do so, run a command using the `RUN` instruction:

   

   ```dockerfile
   RUN yarn install --production
   ```

7. Finally, specify the default command to run by using the `CMD` instruction:

   

   ```dockerfile
   CMD ["node", "./src/index.js"]
   ```

   And with that, you should have the following Dockerfile:

   

   ```dockerfile
   FROM node:20-alpine
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   CMD ["node", "./src/index.js"]
   ```

> **This Dockerfile isn't production-ready yet**
>
> It's important to note that this Dockerfile is *not* following all of the best practices yet (by design). It will build the app, but the builds won't be as fast, or the images as secure, as they could be.
>
> Keep reading to learn more about how to make the image maximize the build cache, run as a non-root user, and multi-stage builds.

> **Containerize new projects quickly with `docker init`**
>
> The `docker init` command will analyze your project and quickly create a Dockerfile, a `compose.yaml`, and a `.dockerignore`, helping you get up and going. Since you're learning about Dockerfiles specifically here, you won't use it now. But, [learn more about it here](https://docs.docker.com/engine/reference/commandline/init/).

## Additional resources

To learn more about writing a Dockerfile, visit the following resources:

- [Dockerfile reference]({{< ref "/reference/Dockerfilereference" >}})
- [Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Base images]({{< ref "/manuals/DockerBuild/Building/Baseimages" >}})
- [Getting started with Docker Init]({{< ref "/reference/CLIreference/docker/dockerinit" >}})

## Next steps

Now that you have created a Dockerfile and learned the basics, it's time to learn about building, tagging, and pushing the images.

[Build, tag and publish the Image]({{< ref "/get-started/Dockerconcepts/Buildingimages/Buildtagandpublishanimage" >}})
