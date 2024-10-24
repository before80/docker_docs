+++
title = "docker search"
date = 2024-10-23T14:54:43+08:00
weight = 190
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/search/](https://docs.docker.com/reference/cli/docker/search/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker search

| Description | Search Docker Hub for images   |
| :---------- | ------------------------------ |
| Usage       | `docker search [OPTIONS] TERM` |

## Description

Search [Docker Hub](https://hub.docker.com/) for images

## Options

| Option                                                       | Default | Description                                |
| ------------------------------------------------------------ | ------- | ------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/search/#filter) |         | Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/search/#format) |         | Pretty-print search using a Go template    |
| [`--limit`](https://docs.docker.com/reference/cli/docker/search/#limit) |         | Max number of search results               |
| [`--no-trunc`](https://docs.docker.com/reference/cli/docker/search/#no-trunc) |         | Don't truncate output                      |

## Examples

### Search images by name

This example displays images with a name containing 'busybox':



```console
$ docker search busybox

NAME                             DESCRIPTION                                     STARS     OFFICIAL
busybox                          Busybox base image.                             316       [OK]
progrium/busybox                                                                 50
radial/busyboxplus               Full-chain, Internet enabled, busybox made...   8
odise/busybox-python                                                             2
azukiapp/busybox                 This image is meant to be used as the base...   2
ofayau/busybox-jvm               Prepare busybox to install a 32 bits JVM.       1
shingonoide/archlinux-busybox    Arch Linux, a lightweight and flexible Lin...   1
odise/busybox-curl                                                               1
ofayau/busybox-libc32            Busybox with 32 bits (and 64 bits) libs         1
peelsky/zulu-openjdk-busybox                                                     1
skomma/busybox-data              Docker image suitable for data volume cont...   1
elektritter/busybox-teamspeak    Lightweight teamspeak3 container based on...    1
socketplane/busybox                                                              1
oveits/docker-nginx-busybox      This is a tiny NginX docker image based on...   0
ggtools/busybox-ubuntu           Busybox ubuntu version with extra goodies       0
nikfoundas/busybox-confd         Minimal busybox based distribution of confd     0
openshift/busybox-http-app                                                       0
jllopis/busybox                                                                  0
swyckoff/busybox                                                                 0
powellquiring/busybox                                                            0
williamyeh/busybox-sh            Docker image for BusyBox's sh                   0
simplexsys/busybox-cli-powered   Docker busybox images, with a few often us...   0
fhisamoto/busybox-java           Busybox java                                    0
scottabernethy/busybox                                                           0
marclop/busybox-solr
```

### Display non-truncated description (--no-trunc)

This example displays images with a name containing 'busybox', at least 3 stars and the description isn't truncated in the output:



```console
$ docker search --filter=stars=3 --no-trunc busybox

NAME                 DESCRIPTION                                                                               STARS     OFFICIAL
busybox              Busybox base image.                                                                       325       [OK]
progrium/busybox                                                                                               50
radial/busyboxplus   Full-chain, Internet enabled, busybox made from scratch. Comes in git and cURL flavors.   8
```

### Limit search results (--limit)

The flag `--limit` is the maximum number of results returned by a search. If no value is set, the default is set by the daemon.

### Filtering (--filter)

The filtering flag (`-f` or `--filter`) format is a `key=value` pair. If there is more than one filter, then pass multiple flags (e.g. `--filter is-official=true --filter stars=3`).

The currently supported filters are:

- stars (int - number of stars the image has)
- is-automated (boolean - true or false) - is the image automated or not (deprecated)
- is-official (boolean - true or false) - is the image official or not

#### stars

This example displays images with a name containing 'busybox' and at least 3 stars:



```console
$ docker search --filter stars=3 busybox

NAME                 DESCRIPTION                                     STARS     OFFICIAL
busybox              Busybox base image.                             325       [OK]
progrium/busybox                                                     50
radial/busyboxplus   Full-chain, Internet enabled, busybox made...   8
```

#### is-official

This example displays images with a name containing 'busybox', at least 3 stars and are official builds:



```console
$ docker search --filter is-official=true --filter stars=3 busybox

NAME      DESCRIPTION           STARS     OFFICIAL
busybox   Busybox base image.   325       [OK]
```

### Format the output (--format)

The formatting option (`--format`) pretty-prints search output using a Go template.

Valid placeholders for the Go template are:

| Placeholder    | Description                                    |
| -------------- | ---------------------------------------------- |
| `.Name`        | Image Name                                     |
| `.Description` | Image description                              |
| `.StarCount`   | Number of stars for the image                  |
| `.IsOfficial`  | "OK" if image is official                      |
| `.IsAutomated` | "OK" if image build was automated (deprecated) |

When you use the `--format` option, the `search` command will output the data exactly as the template declares. If you use the `table` directive, column headers are included as well.

The following example uses a template without headers and outputs the `Name` and `StarCount` entries separated by a colon (`:`) for all images:



```console
$ docker search --format "{{.Name}}: {{.StarCount}}" nginx

nginx: 5441
jwilder/nginx-proxy: 953
richarvey/nginx-php-fpm: 353
million12/nginx-php: 75
webdevops/php-nginx: 70
h3nrik/nginx-ldap: 35
bitnami/nginx: 23
evild/alpine-nginx: 14
million12/nginx: 9
maxexcloo/nginx: 7
```

This example outputs a table format:



```console
$ docker search --format "table {{.Name}}\t{{.IsOfficial}}" nginx

NAME                                     OFFICIAL
nginx                                    [OK]
jwilder/nginx-proxy
richarvey/nginx-php-fpm
jrcs/letsencrypt-nginx-proxy-companion
million12/nginx-php
webdevops/php-nginx
```
