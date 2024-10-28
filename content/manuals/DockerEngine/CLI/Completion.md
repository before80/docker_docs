+++
title = "自动完成"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/cli/completion/](https://docs.docker.com/engine/cli/completion/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Completion - 自动完成

You can generate a shell completion script for the Docker CLI using the `docker completion` command. The completion script gives you word completion for commands, flags, and Docker objects (such as container and volume names) when you hit `<Tab>` as you type into your terminal.

​	您可以使用 `docker completion` 命令为 Docker CLI 生成一个自动完成脚本。该脚本允许您在终端中输入时按 `<Tab>` 键自动补全命令、选项和 Docker 对象（如容器和卷的名称）。

You can generate completion scripts for the following shells:

​	您可以为以下 shell 生成完成脚本：

- [Bash](#bash)

- [Zsh](#zsh)
- [fish](#fish)

## Bash

To get Docker CLI completion with Bash, you first need to install the `bash-completion` package which contains a number of Bash functions for shell completion.

​	要在 Bash 中启用 Docker CLI 的自动完成，您首先需要安装 `bash-completion` 包，该包包含一些用于 shell 完成的 Bash 函数。

```bash
# Install using APT:
# 使用 APT 安装：
sudo apt install bash-completion

# Install using Homebrew (Bash version 4 or later):
# 使用 Homebrew 安装（适用于 Bash 4 或更高版本）：
brew install bash-completion@2
# Homebrew install for older versions of Bash:
# 使用 Homebrew 安装（适用于旧版本 Bash）：
brew install bash-completion

# With pacman:
# 使用 pacman 安装：
sudo pacman -S bash-completion
```

After installing `bash-completion`, source the script in your shell configuration file (in this example, `.bashrc`):

​	安装 `bash-completion` 后，将脚本添加到您的 shell 配置文件（此例中为 `.bashrc`）：



```bash
# On Linux:
# 在 Linux 上：
cat <<EOT >> ~/.bashrc
if [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
fi
EOT

# On macOS / with Homebrew:
# 在 macOS / 使用 Homebrew 时：
cat <<EOT >> ~/.bash_profile
[[ -r "$(brew --prefix)/etc/profile.d/bash_completion.sh" ]] && . "$(brew --prefix)/etc/profile.d/bash_completion.sh"
EOT
```

And reload your shell configuration:

​	重新加载 shell 配置：

```console
$ source ~/.bashrc
```

Now you can generate the Bash completion script using the `docker completion` command:

​	现在可以使用 `docker completion` 命令生成 Bash 完成脚本：

```console
$ mkdir -p ~/.local/share/bash-completion/completions
$ docker completion bash > ~/.local/share/bash-completion/completions/docker
```

## Zsh

The Zsh [completion system](http://zsh.sourceforge.net/Doc/Release/Completion-System.html) takes care of things as long as the completion can be sourced using `FPATH`.

​	Zsh 的[自动完成系统](http://zsh.sourceforge.net/Doc/Release/Completion-System.html)只要通过 `FPATH` 引用完成脚本即可自动处理。

If you use Oh My Zsh, you can install completions without modifying `~/.zshrc` by storing the completion script in the `~/.oh-my-zsh/completions` directory.

​	如果使用 Oh My Zsh，可以将完成脚本存储在 `~/.oh-my-zsh/completions` 目录中，而无需修改 `~/.zshrc`。



```console
$ mkdir -p ~/.oh-my-zsh/completions
$ docker completion zsh > ~/.oh-my-zsh/completions/_docker
```

If you're not using Oh My Zsh, store the completion script in a directory of your choice and add the directory to `FPATH` in your `.zshrc`.

​	如果不使用 Oh My Zsh，将完成脚本保存在任意目录，并在 `.zshrc` 中将该目录添加到 `FPATH`。



```console
$ mkdir -p ~/.docker/completions
$ docker completion zsh > ~/.docker/completions/_docker
```



```console
$ cat <<"EOT" >> ~/.zshrc
FPATH="$HOME/.docker/completions:$FPATH"
autoload -Uz compinit
compinit
EOT
```

## Fish

fish shell supports a [completion system](https://fishshell.com/docs/current/#tab-completion) natively. To activate completion for Docker commands, copy or symlink the completion script to your fish shell `completions/` directory:

​	fish shell 原生支持[自动完成系统](https://fishshell.com/docs/current/#tab-completion)。要启用 Docker 命令的自动完成，将完成脚本复制或链接到 fish shell 的 `completions/` 目录中。



```console
$ mkdir -p ~/.config/fish/completions
$ docker completion fish > ~/.config/fish/completions/docker.fish
```
