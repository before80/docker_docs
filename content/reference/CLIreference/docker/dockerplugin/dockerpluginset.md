+++
title = "docker plugin set"
date = 2024-10-23T14:54:43+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/plugin/set/](https://docs.docker.com/reference/cli/docker/plugin/set/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker plugin set

| Description | Change settings for a plugin                        |
| :---------- | --------------------------------------------------- |
| Usage       | `docker plugin set PLUGIN KEY=VALUE [KEY=VALUE...]` |

## Description

Change settings for a plugin. The plugin must be disabled.

The settings currently supported are:

- env variables
- source of mounts
- path of devices
- args

## Examples

### Change an environment variable

The following example change the env variable `DEBUG` on the `sample-volume-plugin` plugin.



```console
$ docker plugin inspect -f {{.Settings.Env}} tiborvass/sample-volume-plugin
[DEBUG=0]

$ docker plugin set tiborvass/sample-volume-plugin DEBUG=1

$ docker plugin inspect -f {{.Settings.Env}} tiborvass/sample-volume-plugin
[DEBUG=1]
```

### Change the source of a mount

The following example change the source of the `mymount` mount on the `myplugin` plugin.



```console
$ docker plugin inspect -f '{{with $mount := index .Settings.Mounts 0}}{{$mount.Source}}{{end}}' myplugin
/foo

$ docker plugins set myplugin mymount.source=/bar

$ docker plugin inspect -f '{{with $mount := index .Settings.Mounts 0}}{{$mount.Source}}{{end}}' myplugin
/bar
```

> **Note**
>
> Since only `source` is settable in `mymount`, `docker plugins set mymount=/bar myplugin` would work too.

### Change a device path

The following example change the path of the `mydevice` device on the `myplugin` plugin.



```console
$ docker plugin inspect -f '{{with $device := index .Settings.Devices 0}}{{$device.Path}}{{end}}' myplugin

/dev/foo

$ docker plugins set myplugin mydevice.path=/dev/bar

$ docker plugin inspect -f '{{with $device := index .Settings.Devices 0}}{{$device.Path}}{{end}}' myplugin

/dev/bar
```

> **Note**
>
> Since only `path` is settable in `mydevice`, `docker plugins set mydevice=/dev/bar myplugin` would work too.

### Change the source of the arguments

The following example change the value of the args on the `myplugin` plugin.



```console
$ docker plugin inspect -f '{{.Settings.Args}}' myplugin

["foo", "bar"]

$ docker plugins set myplugin myargs="foo bar baz"

$ docker plugin inspect -f '{{.Settings.Args}}' myplugin

["foo", "bar", "baz"]
```
