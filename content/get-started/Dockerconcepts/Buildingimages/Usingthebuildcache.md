+++
title = "Using the build cache"
date = 2024-10-23T14:54:35+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/get-started/docker-concepts/building-images/using-the-build-cache/](https://docs.docker.com/get-started/docker-concepts/building-images/using-the-build-cache/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Using the build cache

<iframe id="youtube-player-Ri6jMknjprY" data-video-id="Ri6jMknjprY" class="youtube-video aspect-video h-fit w-full py-2" frameborder="0" allowfullscreen="" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" title="Docker concepts - Using the build cache" width="100%" height="100%" src="https://www.youtube.com/embed/Ri6jMknjprY?rel=0&amp;iv_load_policy=3&amp;enablejsapi=1&amp;origin=https%3A%2F%2Fdocs.docker.com&amp;widgetid=1" data-gtm-yt-inspected-26="true" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border-width: 0px; border-style: solid; border-color: initial; display: block; vertical-align: middle; aspect-ratio: 16 / 9; height: fit-content; width: 634.672px; padding-top: 0.5rem; padding-bottom: 0.5rem; color: rgb(0, 0, 0); font-family: &quot;Roboto Flex&quot;, system-ui, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Oxygen, Ubuntu, Cantarell, &quot;Open Sans&quot;, &quot;Helvetica Neue&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>

## [Explanation](https://docs.docker.com/get-started/docker-concepts/building-images/using-the-build-cache/#explanation)

Consider the following Dockerfile that you created for the [getting-started](https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/) app.



```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "./src/index.js"]
```

When you run the `docker build` command to create a new image, Docker executes each instruction in your Dockerfile, creating a layer for each command and in the order specified. For each instruction, Docker checks whether it can reuse the instruction from a previous build. If it finds that you've already executed a similar instruction before, Docker doesn't need to redo it. Instead, it’ll use the cached result. This way, your build process becomes faster and more efficient, saving you valuable time and resources.

Using the build cache effectively lets you achieve faster builds by reusing results from previous builds and skipping unnecessary work. In order to maximize cache usage and avoid resource-intensive and time-consuming rebuilds, it's important to understand how cache invalidation works. Here are a few examples of situations that can cause cache to be invalidated:

- Any changes to the command of a `RUN` instruction invalidates that layer. Docker detects the change and invalidates the build cache if there's any modification to a `RUN` command in your Dockerfile.
- Any changes to files copied into the image with the `COPY` or `ADD` instructions. Docker keeps an eye on any alterations to files within your project directory. Whether it's a change in content or properties like permissions, Docker considers these modifications as triggers to invalidate the cache.
- Once one layer is invalidated, all following layers are also invalidated. If any previous layer, including the base image or intermediary layers, has been invalidated due to changes, Docker ensures that subsequent layers relying on it are also invalidated. This keeps the build process synchronized and prevents inconsistencies.

When you're writing or editing a Dockerfile, keep an eye out for unnecessary cache misses to ensure that builds run as fast and efficiently as possible.

## [Try it out](https://docs.docker.com/get-started/docker-concepts/building-images/using-the-build-cache/#try-it-out)

In this hands-on guide, you will learn how to use the Docker build cache effectively for a Node.js application.

### [Build the application](https://docs.docker.com/get-started/docker-concepts/building-images/using-the-build-cache/#build-the-application)

1. [Download and install](https://www.docker.com/products/docker-desktop/) Docker Desktop.

2. Open a terminal and [clone this sample application](https://github.com/dockersamples/todo-list-app).

   

   ```console
   $ git clone https://github.com/dockersamples/todo-list-app
   ```

3. Navigate into the `todo-list-app` directory:

   

   ```console
   $ cd todo-list-app
   ```

   Inside this directory, you'll find a file named `Dockerfile` with the following content:

   

   ```dockerfile
   FROM node:20-alpine
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   EXPOSE 3000
   CMD ["node", "./src/index.js"]
   ```

4. Execute the following command to build the Docker image:

   

   ```console
   $ docker build .
   ```

   Here’s the result of the build process:

   

   ```console
   [+] Building 20.0s (10/10) FINISHED
   ```

   The first line indicates that the entire build process took *20.0 seconds*. The first build may take some time as it installs dependencies.

5. Rebuild without making changes.

   Now, re-run the `docker build` command without making any change in the source code or Dockerfile as shown:

   

   ```console
   $ docker build .
   ```

   Subsequent builds after the initial are faster due to the caching mechanism, as long as the commands and context remain unchanged. Docker caches the intermediate layers generated during the build process. When you rebuild the image without making any changes to the Dockerfile or the source code, Docker can reuse the cached layers, significantly speeding up the build process.

   

   ```console
   [+] Building 1.0s (9/9) FINISHED                                                                            docker:desktop-linux
    => [internal] load build definition from Dockerfile                                                                        0.0s
    => => transferring dockerfile: 187B                                                                                        0.0s
    ...
    => [internal] load build context                                                                                           0.0s
    => => transferring context: 8.16kB                                                                                         0.0s
    => CACHED [2/4] WORKDIR /app                                                                                               0.0s
    => CACHED [3/4] COPY . .                                                                                                   0.0s
    => CACHED [4/4] RUN yarn install --production                                                                              0.0s
    => exporting to image                                                                                                      0.0s
    => => exporting layers                                                                                                     0.0s
    => => exporting manifest
   ```

   The subsequent build was completed in just 1.0 second by leveraging the cached layers. No need to repeat time-consuming steps like installing dependencies.

   | Steps | Description                                        | Time Taken(1st Run) | Time Taken (2nd Run) |
   | ----- | -------------------------------------------------- | ------------------- | -------------------- |
   | 1     | Load build definition from Dockerfile              | 0.0 seconds         | 0.0 seconds          |
   | 2     | Load metadata for docker.io/library/node:20-alpine | 2.7 seconds         | 0.9 seconds          |
   | 3     | Load .dockerignore                                 | 0.0 seconds         | 0.0 seconds          |
   | 4     | Load build context(Context size: 4.60MB)           | 0.1 seconds         | 0.0 seconds          |
   | 5     | Set the working directory (WORKDIR)                | 0.1 seconds         | 0.0 seconds          |
   | 6     | Copy the local code into the container             | 0.0 seconds         | 0.0 seconds          |
   | 7     | Run yarn install --production                      | 10.0 seconds        | 0.0 seconds          |
   | 8     | Exporting layers                                   | 2.2 seconds         | 0.0 seconds          |
   | 9     | Exporting the final image                          | 3.0 seconds         | 0.0 seconds          |

   Going back to the `docker image history` output, you see that each command in the Dockerfile becomes a new layer in the image. You might remember that when you made a change to the image, the `yarn` dependencies had to be reinstalled. Is there a way to fix this? It doesn't make much sense to reinstall the same dependencies every time you build, right?

   To fix this, restructure your Dockerfile so that the dependency cache remains valid unless it really needs to be invalidated. For Node-based applications, dependencies are defined in the `package.json` file. You'll want to reinstall the dependencies if that file changes, but use cached dependencies if the file is unchanged. So, start by copying only that file first, then install the dependencies, and finally copy everything else. Then, you only need to recreate the yarn dependencies if there was a change to the `package.json` file.

6. Update the Dockerfile to copy in the `package.json` file first, install dependencies, and then copy everything else in.

   

   ```dockerfile
   FROM node:20-alpine
   WORKDIR /app
   COPY package.json yarn.lock ./
   RUN yarn install --production 
   COPY . . 
   EXPOSE 3000
   CMD ["node", "src/index.js"]
   ```

7. Create a file named `.dockerignore` in the same folder as the Dockerfile with the following contents.

   

   ```plaintext
   node_modules
   ```

8. Build the new image:

   

   ```console
   $ docker build .
   ```

   You'll then see output similar to the following:

   

   ```console
   [+] Building 16.1s (10/10) FINISHED
   => [internal] load build definition from Dockerfile                                               0.0s
   => => transferring dockerfile: 175B                                                               0.0s
   => [internal] load .dockerignore                                                                  0.0s
   => => transferring context: 2B                                                                    0.0s
   => [internal] load metadata for docker.io/library/node:21-alpine                                  0.0s
   => [internal] load build context                                                                  0.8s
   => => transferring context: 53.37MB                                                               0.8s
   => [1/5] FROM docker.io/library/node:21-alpine                                                    0.0s
   => CACHED [2/5] WORKDIR /app                                                                      0.0s
   => [3/5] COPY package.json yarn.lock ./                                                           0.2s
   => [4/5] RUN yarn install --production                                                           14.0s
   => [5/5] COPY . .                                                                                 0.5s
   => exporting to image                                                                             0.6s
   => => exporting layers                                                                            0.6s
   => => writing image     
   sha256:d6f819013566c54c50124ed94d5e66c452325327217f4f04399b45f94e37d25        0.0s
   => => naming to docker.io/library/node-app:2.0                                                 0.0s
   ```

   You'll see that all layers were rebuilt. Perfectly fine since you changed the Dockerfile quite a bit.

9. Now, make a change to the `src/static/index.html` file (like change the title to say "The Awesome Todo App").

10. Build the Docker image. This time, your output should look a little different.

    

    ```console
    $ docker build -t node-app:3.0 .
    ```

    You'll then see output similar to the following:

    

    ```console
    [+] Building 1.2s (10/10) FINISHED 
    => [internal] load build definition from Dockerfile                                               0.0s
    => => transferring dockerfile: 37B                                                                0.0s
    => [internal] load .dockerignore                                                                  0.0s
    => => transferring context: 2B                                                                    0.0s
    => [internal] load metadata for docker.io/library/node:21-alpine                                  0.0s 
    => [internal] load build context                                                                  0.2s
    => => transferring context: 450.43kB                                                              0.2s
    => [1/5] FROM docker.io/library/node:21-alpine                                                    0.0s
    => CACHED [2/5] WORKDIR /app                                                                      0.0s
    => CACHED [3/5] COPY package.json yarn.lock ./                                                    0.0s
    => CACHED [4/5] RUN yarn install --production                                                     0.0s
    => [5/5] COPY . .                                                                                 0.5s 
    => exporting to image                                                                             0.3s
    => => exporting layers                                                                            0.3s
    => => writing image     
    sha256:91790c87bcb096a83c2bd4eb512bc8b134c757cda0bdee4038187f98148e2eda       0.0s
    => => naming to docker.io/library/node-app:3.0                                                 0.0s
    ```

    First off, you should notice that the build was much faster. You'll see that several steps are using previously cached layers. That's good news; you're using the build cache. Pushing and pulling this image and updates to it will be much faster as well.

By following these optimization techniques, you can make your Docker builds faster and more efficient, leading to quicker iteration cycles and improved development productivity.

## [Additional resources](https://docs.docker.com/get-started/docker-concepts/building-images/using-the-build-cache/#additional-resources)

- [Optimizing builds with cache management](https://docs.docker.com/build/cache/)
- [Cache Storage Backend](https://docs.docker.com/build/cache/backends/)
- [Build cache invalidation](https://docs.docker.com/build/cache/invalidation/)

## [Next steps](https://docs.docker.com/get-started/docker-concepts/building-images/using-the-build-cache/#next-steps)

Now that you understand how to use the Docker build cache effectively, you're ready to learn about Multi-stage builds.

[Multi-stage builds](https://docs.docker.com/get-started/docker-concepts/building-images/multi-stage-builds/)
