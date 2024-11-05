+++
title = "docker stack config"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/stack/config/](https://docs.docker.com/reference/cli/docker/stack/config/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker stack config

| Description | Outputs the final config file, after doing merges and interpolations |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker stack config [OPTIONS]`                              |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 编排器。

## Description

Outputs the final Compose file, after doing the merges and interpolations of the input Compose files.

​	输出最终的 Compose 文件，执行合并和插值后。

## Options

| Option                 | Default | Description                                                  |
| ---------------------- | ------- | ------------------------------------------------------------ |
| `-c, --compose-file`   |         | Compose 文件路径，或使用 `-` 从标准输入读取 Path to a Compose file, or `-` to read from stdin |
| `--skip-interpolation` |         | 跳过插值，仅输出合并的配置 Skip interpolation and output only merged config |

## Examples

The following command outputs the result of the merge and interpolation of two Compose files.

​	以下命令输出两个 Compose 文件的合并和插值结果。



```console
$ docker stack config --compose-file docker-compose.yml --compose-file docker-compose.prod.yml
```

The Compose file can also be provided as standard input with `--compose-file -`:

​	Compose 文件也可以通过标准输入提供，使用 `--compose-file -`：



```console
$ cat docker-compose.yml | docker stack config --compose-file -
```

### 跳过插值 Skipping interpolation

In some cases, it might be useful to skip interpolation of environment variables. For example, when you want to pipe the output of this command back to `stack deploy`.

​	在某些情况下，跳过环境变量的插值可能有用，例如，当你想将该命令的输出通过管道传递回 `stack deploy`。

If you have a regex for a redirect route in an environment variable for your webserver you would use two `$` signs to prevent `stack deploy` from interpolating `${1}`.

​	如果你在环境变量中使用一个用于 web 服务器重定向路径的正则表达式，可以使用两个 `$` 符号以避免 `stack deploy` 插值 `${1}`。



```yaml
  service: webserver
  environment:
    REDIRECT_REGEX=http://host/redirect/$${1}
```

With interpolation, the `stack config` command will replace the environment variable in the Compose file with `REDIRECT_REGEX=http://host/redirect/${1}`, but then when piping it back to the `stack deploy` command it will be interpolated again and result in undefined behavior. That is why, when piping the output back to `stack deploy` one should always prefer the `--skip-interpolation` option.

​	通过插值，`stack config` 命令会将 Compose 文件中的环境变量替换为 `REDIRECT_REGEX=http://host/redirect/${1}`，但当将其传递给 `stack deploy` 命令时会再次插值，导致未定义的行为。因此，当将输出通过管道传递回 `stack deploy` 时，建议使用 `--skip-interpolation` 选项。

```console
$ docker stack config --compose-file web.yml --compose-file web.prod.yml --skip-interpolation | docker stack deploy --compose-file -
```

