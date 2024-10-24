+++
title = "Volumes"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/use-desktop/volumes/](https://docs.docker.com/desktop/use-desktop/volumes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Explore the Volumes view in Docker Desktop

The **Volumes** view in Docker Dashboard lets you create, delete, and perform other actions on your [volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}). You can also see which volumes are being used as well as inspect the files and folders in your volumes.

## View your volumes

You can view the following information about your volumes:

- Name: The name of the volume.
- Status: Whether the volume is in-use by a container or not.
- Created: How long ago the volume was created.
- Size: The size of the volume.
- Scheduled exports: Whether a scheduled export is active or not.

By default, the **Volumes** view displays a list of all the volumes.

You can filter and sort volumes as well as modify which columns are displayed by doing the following:

- Filter volumes by name: Use the **Search** field.
- Filter volumes by status: To the right of the search bar, filter volumes by **In use** or **Unused**.
- Sort volumes: Select a column name to sort the volumes.
- Customize columns: To the right of the search bar, choose what volume information to display.

## Create a volume

You use the following steps to create an empty volume. Alternatively, if you [start a container with a volume](https://docs.docker.com/engine/storage/volumes/#start-a-container-with-a-volume) that doesn't yet exist, Docker creates the volume for you.

To create a volume:

1. In the **Volumes** view, select the **Create** button.
2. In the **New Volume** modal, specify a volume name, and then select **Create**.

To use the volume with a container, see [Use volumes](https://docs.docker.com/engine/storage/volumes/#start-a-container-with-a-volume).

## Inspect a volume

To explore the details of a specific volume, select a volume from the list. This opens the detailed view.

The **Container in-use** tab displays the name of the container using the volume, the image name, the port number used by the container, and the target. A target is a path inside a container that gives access to the files in the volume.

The **Stored data** tab displays the files and folders in the volume and the file size. To save a file or a folder, right-click on the file or folder to display the options menu, select **Save as...**, and then specify a location to download the file.

To delete a file or a folder from the volume, right-click on the file or folder to display the options menu, select **Delete**, and then select **Delete** again to confirm.

The **Exports** tab lets you [export the volume](https://docs.docker.com/desktop/use-desktop/volumes/#export-a-volume).

## Clone a volume

Cloning a volume creates a new volume with a copy of all of the data from the cloned volume. When cloning a volume used by one or more running containers, the containers are temporarily stopped while Docker clones the data, and then restarted when the cloning process is completed.

To clone a volume:

1. Sign in to Docker Desktop. You must be signed in to clone a volume.
2. In the **Volumes** view, select the **Clone** icon in the **Actions** column for the volume you want to clone.
3. In the **Clone a volume** modal, specify a **Volume name**, and then select **Clone**.

## Delete one or more volumes

Deleting a volume deletes the volume and all its data. When a container is using a volume, you can't delete the volume, even if the container is stopped. You must first stop and remove any containers using the volume before you can delete the volume.

To delete a volume:

1. In the **Volumes** view, select **Delete** icon in the **Actions** column for the volume you want to delete.
2. In the **Delete volume?** modal, select **Delete forever**.

To delete multiple volumes:

1. In the **Volumes** view, select the checkbox next to all the volumes you want to delete.
2. Select **Delete**.
3. In the **Delete volumes?** modal, select **Delete forever**.

## Empty a volume

Emptying a volume deletes all a volume's data, but doesn't delete the volume. When emptying a volume used by one or more running containers, the containers are temporarily stopped while Docker empties the data, and then restarted when the emptying process is completed.

To empty a volume:

1. Sign in to Docker Desktop. You must be signed in to empty a volume.
2. In the **Volumes** view, select the volume you want to empty.
3. Next to **Import**, select the **More volume actions** icon, and then select **Empty volume**.
4. In the **Empty a volume?** modal, select **Empty**.

## Export a volume

**Beta feature**

The export volume feature is currently in [Beta](https://docs.docker.com/release-lifecycle/#beta).

You can export the content of a volume to a local file, a local image, an to an image in Docker Hub, or to a supported cloud provider. When exporting content from a volume used by one or more running containers, the containers are temporarily stopped while Docker exports the content, and then restarted when the export process is completed.

You can either [export a volume now](https://docs.docker.com/desktop/use-desktop/volumes/#export-a-volume-now) or [schedule a recurring export](https://docs.docker.com/desktop/use-desktop/volumes/#schedule-a-volume-export).

### Export a volume now

1. Sign in to Docker Desktop. You must be signed in to export a volume.

2. In the **Volumes** view, select the volume you want to export.

3. Select the **Exports** tab.

4. Select **Quick export**.

5. Select whether to export the volume to **Local or Hub storage** or **External cloud storage**, then specify the following additional details depending on your selection.

   Local or Hub storage External cloud storage

   ------

   - **Local file**: Specify a file name and select a folder.
   - **Local image**: Select a local image to export the content to. Any existing data in the image will be replaced by the exported content.
   - **New image**: Specify a name for the new image.
   - **Registry**: Specify a Docker Hub repository. Note that Docker Hub repositories can be publicly accessible which means your data can be publicly accessible. For more details, see [Change a repository from public to private](https://docs.docker.com/docker-hub/repos/#change-a-repository-from-public-to-private).

   ------

6. Select **Save**.

### Schedule a volume export

1. Sign in to Docker Desktop. You must be signed in and have a paid [Docker subscription]({{< ref "/manuals/Subscription/DockerCore/Subscriptionsandfeatures" >}}) to schedule a volume export.

2. In the **Volumes** view, select the volume you want to export.

3. Select the **Exports** tab.

4. Select **Schedule export**.

5. In **Recurrence**, select how often the export occurs, and then specify the following additional details based on your selection.

   - **Daily**: Specify the time that the backup occurs each day.
   - **Weekly**: Specify one or more days, and the time that the backup occurs each week.
   - **Monthly**: Specify which day of the month and the time that the backup occurs each month.

6. Select whether to export the volume to **Local or Hub storage** or **External cloud storage**, then specify the following additional details depending on your selection.

   Local or Hub storage External cloud storage

   ------

   - **Local file**: Specify a file name and select a folder.
   - **Local image**: Select a local image to export the content to. Any existing data in the image will be replaced by the exported content.
   - **New image**: Specify a name for the new image.
   - **Registry**: Specify a Docker Hub repository. Note that Docker Hub repositories can be publicly accessible which means your data can be publicly accessible. For more details, see [Change a repository from public to private](https://docs.docker.com/docker-hub/repos/#change-a-repository-from-public-to-private).

   ------

7. Select **Save**.

## Import a volume

You can import a local file, a local image, or an image from Docker Hub. Any existing data in the volume is replaced by the imported content. When importing content to a volume used by one or more running containers, the containers are temporarily stopped while Docker imports the content, and then restarted when the import process is completed.

To import a volume:

1. Sign in to Docker Desktop. You must be signed in to import a volume.
2. Optionally, [create](https://docs.docker.com/desktop/use-desktop/volumes/#create-a-volume) a new volume to import the content into.
3. Select the volume you want to import content in to.
4. Select **Import**.
5. Select where the content is coming from and then specify the following additional details depending on your selection:
   - **Local file**: Select the file that contains the content.
   - **Local image**: Select the local image that contains the content.
   - **Registry**: Specify the image from Docker Hub that contains the content.
6. Select **Import**.

## Additional resources

- [Persisting container data]({{< ref "/get-started/Dockerconcepts/Runningcontainers/Persistingcontainerdata" >}})
- [Use volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})
