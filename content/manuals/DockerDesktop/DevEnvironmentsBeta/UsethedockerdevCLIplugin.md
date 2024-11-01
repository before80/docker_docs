+++
title = "使用 docker dev CLI 插件"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/dev-environments/dev-cli/](https://docs.docker.com/desktop/dev-environments/dev-cli/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use the docker dev CLI plugin - 使用 docker dev CLI 插件

> **Important**
>
> 
>
> Dev Environments is no longer under active development.
>
> ​	开发环境不再进行活跃开发。
>
> While the current functionality remains available, it may take us longer to respond to support requests.
>
> ​	尽管当前功能仍然可用，但我们可能会对支持请求的响应时间更长。

Use the new `docker dev` CLI plugin to get the full Dev Environments experience from the terminal in addition to the Dashboard.

​	使用新的 `docker dev` CLI 插件，您可以直接在终端获得完整的开发环境体验。

It is available with [Docker Desktop 4.13.0 and later]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}).

​	该功能在 [Docker Desktop 4.13.0 及更高版本]({{< ref "/manuals/DockerDesktop/Releasenotes" >}})中可用。Usage



```bash
docker dev [OPTIONS] COMMAND
```

### 用法 Commands

| Command   | Description                                                  |
| :-------- | :----------------------------------------------------------- |
| `check`   | 检查开发环境 Check Dev Environments                          |
| `create`  | 创建一个新开发环境 Create a new dev environment              |
| `list`    | 列出所有开发环境 Lists all dev environments                  |
| `logs`    | 跟踪开发环境的日志 Traces logs from a dev environment        |
| `open`    | 使用 IDE 打开开发环境 Open Dev Environment with the IDE      |
| `rm`      | 删除一个开发环境 Removes a dev environment                   |
| `start`   | 启动一个开发环境 Starts a dev environment                    |
| `stop`    | 停止一个开发环境 Stops a dev environment                     |
| `version` | 显示 Docker Dev 版本信息 Shows the Docker Dev version information |

### `docker dev check`

#### Usage

```
docker dev check [OPTIONS]
```

#### Options

| Name, shorthand | Description                     |
| :-------------- | :------------------------------ |
| `--format`,`-f` | 格式化输出。 Format the output. |

### `docker dev create`

#### Usage

```
docker dev create [OPTIONS] REPOSITORY_URL
```

#### Options

| Name, shorthand | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| `--detach`,`-d` | 分离模式创建开发环境而不附加到日志。 Detach creates a Dev Env without attaching to it's logs. |
| `--open`,`-o`   | 成功创建后打开 IDE - Open IDE after a successful creation    |

### `docker dev list`

#### Usage

```
docker dev list [OPTIONS]
```

#### Options

| Name, shorthand | Description                                         |
| :-------------- | :-------------------------------------------------- |
| `--format`,`-f` | 格式化输出。 Format the output                      |
| `--quiet`,`-q`  | 仅显示开发环境名称 Only show dev environments names |

### `docker dev logs`

#### Usage

```
docker dev logs [OPTIONS] DEV_ENV_NAME
```

### `docker dev open`

#### Usage

```
docker dev open DEV_ENV_NAME CONTAINER_REF [OPTIONS]
```

#### Options

| Name, shorthand | Description      |
| :-------------- | :--------------- |
| `--editor`,`-e` | 编辑器。 Editor. |

### `docker dev rm`

#### Usage

```
docker dev rm DEV_ENV_NAME
```

### `docker dev start`

#### Usage

```
docker dev start DEV_ENV_NAME
```

### `docker dev stop`

#### Usage

```
docker dev stop DEV_ENV_NAME
```

### `docker dev version`

#### Usage

```
docker dev version [OPTIONS]
```

#### Options

| Name, shorthand | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| `--format`,`-f` | 格式化输出。 Format the output.                              |
| `--short`,`-s`  | 仅显示 Docker Dev 版本号。 Shows only Docker Dev's version number. |
