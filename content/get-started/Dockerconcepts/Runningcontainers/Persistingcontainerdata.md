+++
title = "Persisting container data"
date = 2024-10-23T14:54:35+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/running-containers/persisting-container-data/](https://docs.docker.com/get-started/docker-concepts/running-containers/persisting-container-data/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Persisting container data

<iframe id="youtube-player-10_2BjqB_Ls" data-video-id="10_2BjqB_Ls" class="youtube-video aspect-video h-fit w-full py-2" frameborder="0" allowfullscreen="" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" title="Docker Concepts - Persisting container data" width="100%" height="100%" src="https://www.youtube.com/embed/10_2BjqB_Ls?rel=0&amp;iv_load_policy=3&amp;enablejsapi=1&amp;origin=https%3A%2F%2Fdocs.docker.com&amp;widgetid=1" data-gtm-yt-inspected-24="true" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border-width: 0px; border-style: solid; border-color: initial; display: block; vertical-align: middle; aspect-ratio: 16 / 9; height: fit-content; width: 634.672px; padding-top: 0.5rem; padding-bottom: 0.5rem; color: rgb(0, 0, 0); font-family: &quot;Roboto Flex&quot;, system-ui, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Oxygen, Ubuntu, Cantarell, &quot;Open Sans&quot;, &quot;Helvetica Neue&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>

## Explanation

When a container starts, it uses the files and configuration provided by the image. Each container is able to create, modify, and delete files and does so without affecting any other containers. When the container is deleted, these file changes are also deleted.

While this ephemeral nature of containers is great, it poses a challenge when you want to persist the data. For example, if you restart a database container, you might not want to start with an empty database. So, how do you persist files?

### Container volumes

Volumes are a storage mechanism that provide the ability to persist data beyond the lifecycle of an individual container. Think of it like providing a shortcut or symlink from inside the container to outside the container.

As an example, imagine you create a volume named `log-data`.



```console
$ docker volume create log-data
```

When starting a container with the following command, the volume will be mounted (or attached) into the container at `/logs`:



```console
$ docker run -d -p 80:80 -v log-data:/logs docker/welcome-to-docker
```

If the volume `log-data` doesn't exist, Docker will automatically create it for you.

When the container runs, all files it writes into the `/logs` folder will be saved in this volume, outside of the container. If you delete the container and start a new container using the same volume, the files will still be there.

> **Sharing files using volumes**
>
> You can attach the same volume to multiple containers to share files between containers. This might be helpful in scenarios such as log aggregation, data pipelines, or other event-driven applications.

### Managing volumes

Volumes have their own lifecycle beyond that of containers and can grow quite large depending on the type of data and applications you’re using. The following commands will be helpful to manage volumes:

- `docker volume ls` - list all volumes
- `docker volume rm <volume-name-or-id>` - remove a volume (only works when the volume is not attached to any containers)
- `docker volume prune` - remove all unused (unattached) volumes

## Try it out

In this guide, you’ll practice creating and using volumes to persist data created by a Postgres container. When the database runs, it stores files into the `/var/lib/postgresql/data` directory. By attaching the volume here, you will be able to restart the container multiple times while keeping the data.

### Use volumes

1. [Download and install]({{< ref "/get-started/GetDocker" >}}) Docker Desktop.

2. Start a container using the [Postgres image](https://hub.docker.com/_/postgres) with the following command:

   

   ```console
   $ docker run --name=db -e POSTGRES_PASSWORD=secret -d -v postgres_data:/var/lib/postgresql/data postgres
   ```

   This will start the database in the background, configure it with a password, and attach a volume to the directory PostgreSQL will persist the database files.

3. Connect to the database by using the following command:

   

   ```console
   $ docker exec -ti db psql -U postgres
   ```

4. In the PostgreSQL command line, run the following to create a database table and insert two records:

   

   ```text
   CREATE TABLE tasks (
       id SERIAL PRIMARY KEY,
       description VARCHAR(100)
   );
   INSERT INTO tasks (description) VALUES ('Finish work'), ('Have fun');
   ```

5. Verify the data is in the database by running the following in the PostgreSQL command line:

   

   ```text
   SELECT * FROM tasks;
   ```

   You should get output that looks like the following:

   

   ```text
    id | description
   ----+-------------
     1 | Finish work
     2 | Have fun
   (2 rows)
   ```

6. Exit out of the PostgreSQL shell by running the following command:

   

   ```console
   \q
   ```

7. Stop and remove the database container. Remember that, even though the container has been deleted, the data is persisted in the `postgres_data` volume.

   

   ```console
   $ docker stop db
   $ docker rm db
   ```

8. Start a new container by running the following command, attaching the same volume with the persisted data:

   

   ```console
   $ docker run --name=new-db -d -v postgres_data:/var/lib/postgresql/data postgres 
   ```

   You might have noticed that the `POSTGRES_PASSWORD` environment variable has been omitted. That’s because that variable is only used when bootstrapping a new database.

9. Verify the database still has the records by running the following command:

   

   ```console
   $ docker exec -ti new-db psql -U postgres -c "SELECT * FROM tasks"
   ```

### View volume contents

The Docker Dashboard provides the ability to view the contents of any volume, as well as the ability to export, import, and clone volumes.

1. Open the Docker Dashboard and navigate to the **Volumes** view. In this view, you should see the **postgres_data** volume.
2. Select the **postgres_data** volume’s name.
3. The **Data** tab shows the contents of the volume and provides the ability to navigate the files. Double-clicking on a file will let you see the contents and make changes.
4. Right-click on any file to save it or delete it.

### Remove volumes

Before removing a volume, it must not be attached to any containers. If you haven’t removed the previous container, do so with the following command (the `-f` will stop the container first and then remove it):



```console
$ docker rm -f new-db
```

There are a few methods to remove volumes, including the following:

- Select the **Delete Volume** option on a volume in the Docker Dashboard.

- Use the `docker volume rm` command:

  

  ```console
  $ docker volume rm postgres_data
  ```

- Use the `docker volume prune` command to remove all unused volumes:

  

  ```console
  $ docker volume prune
  ```

## Additional resources

The following resources will help you learn more about volumes:

- [Manage data in Docker]({{< ref "/manuals/DockerEngine/Storage" >}})
- [Volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})
- [Volume mounts](https://docs.docker.com/engine/containers/run/#volume-mounts)

## Next steps

Now that you have learned about persisting container data, it’s time to learn about sharing local files with containers.

[Sharing local files with containers]({{< ref "/get-started/Dockerconcepts/Runningcontainers/Sharinglocalfileswithcontainers" >}})
