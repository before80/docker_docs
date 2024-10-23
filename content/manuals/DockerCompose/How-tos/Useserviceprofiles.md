+++
title = "Use service profiles"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/compose/how-tos/profiles/](https://docs.docker.com/compose/how-tos/profiles/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Using profiles with Compose

Profiles help you adjust your Compose application for different environments or use cases by selectively activating services. Services can be assigned to one or more profiles; unassigned services start by default, while assigned ones only start when their profile is active. This setup means specific services, like those for debugging or development, to be included in a single `compose.yml` file and activated only as needed.

## [Assigning profiles to services](https://docs.docker.com/compose/how-tos/profiles/#assigning-profiles-to-services)

Services are associated with profiles through the [`profiles` attribute](https://docs.docker.com/reference/compose-file/services/#profiles) which takes an array of profile names:



```yaml
services:
  frontend:
    image: frontend
    profiles: [frontend]

  phpmyadmin:
    image: phpmyadmin
    depends_on: [db]
    profiles: [debug]

  backend:
    image: backend

  db:
    image: mysql
```

Here the services `frontend` and `phpmyadmin` are assigned to the profiles `frontend` and `debug` respectively and as such are only started when their respective profiles are enabled.

Services without a `profiles` attribute are always enabled. In this case running `docker compose up` would only start `backend` and `db`.

Valid profiles names follow the regex format of `[a-zA-Z0-9][a-zA-Z0-9_.-]+`.

> **Tip**
>
> 
>
> The core services of your application shouldn't be assigned `profiles` so they are always enabled and automatically started.

## [Start specific profiles](https://docs.docker.com/compose/how-tos/profiles/#start-specific-profiles)

To start a specific profile supply the `--profile` [command-line option](https://docs.docker.com/reference/cli/docker/compose/) or use the [`COMPOSE_PROFILES` environment variable](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_profiles):



```console
$ docker compose --profile debug up
```



```console
$ COMPOSE_PROFILES=debug docker compose up
```

Both commands start the services with the `debug` profile enabled. In the previous `compose.yml` file, this starts the services `db`, `backend` and `phpmyadmin`.

### [Start multiple profiles](https://docs.docker.com/compose/how-tos/profiles/#start-multiple-profiles)

You can also enable multiple profiles, e.g. with `docker compose --profile frontend --profile debug up` the profiles `frontend` and `debug` will be enabled.

Multiple profiles can be specified by passing multiple `--profile` flags or a comma-separated list for the `COMPOSE_PROFILES` environment variable:



```console
$ docker compose --profile frontend --profile debug up
```



```console
$ COMPOSE_PROFILES=frontend,debug docker compose up
```

If you want to enable all profiles at the same time, you can run `docker compose --profile "*"`.

## [Auto-starting profiles and dependency resolution](https://docs.docker.com/compose/how-tos/profiles/#auto-starting-profiles-and-dependency-resolution)

When a service with assigned `profiles` is explicitly targeted on the command line its profiles are started automatically so you don't need to start them manually. This can be used for one-off services and debugging tools. As an example consider the following configuration:



```yaml
services:
  backend:
    image: backend

  db:
    image: mysql

  db-migrations:
    image: backend
    command: myapp migrate
    depends_on:
      - db
    profiles:
      - tools
```



```sh
# Only start backend and db
$ docker compose up -d

# This runs db-migrations (and,if necessary, start db)
# by implicitly enabling the profiles `tools`
$ docker compose run db-migrations
```

But keep in mind that `docker compose` only automatically starts the profiles of the services on the command line and not of any dependencies.

This means that any other services the targeted service `depends_on` should either:

- Share a common profile
- Always be started, by omitting `profiles` or having a matching profile started explicitly



```yaml
services:
  web:
    image: web

  mock-backend:
    image: backend
    profiles: ["dev"]
    depends_on:
      - db

  db:
    image: mysql
    profiles: ["dev"]

  phpmyadmin:
    image: phpmyadmin
    profiles: ["debug"]
    depends_on:
      - db
```



```sh
# Only start "web"
$ docker compose up -d

# Start mock-backend (and, if necessary, db)
# by implicitly enabling profiles `dev`
$ docker compose up -d mock-backend

# This fails because profiles "dev" is not enabled
$ docker compose up phpmyadmin
```

Although targeting `phpmyadmin` automatically starts the profiles `debug`, it doesn't automatically start the profiles required by `db` which is `dev`.

To fix this you either have to add the `debug` profile to the `db` service:



```yaml
db:
  image: mysql
  profiles: ["debug", "dev"]
```

or start the `dev` profile explicitly:



```console
# Profiles "debug" is started automatically by targeting phpmyadmin
$ docker compose --profile dev up phpmyadmin
$ COMPOSE_PROFILES=dev docker compose up phpmyadmin
```

## [Stop specific profiles](https://docs.docker.com/compose/how-tos/profiles/#stop-specific-profiles)

As with starting specific profiles, you can use the `--profile` [command-line option](https://docs.docker.com/reference/cli/docker/compose/#use--p-to-specify-a-project-name) or use the [`COMPOSE_PROFILES` environment variable](https://docs.docker.com/compose/how-tos/environment-variables/envvars/#compose_profiles):



```console
$ docker compose --profile debug down
```



```console
$ COMPOSE_PROFILES=debug docker compose down
```

Both commands stop and remove services with the `debug` profile. In the following `compose.yml` file, this stops the services `db` and `phpmyadmin`.



```yaml
services:
  frontend:
    image: frontend
    profiles: [frontend]

  phpmyadmin:
    image: phpmyadmin
    depends_on: [db]
    profiles: [debug]

  backend:
    image: backend

  db:
    image: mysql
```

> **Note**
>
> 
>
> Running `docker compose down` only stops `backend` and `db`.

## [Reference information](https://docs.docker.com/compose/how-tos/profiles/#reference-information)

[`profiles`](https://docs.docker.com/reference/compose-file/services/#profiles)
