+++
title = "Use the docker dev CLI plugin"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/dev-environments/dev-cli/](https://docs.docker.com/desktop/dev-environments/dev-cli/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use the docker dev CLI plugin

> **Important**
>
> 
>
> Dev Environments is no longer under active development.
>
> While the current functionality remains available, it may take us longer to respond to support requests.

Use the new `docker dev` CLI plugin to get the full Dev Environments experience from the terminal in addition to the Dashboard.

It is available with [Docker Desktop 4.13.0 and later](https://docs.docker.com/desktop/release-notes/).

### [Usage](https://docs.docker.com/desktop/dev-environments/dev-cli/#usage)



```bash
docker dev [OPTIONS] COMMAND
```

### [Commands](https://docs.docker.com/desktop/dev-environments/dev-cli/#commands)

| Command   | Description                              |
| :-------- | :--------------------------------------- |
| `check`   | Check Dev Environments                   |
| `create`  | Create a new dev environment             |
| `list`    | Lists all dev environments               |
| `logs`    | Traces logs from a dev environment       |
| `open`    | Open Dev Environment with the IDE        |
| `rm`      | Removes a dev environment                |
| `start`   | Starts a dev environment                 |
| `stop`    | Stops a dev environment                  |
| `version` | Shows the Docker Dev version information |

### [`docker dev check`](https://docs.docker.com/desktop/dev-environments/dev-cli/#docker-dev-check)

#### [Usage](https://docs.docker.com/desktop/dev-environments/dev-cli/#usage-1)

```
docker dev check [OPTIONS]
```

#### [Options](https://docs.docker.com/desktop/dev-environments/dev-cli/#options)

| Name, shorthand | Description        |
| :-------------- | :----------------- |
| `--format`,`-f` | Format the output. |

### [`docker dev create`](https://docs.docker.com/desktop/dev-environments/dev-cli/#docker-dev-create)

#### [Usage](https://docs.docker.com/desktop/dev-environments/dev-cli/#usage-2)

```
docker dev create [OPTIONS] REPOSITORY_URL
```

#### [Options](https://docs.docker.com/desktop/dev-environments/dev-cli/#options-1)

| Name, shorthand | Description                                              |
| :-------------- | :------------------------------------------------------- |
| `--detach`,`-d` | Detach creates a Dev Env without attaching to it's logs. |
| `--open`,`-o`   | Open IDE after a successful creation                     |

### [`docker dev list`](https://docs.docker.com/desktop/dev-environments/dev-cli/#docker-dev-list)

#### [Usage](https://docs.docker.com/desktop/dev-environments/dev-cli/#usage-3)

```
docker dev list [OPTIONS]
```

#### [Options](https://docs.docker.com/desktop/dev-environments/dev-cli/#options-2)

| Name, shorthand | Description                      |
| :-------------- | :------------------------------- |
| `--format`,`-f` | Format the output                |
| `--quiet`,`-q`  | Only show dev environments names |

### [`docker dev logs`](https://docs.docker.com/desktop/dev-environments/dev-cli/#docker-dev-logs)

#### [Usage](https://docs.docker.com/desktop/dev-environments/dev-cli/#usage-4)

```
docker dev logs [OPTIONS] DEV_ENV_NAME
```

### [`docker dev open`](https://docs.docker.com/desktop/dev-environments/dev-cli/#docker-dev-open)

#### [Usage](https://docs.docker.com/desktop/dev-environments/dev-cli/#usage-5)

```
docker dev open DEV_ENV_NAME CONTAINER_REF [OPTIONS]
```

#### [Options](https://docs.docker.com/desktop/dev-environments/dev-cli/#options-3)

| Name, shorthand | Description |
| :-------------- | :---------- |
| `--editor`,`-e` | Editor.     |

### [`docker dev rm`](https://docs.docker.com/desktop/dev-environments/dev-cli/#docker-dev-rm)

#### [Usage](https://docs.docker.com/desktop/dev-environments/dev-cli/#usage-6)

```
docker dev rm DEV_ENV_NAME
```

### [`docker dev start`](https://docs.docker.com/desktop/dev-environments/dev-cli/#docker-dev-start)

#### [Usage](https://docs.docker.com/desktop/dev-environments/dev-cli/#usage-7)

```
docker dev start DEV_ENV_NAME
```

### [`docker dev stop`](https://docs.docker.com/desktop/dev-environments/dev-cli/#docker-dev-stop)

#### [Usage](https://docs.docker.com/desktop/dev-environments/dev-cli/#usage-8)

```
docker dev stop DEV_ENV_NAME
```

### [`docker dev version`](https://docs.docker.com/desktop/dev-environments/dev-cli/#docker-dev-version)

#### [Usage](https://docs.docker.com/desktop/dev-environments/dev-cli/#usage-9)

```
docker dev version [OPTIONS]
```

#### [Options](https://docs.docker.com/desktop/dev-environments/dev-cli/#options-4)

| Name, shorthand | Description                             |
| :-------------- | :-------------------------------------- |
| `--format`,`-f` | Format the output.                      |
| `--short`,`-s`  | Shows only Docker Dev's version number. |
