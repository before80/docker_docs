+++
title = "Dockerfile 参考"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/reference/dockerfile/](https://docs.docker.com/reference/dockerfile/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Dockerfile reference - Dockerfile 参考

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. This page describes the commands you can use in a Dockerfile.

​	Docker 可以通过读取 Dockerfile 中的指令自动构建镜像。Dockerfile 是一个包含用户可在命令行中调用的所有命令的文本文件，用于构建镜像。本页描述了 Dockerfile 中可用的指令。

## 概述 Overview

The Dockerfile supports the following instructions:

​	Dockerfile 支持以下指令：

| Instruction 指令                       | Description 描述                                             |
| :------------------------------------- | :----------------------------------------------------------- |
| [`ADD`](#add)                          | 添加本地或远程文件和目录。 Add local or remote files and directories. |
| [`ARG`](#arg)                          | 使用构建时变量。 Use build-time variables.                   |
| [`CMD`](#cmd)                          | 指定默认命令。Specify default commands.                      |
| [`COPY`](#copy)                        | 复制文件和目录。Copy files and directories.                  |
| [`ENTRYPOINT`](#entrypoint)            | 指定默认可执行文件。Specify default executable.              |
| [`ENV`](#env)                          | 设置环境变量。Set environment variables.                     |
| [`EXPOSE`](#expose)                    | 描述应用程序监听的端口。Describe which ports your application is listening on. |
| [`FROM`](#from)                        | 从基础镜像创建一个新的构建阶段。Create a new build stage from a base image. |
| [`HEALTHCHECK`](#healthcheck)          | 启动时检查容器的健康状况。Check a container's health on startup. |
| [`LABEL`](#label)                      | 为镜像添加元数据。Add metadata to an image.                  |
| [`MAINTAINER`](#maintainer-deprecated) | 指定镜像的作者。Specify the author of an image.              |
| [`ONBUILD`](#onbuild)                  | 指定在构建中使用镜像时的指令。Specify instructions for when the image is used in a build. |
| [`RUN`](#run)                          | 执行构建命令。Execute build commands.                        |
| [`SHELL`](#shell)                      | 设置镜像的默认 shell。Set the default shell of an image.     |
| [`STOPSIGNAL`](#stopsignal)            | 指定容器退出时的系统调用信号。Specify the system call signal for exiting a container. |
| [`USER`](#user)                        | 设置用户和组 ID。Set user and group ID.                      |
| [`VOLUME`](#volume)                    | 创建卷挂载。Create volume mounts.                            |
| [`WORKDIR`](#workdir)                  | 更改工作目录。Change working directory.                      |

## 格式 Format

Here is the format of the Dockerfile:

​	Dockerfile 的格式如下：

```dockerfile
# Comment
INSTRUCTION arguments
```

The instruction is not case-sensitive. However, convention is for them to be UPPERCASE to distinguish them from arguments more easily.

​	指令不区分大小写，通常习惯上使用大写以便更容易区分参数。

Docker runs instructions in a Dockerfile in order. A Dockerfile **must begin with a `FROM` instruction**. This may be after [parser directives](#parser-directives), [comments](#format), and globally scoped [ARGs](#arg). The `FROM` instruction specifies the [parent image](https://docs.docker.com/glossary/#parent-image) from which you are building. `FROM` may only be preceded by one or more `ARG` instructions, which declare arguments that are used in `FROM` lines in the Dockerfile.

​	Docker 会按顺序运行 Dockerfile 中的指令。Dockerfile **必须以 `FROM` 指令开头**。这可以在 [解析指令](#parser-directives)、[注释](#format) 和全局作用域的 [ARG](#arg) 之后。`FROM` 指令指定了要构建的[父镜像](https://docs.docker.com/glossary/#parent-image)。`FROM` 前仅能有一个或多个 `ARG` 指令，用于声明 Dockerfile 中 `FROM` 行中使用的参数。

BuildKit treats lines that begin with `#` as a comment, unless the line is a valid [parser directive](#parser-directives). A `#` marker anywhere else in a line is treated as an argument. This allows statements like:

​	BuildKit 将以 `#` 开头的行视为注释，除非该行是有效的[解析指令](#parser-directives)。行中其他位置的 `#` 被视为参数，允许如下语句：

```dockerfile
# Comment
RUN echo 'we are running some # of cool things'
```

Comment lines are removed before the Dockerfile instructions are executed. The comment in the following example is removed before the shell executes the `echo` command.

​	注释行在 Dockerfile 指令执行之前被移除。以下示例中的注释在 shell 执行 `echo` 命令之前被移除：

```dockerfile
RUN echo hello \
# comment
world
```

The following examples is equivalent.

​	以下示例是等效的：

```dockerfile
RUN echo hello \
world
```

Comments don't support line continuation characters.

​	注释不支持换行符。

> **Note**
>
> **Note on whitespace 关于空白**
>
> For backward compatibility, leading whitespace before comments (`#`) and instructions (such as `RUN`) are ignored, but discouraged. Leading whitespace is not preserved in these cases, and the following examples are therefore equivalent:
>
> ​	为了向后兼容，指令（如 `RUN`）和注释（如 `#`）前的前导空白会被忽略，但不推荐。前导空白不会保留，因此以下示例是等效的：
>
> ```dockerfile
>         # this is a comment-line
>     RUN echo hello
> RUN echo world
> ```
>
> 
>
> ```dockerfile
> # this is a comment-line
> RUN echo hello
> RUN echo world
> ```
>
> Whitespace in instruction arguments, however, isn't ignored. The following example prints `hello world` with leading whitespace as specified:
>
> ​	但是，指令参数中的空白不会被忽略。以下示例按指定的前导空白输出 `hello world`：
>
> ```dockerfile
> RUN echo "\
>      hello\
>      world"
> ```

## 解析指令 Parser directives

Parser directives are optional, and affect the way in which subsequent lines in a Dockerfile are handled. Parser directives don't add layers to the build, and don't show up as build steps. Parser directives are written as a special type of comment in the form `# directive=value`. A single directive may only be used once.

​	解析指令是可选的，用于影响 Dockerfile 中后续行的处理方式。解析指令不会增加构建层数，也不会显示为构建步骤。解析指令写成特殊的注释形式：`# directive=value`。每个解析指令只能使用一次。

The following parser directives are supported:

​	以下解析指令受支持：

- [`syntax`](#syntax)
- [`escape`](#escape)
- [`check`](#check) (since Dockerfile v1.8.0) （自 Dockerfile v1.8.0 起支持）

Once a comment, empty line or builder instruction has been processed, BuildKit no longer looks for parser directives. Instead it treats anything formatted as a parser directive as a comment and doesn't attempt to validate if it might be a parser directive. Therefore, all parser directives must be at the top of a Dockerfile.

​	一旦处理了注释、空行或构建器指令，BuildKit 不再查找解析指令，而是将任何格式为解析指令的内容视为注释，因此所有解析指令必须位于 Dockerfile 的顶部。

Parser directive keys, such as `syntax` or `check`, aren't case-sensitive, but they're lowercase by convention. Values for a directive are case-sensitive and must be written in the appropriate case for the directive. For example, `#check=skip=jsonargsrecommended` is invalid because the check name must use Pascal case, not lowercase. It's also conventional to include a blank line following any parser directives. Line continuation characters aren't supported in parser directives.

​	解析指令的键（如 `syntax` 或 `check`）不区分大小写，但惯例是使用小写。指令的值区分大小写，必须以正确的大小写书写。例如，`#check=skip=jsonargsrecommended` 是无效的，因为检查名称应使用 Pascal 大小写，而不是小写。同时，惯例上应在解析指令后留一个空行。解析指令中不支持换行符。

Due to these rules, the following examples are all invalid:

​	基于这些规则，以下示例均无效：

Invalid due to line continuation:

​	因换行符无效：

```dockerfile
# direc \
tive=value
```

Invalid due to appearing twice:

​	因多次出现无效：

```dockerfile
# directive=value1
# directive=value2

FROM ImageName
```

Treated as a comment because it appears after a builder instruction:

​	因出现在构建指令之后被视为注释：

```dockerfile
FROM ImageName
# directive=value
```

Treated as a comment because it appears after a comment that isn't a parser directive:

​	因出现在非解析指令注释后被视为注释：

```dockerfile
# About my dockerfile
# directive=value
FROM ImageName
```

The following `unknowndirective` is treated as a comment because it isn't recognized. The known `syntax` directive is treated as a comment because it appears after a comment that isn't a parser directive.

​	以下 `unknowndirective` 因无法识别被视为注释，而已知的 `syntax` 指令也因出现在非解析指令注释后被视为注释：

```dockerfile
# unknowndirective=value
# syntax=value
```

Non line-breaking whitespace is permitted in a parser directive. Hence, the following lines are all treated identically:

​	解析指令中允许非换行空白，因此以下各行的处理方式相同：

```dockerfile
#directive=value
# directive =value
#	directive= value
# directive = value
#	  dIrEcTiVe=value
```

The following parser directives are supported:

​	以下解析指令受支持：

- `syntax`
- `escape`

### syntax



Use the `syntax` parser directive to declare the Dockerfile syntax version to use for the build. If unspecified, BuildKit uses a bundled version of the Dockerfile frontend. Declaring a syntax version lets you automatically use the latest Dockerfile version without having to upgrade BuildKit or Docker Engine, or even use a custom Dockerfile implementation.

​	使用 `syntax` 解析指令声明构建时使用的 Dockerfile 语法版本。如果未指定，BuildKit 会使用捆绑的 Dockerfile 前端版本。声明语法版本可在无需升级 BuildKit 或 Docker 引擎的情况下自动使用最新的 Dockerfile 版本，甚至可以使用自定义 Dockerfile 实现。

Most users will want to set this parser directive to `docker/dockerfile:1`, which causes BuildKit to pull the latest stable version of the Dockerfile syntax before the build.

​	大多数用户会将此解析指令设置为 `docker/dockerfile:1`，这会使 BuildKit 在构建前拉取最新的稳定 Dockerfile 语法版本。

```dockerfile
# syntax=docker/dockerfile:1
```

For more information about how the parser directive works, see [Custom Dockerfile syntax](https://docs.docker.com/build/buildkit/dockerfile-frontend/).

​	关于解析指令的工作方式的更多信息，请参阅[自定义 Dockerfile 语法](https://docs.docker.com/build/buildkit/dockerfile-frontend/)。

### escape



```dockerfile
# escape=\
```

Or  或



```dockerfile
# escape=`
```

The `escape` directive sets the character used to escape characters in a Dockerfile. If not specified, the default escape character is `\`.

​	`escape` 指令设置 Dockerfile 中用于转义字符的字符。如果未指定，默认转义字符为 `\`。

The escape character is used both to escape characters in a line, and to escape a newline. This allows a Dockerfile instruction to span multiple lines. Note that regardless of whether the `escape` parser directive is included in a Dockerfile, escaping is not performed in a `RUN` command, except at the end of a line.

​	转义字符用于转义行中的字符和换行符，这允许 Dockerfile 指令跨多行。无论 Dockerfile 中是否包含 `escape` 解析指令，`RUN` 命令中仅在行尾执行转义。

Setting the escape character to `` ` `` is especially useful on `Windows`, where `\` is the directory path separator. `` ` `` is consistent with [Windows PowerShell](https://technet.microsoft.com/en-us/library/hh847755.aspx).

​	将转义字符设置为 ``` 在 `Windows` 系统上特别有用，因为 `\` 是目录路径分隔符。``` 与 [Windows PowerShell](https://technet.microsoft.com/en-us/library/hh847755.aspx) 保持一致。

Consider the following example which would fail in a non-obvious way on Windows. The second `\` at the end of the second line would be interpreted as an escape for the newline, instead of a target of the escape from the first `\`. Similarly, the `\` at the end of the third line would, assuming it was actually handled as an instruction, cause it be treated as a line continuation. The result of this Dockerfile is that second and third lines are considered a single instruction:

​	考虑以下示例，它在 Windows 上可能会以一种非直观的方式失败。第二行末尾的第二个 `\` 会被解释为换行的转义，而不是第一个 `\` 的转义目标。同样地，假设第三行末尾的 `\` 被实际作为指令处理，它会被视为行延续。该 Dockerfile 的结果是将第二行和第三行视为一个指令：

```dockerfile
FROM microsoft/nanoserver
COPY testfile.txt c:\\
RUN dir c:\
```

Results in:

结果如下：

```console
PS E:\myproject> docker build -t cmd .

Sending build context to Docker daemon 3.072 kB
Step 1/2 : FROM microsoft/nanoserver
 ---> 22738ff49c6d
Step 2/2 : COPY testfile.txt c:\RUN dir c:
GetFileAttributesEx c:RUN: The system cannot find the file specified.
PS E:\myproject>
```

One solution to the above would be to use `/` as the target of both the `COPY` instruction, and `dir`. However, this syntax is, at best, confusing as it is not natural for paths on Windows, and at worst, error prone as not all commands on Windows support `/` as the path separator.

​	一种解决方案是使用 `/` 作为 `COPY` 指令和 `dir` 的目标路径。然而，这种语法在 Windows 上不直观且容易出错，因为并非所有 Windows 命令都支持 `/` 作为路径分隔符。

By adding the `escape` parser directive, the following Dockerfile succeeds as expected with the use of natural platform semantics for file paths on Windows:

​	通过添加 `escape` 解析指令，以下 Dockerfile 在 Windows 上按预期成功运行，使用自然平台语义来处理文件路径：

```dockerfile
# escape=`

FROM microsoft/nanoserver
COPY testfile.txt c:\
RUN dir c:\
```

Results in:

结果如下：

```console
PS E:\myproject> docker build -t succeeds --no-cache=true .

Sending build context to Docker daemon 3.072 kB
Step 1/3 : FROM microsoft/nanoserver
 ---> 22738ff49c6d
Step 2/3 : COPY testfile.txt c:\
 ---> 96655de338de
Removing intermediate container 4db9acbb1682
Step 3/3 : RUN dir c:\
 ---> Running in a2c157f842f5
 Volume in drive C has no label.
 Volume Serial Number is 7E6D-E0F7

 Directory of c:\

10/05/2016  05:04 PM             1,894 License.txt
10/05/2016  02:22 PM    DIR          Program Files
10/05/2016  02:14 PM    DIR          Program Files (x86)
10/28/2016  11:18 AM                62 testfile.txt
10/28/2016  11:20 AM    DIR          Users
10/28/2016  11:20 AM    DIR          Windows
           2 File(s)          1,956 bytes
           4 Dir(s)  21,259,096,064 bytes free
 ---> 01c7f3bef04f
Removing intermediate container a2c157f842f5
Successfully built 01c7f3bef04f
PS E:\myproject>
```

### check



```dockerfile
# check=skip=<checks|all>
# check=error=<boolean>
```

The `check` directive is used to configure how [build checks]({{< ref "/manuals/DockerBuild/BuildchecksNew" >}}) are evaluated. By default, all checks are run, and failures are treated as warnings.

​	`check` 指令用于配置[构建检查]({{< ref "/manuals/DockerBuild/BuildchecksNew" >}})的评估方式。默认情况下，运行所有检查，失败将被视为警告。

You can disable specific checks using `#check=skip=<check-name>`. To specify multiple checks to skip, separate them with a comma:

​	您可以使用 `#check=skip=<check-name>` 禁用特定检查。要指定多个要跳过的检查，请用逗号分隔：



```dockerfile
# check=skip=JSONArgsRecommended,StageNameCasing
```

To disable all checks, use `#check=skip=all`.

​	要禁用所有检查，请使用 `#check=skip=all`。

By default, builds with failing build checks exit with a zero status code despite warnings. To make the build fail on warnings, set `#check=error=true`.

​	默认情况下，包含失败检查的构建即使有警告也会以零状态码退出。要使构建在警告时失败，请设置 `#check=error=true`。



```dockerfile
# check=error=true
```

To combine both the `skip` and `error` options, use a semi-colon to separate them:

​	要同时使用 `skip` 和 `error` 选项，用分号分隔它们：



```dockerfile
# check=skip=JSONArgsRecommended;error=true
```

To see all available checks, see the [build checks reference]({{< ref "/reference/Buildchecks" >}}). Note that the checks available depend on the Dockerfile syntax version. To make sure you're getting the most up-to-date checks, use the [`syntax`](#syntax) directive to specify the Dockerfile syntax version to the latest stable version.

​	查看所有可用的检查，请参阅[构建检查参考]({{< ref "/reference/Buildchecks" >}})。注意，可用的检查取决于 Dockerfile 的语法版本。为确保获得最新的检查，请使用 [`syntax`](#syntax) 指令将 Dockerfile 语法版本指定为最新稳定版本。

## 环境变量替换 Environment replacement

Environment variables (declared with [the `ENV` statement](#env)) can also be used in certain instructions as variables to be interpreted by the Dockerfile. Escapes are also handled for including variable-like syntax into a statement literally.

​	环境变量（通过 [`ENV` 语句](#env) 声明）也可以在特定指令中用作 Dockerfile 的变量。转义也适用于在指令中包括变量语法字面量。

Environment variables are notated in the Dockerfile either with `$variable_name` or `${variable_name}`. They are treated equivalently and the brace syntax is typically used to address issues with variable names with no whitespace, like `${foo}_bar`.

​	在 Dockerfile 中，环境变量使用 `$variable_name` 或 `${variable_name}` 表示。两者等效，且花括号语法通常用于避免无空格变量名的解析问题，如 `${foo}_bar`。

The `${variable_name}` syntax also supports a few of the standard `bash` modifiers as specified below:

​	`${variable_name}` 语法还支持一些标准的 `bash` 修饰符，如下所示：

- `${variable:-word}` indicates that if `variable` is set then the result will be that value. If `variable` is not set then `word` will be the result.
  - `${variable:-word}` 表示如果 `variable` 已设置，则结果为该值；如果 `variable` 未设置，则结果为 `word`。

- `${variable:+word}` indicates that if `variable` is set then `word` will be the result, otherwise the result is the empty string.
  - `${variable:+word}` 表示如果 `variable` 已设置，则结果为 `word`，否则结果为空字符串。


The following variable replacements are supported in a pre-release version of Dockerfile syntax, when using the `# syntax=docker/dockerfile-upstream:master` syntax directive in your Dockerfile:

​	当使用 Dockerfile 的预发布版本语法 `# syntax=docker/dockerfile-upstream:master` 时，支持以下变量替换：

- `${variable#pattern}` removes the shortest match of `pattern` from `variable`, seeking from the start of the string. `${variable#pattern}` 从 `variable` 开始去除 `pattern` 的最短匹配。

  

  ```bash
  str=foobarbaz echo ${str#f*b}     # arbaz
  ```

- `${variable##pattern}` removes the longest match of `pattern` from `variable`, seeking from the start of the string. `${variable##pattern}` 从 `variable` 开始去除 `pattern` 的最长匹配。

  

  ```bash
  str=foobarbaz echo ${str##f*b}    # az
  ```

- `${variable%pattern}` removes the shortest match of `pattern` from `variable`, seeking backwards from the end of the string. `${variable%pattern}` 从 `variable` 末尾开始去除 `pattern` 的最短匹配。

  

  ```bash
  string=foobarbaz echo ${string%b*}    # foobar
  ```

- `${variable%%pattern}` removes the longest match of `pattern` from `variable`, seeking backwards from the end of the string. `${variable%%pattern}` 从 `variable` 末尾开始去除 `pattern` 的最长匹配。

  

  ```bash
  string=foobarbaz echo ${string%%b*}   # foo
  ```

- `${variable/pattern/replacement}` replace the first occurrence of `pattern` in `variable` with `replacement` `${variable/pattern/replacement}` 将 `variable` 中的第一个 `pattern` 替换为 `replacement`。

  

  ```bash
  string=foobarbaz echo ${string/ba/fo}  # fooforbaz
  ```

- `${variable//pattern/replacement}` replaces all occurrences of `pattern` in `variable` with `replacement` `${variable//pattern/replacement}` 将 `variable` 中所有的 `pattern` 替换为 `replacement`。

  

  ```bash
  string=foobarbaz echo ${string//ba/fo}  # fooforfoz
  ```

In all cases, `word` can be any string, including additional environment variables.

​	在所有情况下，`word` 可以是任意字符串，包括其他环境变量。

`pattern` is a glob pattern where `?` matches any single character and `*` any number of characters (including zero). To match literal `?` and `*`, use a backslash escape: `\?` and `\*`.

​	`pattern` 是一个通配符模式，其中 `?` 匹配任意单个字符，`*` 匹配任意数量的字符（包括零个）。要匹配字面上的 `?` 和 `*`，使用反斜杠转义：`\?` 和 `\*`。

You can escape whole variable names by adding a `\` before the variable: `\$foo` or `\${foo}`, for example, will translate to `$foo` and `${foo}` literals respectively.

​	您可以通过在变量名前添加 `\` 来转义整个变量。例如，`\$foo` 或 `\${foo}` 将分别解析为字面上的 `$foo` 和 `${foo}`。

Example (parsed representation is displayed after the `#`):

​	示例（解析后的表示在 `#` 后显示）：



```dockerfile
FROM busybox
ENV FOO=/bar
WORKDIR ${FOO}   # WORKDIR /bar
ADD . $FOO       # ADD . /bar
COPY \$FOO /quux # COPY $FOO /quux
```

Environment variables are supported by the following list of instructions in the Dockerfile:

​	Dockerfile 中以下指令支持环境变量：

- `ADD`

- `COPY`
- `ENV`
- `EXPOSE`
- `FROM`
- `LABEL`
- `STOPSIGNAL`
- `USER`
- `VOLUME`
- `WORKDIR`
- `ONBUILD` (when combined with one of the supported instructions above) （当与以上支持的指令之一结合使用时）

You can also use environment variables with `RUN`, `CMD`, and `ENTRYPOINT` instructions, but in those cases the variable substitution is handled by the command shell, not the builder. Note that instructions using the exec form don't invoke a command shell automatically. See [Variable substitution](#variable-substitution).

​	在 `RUN`、`CMD` 和 `ENTRYPOINT` 指令中也可以使用环境变量，但在这些情况下，变量替换由命令行解释器处理，而不是构建器。请注意，使用 exec 形式的指令不会自动调用命令行解释器。详情请参阅[变量替换](#variable-substitution)。

Environment variable substitution use the same value for each variable throughout the entire instruction. Changing the value of a variable only takes effect in subsequent instructions. Consider the following example:

​	在整个指令中，环境变量替换会使用每个变量的相同值。更改变量的值仅会影响后续的指令。考虑以下示例：



```dockerfile
ENV abc=hello
ENV abc=bye def=$abc
ENV ghi=$abc
```

- The value of `def` becomes `hello` -  `def` 的值为 `hello`
- The value of `ghi` becomes `bye` - `ghi` 的值为 `bye`

## .dockerignore 文件 .dockerignore file

You can use `.dockerignore` file to exclude files and directories from the build context. For more information, see [.dockerignore file](https://docs.docker.com/build/building/context/#dockerignore-files).

​	您可以使用 `.dockerignore` 文件来排除构建上下文中的文件和目录。详情请参阅[.dockerignore 文件](https://docs.docker.com/build/building/context/#dockerignore-files)。

## Shell 和 exec 形式 Shell and exec form

The `RUN`, `CMD`, and `ENTRYPOINT` instructions all have two possible forms:

​	`RUN`、`CMD` 和 `ENTRYPOINT` 指令都可以有以下两种形式：

- `INSTRUCTION ["executable","param1","param2"]` (exec form) （exec 形式）
- `INSTRUCTION command param1 param2` (shell form) （shell 形式）

The exec form makes it possible to avoid shell string munging, and to invoke commands using a specific command shell, or any other executable. It uses a JSON array syntax, where each element in the array is a command, flag, or argument.

​	exec 形式可以避免 shell 字符串的复杂性，并使用特定的命令行解释器或其他可执行文件。它使用 JSON 数组语法，数组中的每个元素都是命令、标志或参数。

The shell form is more relaxed, and emphasizes ease of use, flexibility, and readability. The shell form automatically uses a command shell, whereas the exec form does not.

​	shell 形式更灵活，强调易用性、灵活性和可读性。shell 形式自动使用命令行解释器，而 exec 形式不会。

### Exec 形式 Exec form

The exec form is parsed as a JSON array, which means that you must use double-quotes (") around words, not single-quotes (').

​	exec 形式解析为 JSON 数组，因此必须使用双引号 `"` 包裹单词，而不是单引号 `'`。



```dockerfile
ENTRYPOINT ["/bin/bash", "-c", "echo hello"]
```

The exec form is best used to specify an `ENTRYPOINT` instruction, combined with `CMD` for setting default arguments that can be overridden at runtime. For more information, see [ENTRYPOINT](#entrypoint).

​	exec 形式最适合用于指定 `ENTRYPOINT` 指令，并结合 `CMD` 设置可以在运行时覆盖的默认参数。详情请参阅 [ENTRYPOINT](#entrypoint)。

#### 变量替换 Variable substitution

Using the exec form doesn't automatically invoke a command shell. This means that normal shell processing, such as variable substitution, doesn't happen. For example, `RUN [ "echo", "$HOME" ]` won't handle variable substitution for `$HOME`.

​	使用 exec 形式不会自动调用命令行解释器，这意味着不会进行正常的 shell 处理（例如变量替换）。例如，`RUN [ "echo", "$HOME" ]` 不会处理 `$HOME` 的变量替换。

If you want shell processing then either use the shell form or execute a shell directly with the exec form, for example: `RUN [ "sh", "-c", "echo $HOME" ]`. When using the exec form and executing a shell directly, as in the case for the shell form, it's the shell that's doing the environment variable substitution, not the builder.

​	如果需要 shell 处理，可以使用 shell 形式或直接在 exec 形式中执行 shell，例如：`RUN [ "sh", "-c", "echo $HOME" ]`。在 exec 形式中直接执行 shell 时，环境变量替换由 shell 进行，而不是由构建器处理。

#### 反斜杠 Backslashes

In exec form, you must escape backslashes. This is particularly relevant on Windows where the backslash is the path separator. The following line would otherwise be treated as shell form due to not being valid JSON, and fail in an unexpected way:

​	在 exec 形式中必须转义反斜杠。这在 Windows 上尤为重要，因为反斜杠是路径分隔符。以下代码由于不是有效的 JSON，因此会被视为 shell 形式，并以意外方式失败：



```dockerfile
RUN ["c:\windows\system32\tasklist.exe"]
```

The correct syntax for this example is:

​	正确的语法应为：

```dockerfile
RUN ["c:\\windows\\system32\\tasklist.exe"]
```

### Shell 形式 Shell form

Unlike the exec form, instructions using the shell form always use a command shell. The shell form doesn't use the JSON array format, instead it's a regular string. The shell form string lets you escape newlines using the [escape character](#escape) (backslash by default) to continue a single instruction onto the next line. This makes it easier to use with longer commands, because it lets you split them up into multiple lines. For example, consider these two lines:

​	与 exec 形式不同，使用 shell 形式的指令始终使用命令行解释器。shell 形式不使用 JSON 数组格式，而是常规字符串。shell 形式的字符串允许使用[转义字符](#escape)（默认为反斜杠）来将单个指令延续到下一行。这使得使用较长命令时更容易，因为可以将它们拆分成多行。例如，以下两行代码：



```dockerfile
RUN source $HOME/.bashrc && \
echo $HOME
```

They're equivalent to the following line:

​	等效于以下单行代码：

```dockerfile
RUN source $HOME/.bashrc && echo $HOME
```

You can also use heredocs with the shell form to break up supported commands.

​	您还可以使用 heredoc 与 shell 形式来分解支持的命令：

```dockerfile
RUN <<EOF
source $HOME/.bashrc && \
echo $HOME
EOF
```

For more information about heredocs, see [Here-documents](#here-documents).

​	有关 heredoc 的更多信息，请参阅 [Here-documents](#here-documents)。

### 使用不同的 shell - Use a different shell

You can change the default shell using the `SHELL` command. For example:

​	您可以使用 `SHELL` 指令更改默认 shell。例如：

```dockerfile
SHELL ["/bin/bash", "-c"]
RUN echo hello
```

For more information, see [SHELL](#shell).

​	详情请参阅 [SHELL](#shell)。

## FROM



```dockerfile
FROM [--platform=<platform>] <image> [AS <name>]
```

Or 或



```dockerfile
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
```

Or 或



```dockerfile
FROM [--platform=<platform>] <image>[@<digest>] [AS <name>]
```

The `FROM` instruction initializes a new build stage and sets the [base image](https://docs.docker.com/glossary/#base-image) for subsequent instructions. As such, a valid Dockerfile must start with a `FROM` instruction. The image can be any valid image.

​	`FROM` 指令初始化新的构建阶段，并为后续指令设置[基础镜像](https://docs.docker.com/glossary/#base-image)。因此，一个有效的 Dockerfile 必须以 `FROM` 指令开始。镜像可以是任何有效的镜像。

- `ARG` is the only instruction that may precede `FROM` in the Dockerfile. See [Understand how ARG and FROM interact](#understand-how-arg-and-from-interact).
  - `ARG` 是 Dockerfile 中唯一可以在 `FROM` 之前出现的指令。详情请参阅[理解 ARG 和 FROM 的交互](#understand-how-arg-and-from-interact)。

- `FROM` can appear multiple times within a single Dockerfile to create multiple images or use one build stage as a dependency for another. Simply make a note of the last image ID output by the commit before each new `FROM` instruction. Each `FROM` instruction clears any state created by previous instructions.
  - 在同一个 Dockerfile 中，`FROM` 可以出现多次以创建多个镜像，或将一个构建阶段作为另一个构建阶段的依赖项。只需在每个新 `FROM` 指令前记录下上一个提交的镜像 ID。每个 `FROM` 指令都会清除前面指令创建的状态。

- Optionally a name can be given to a new build stage by adding `AS name` to the `FROM` instruction. The name can be used in subsequent `FROM <name>`, [`COPY --from=`](#copy---from), and [`RUN --mount=type=bind,from=`](#run---mounttypebind) instructions to refer to the image built in this stage.
  - 可以通过在 `FROM` 指令后添加 `AS name` 来为新的构建阶段指定名称。该名称可在后续指令中用于引用该构建阶段中的镜像。

- The `tag` or `digest` values are optional. If you omit either of them, the builder assumes a `latest` tag by default. The builder returns an error if it can't find the `tag` value.
  - `tag` 或 `digest` 值是可选的。如果省略其中一个，构建器默认使用 `latest` 标签。如果找不到 `tag` 值，构建器会返回错误。


The optional `--platform` flag can be used to specify the platform of the image in case `FROM` references a multi-platform image. For example, `linux/amd64`, `linux/arm64`, or `windows/amd64`. By default, the target platform of the build request is used. Global build arguments can be used in the value of this flag, for example [automatic platform ARGs](#automatic-platform-args-in-the-global-scope) allow you to force a stage to native build platform (`--platform=$BUILDPLATFORM`), and use it to cross-compile to the target platform inside the stage.

​	可选的 `--platform` 标志可用于指定镜像的平台，以便 `FROM` 引用多平台镜像。例如，`linux/amd64`、`linux/arm64` 或 `windows/amd64`。默认情况下使用构建请求的目标平台。全局构建参数可以用于此标志的值，例如[自动平台参数](#automatic-platform-args-in-the-global-scope)允许强制阶段使用原生构建平台（`--platform=$BUILDPLATFORM`），并在阶段内对目标平台进行交叉编译。

### 理解 ARG 和 FROM 的交互 Understand how ARG and FROM interact

`FROM` instructions support variables that are declared by any `ARG` instructions that occur before the first `FROM`.

​	`FROM` 指令支持在第一个 `FROM` 之前出现的任何 `ARG` 指令声明的变量。

```dockerfile
ARG  CODE_VERSION=latest
FROM base:${CODE_VERSION}
CMD  /code/run-app

FROM extras:${CODE_VERSION}
CMD  /code/run-extras
```

An `ARG` declared before a `FROM` is outside of a build stage, so it can't be used in any instruction after a `FROM`. To use the default value of an `ARG` declared before the first `FROM` use an `ARG` instruction without a value inside of a build stage:

​	在 `FROM` 之前声明的 `ARG` 位于构建阶段之外，因此在 `FROM` 之后的任何指令中无法使用。要使用在第一个 `FROM` 之前声明的 `ARG` 的默认值，请在构建阶段内使用不带值的 `ARG` 指令：

```dockerfile
ARG VERSION=latest
FROM busybox:$VERSION
ARG VERSION
RUN echo $VERSION > image_version
```

## RUN

The `RUN` instruction will execute any commands to create a new layer on top of the current image. The added layer is used in the next step in the Dockerfile. `RUN` has two forms:

​	`RUN` 指令将执行任何命令以在当前镜像上创建新层。添加的新层将在 Dockerfile 的下一步中使用。`RUN` 有两种形式：

```dockerfile
# Shell form:
RUN [OPTIONS] <command> ...
# Exec form:
RUN [OPTIONS] [ "<command>", ... ]
```

For more information about the differences between these two forms, see [shell or exec forms](#shell-and-exec-form).

​	有关这两种形式的差异的更多信息，请参阅[shell 或 exec 形式](#shell-and-exec-form)。

The shell form is most commonly used, and lets you break up longer instructions into multiple lines, either using newline [escapes](#escape), or with [heredocs](#here-documents):

​	shell 形式最常用，可以使用换行[转义](#escape)或[heredocs](#here-documents)将较长的指令拆分为多行：

```dockerfile
RUN <<EOF
apt-get update
apt-get install -y curl
EOF
```

The available `[OPTIONS]` for the `RUN` instruction are:

​	`RUN` 指令的可用 `[OPTIONS]` 如下：

| Option 选项                     | Minimum Dockerfile version Dockerfile 最低版本 |
| ------------------------------- | ---------------------------------------------- |
| [`--mount`](#run---mount)       | 1.2                                            |
| [`--network`](#run---network)   | 1.3                                            |
| [`--security`](#run---security) | 1.1.2-labs                                     |

### RUN 指令的缓存失效 Cache invalidation for RUN instructions

The cache for `RUN` instructions isn't invalidated automatically during the next build. The cache for an instruction like `RUN apt-get dist-upgrade -y` will be reused during the next build. The cache for `RUN` instructions can be invalidated by using the `--no-cache` flag, for example `docker build --no-cache`.

​	`RUN` 指令的缓存在下一次构建时不会自动失效。像 `RUN apt-get dist-upgrade -y` 这样的指令在下一次构建时会重用缓存。可以通过使用 `--no-cache` 标志来使 `RUN` 指令的缓存失效，例如 `docker build --no-cache`。

See the [Dockerfile Best Practices guide](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/) for more information.

​	有关更多信息，请参阅[Dockerfile 最佳实践指南](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)。

The cache for `RUN` instructions can be invalidated by [`ADD`](#add) and [`COPY`](#copy) instructions.

​	`RUN` 指令的缓存也可以通过 [`ADD`](#add) 和 [`COPY`](#copy) 指令使其失效。

### RUN `--mount`



```dockerfile
RUN --mount=[type=TYPE][,option=<value>[,option=<value>]...]
```

`RUN --mount` allows you to create filesystem mounts that the build can access. This can be used to:

​	`RUN --mount` 允许创建构建可以访问的文件系统挂载。这可以用于：

- Create bind mount to the host filesystem or other build stages 创建到主机文件系统或其他构建阶段的绑定挂载

- Access build secrets or ssh-agent sockets 访问构建机密或 ssh-agent 套接字
- Use a persistent package management cache to speed up your build 使用持久化包管理缓存以加速构建

The supported mount types are:

​	支持的挂载类型有：

| Type 类型                                | Description 描述                                             |
| ---------------------------------------- | ------------------------------------------------------------ |
| [`bind`](#run---mounttypebind) (default) | Bind-mount context directories (read-only). 绑定挂载上下文目录（只读）。 |
| [`cache`](#run---mounttypecache)         | Mount a temporary directory to cache directories for compilers and package managers. 挂载一个临时目录以缓存编译器和包管理器的目录。 |
| [`tmpfs`](#run---mounttypetmpfs)         | Mount a `tmpfs` in the build container. 在构建容器中挂载 `tmpfs`。 |
| [`secret`](#run---mounttypesecret)       | Allow the build container to access secure files such as private keys without baking them into the image or build cache. 允许构建容器访问安全文件（如私钥），而无需将它们写入镜像或构建缓存。 |
| [`ssh`](#run---mounttypessh)             | Allow the build container to access SSH keys via SSH agents, with support for passphrases. 允许构建容器通过 SSH 代理访问 SSH 密钥，并支持密码短语。 |

### RUN `--mount=type=bind`

This mount type allows binding files or directories to the build container. A bind mount is read-only by default.

​	此挂载类型允许将文件或目录绑定到构建容器中。绑定挂载默认是只读的。

| Option 选项                              | Description 描述                                             |
| ---------------------------------------- | ------------------------------------------------------------ |
| `target`, `dst`, `destination`[1](#fn:1) | Mount path. 挂载路径。                                       |
| `source`                                 | Source path in the `from`. Defaults to the root of the `from`. `from` 的根路径。默认为 `from` 的根目录。 |
| `from`                                   | Build stage, context, or image name for the root of the source. Defaults to the build context. 用作源根的构建阶段、上下文或镜像名称。默认为构建上下文。 |
| `rw`,`readwrite`                         | Allow writes on the mount. Written data will be discarded. 允许在挂载上进行写操作。写入的数据将被丢弃。 |

### RUN `--mount=type=cache`

This mount type allows the build container to cache directories for compilers and package managers.

​	此挂载类型允许构建容器缓存编译器和包管理器的目录。

| Option 选项                              | Description 描述                                             |
| ---------------------------------------- | ------------------------------------------------------------ |
| `id`                                     | Optional ID to identify separate/different caches. Defaults to value of `target`. 用于标识不同缓存的可选 ID。默认为 `target` 的值。 |
| `target`, `dst`, `destination`[1](#fn:1) | Mount path. 挂载路径。                                       |
| `ro`,`readonly`                          | Read-only if set. 如果设置为只读。                           |
| `sharing`                                | One of `shared`, `private`, or `locked`. Defaults to `shared`. A `shared` cache mount can be used concurrently by multiple writers. `private` creates a new mount if there are multiple writers. `locked` pauses the second writer until the first one releases the mount. 共享模式之一：`shared`、`private` 或 `locked`。默认为 `shared`。`shared` 缓存挂载可被多个写入者同时使用。`private` 为多个写入者创建新的挂载。`locked` 会暂停第二个写入者，直到第一个写入者释放挂载。 |
| `from`                                   | Build stage, context, or image name to use as a base of the cache mount. Defaults to empty directory. 用作缓存挂载基础的构建阶段、上下文或镜像名称。默认为空目录。 |
| `source`                                 | Subpath in the `from` to mount. Defaults to the root of the `from`. `from` 中要挂载的子路径。默认为 `from` 的根目录。 |
| `mode`                                   | File mode for new cache directory in octal. Default `0755`. 新缓存目录的文件模式（八进制）。默认为 `0755`。 |
| `uid`                                    | User ID for new cache directory. Default `0`. 新缓存目录的用户 ID。默认为 `0`。 |
| `gid`                                    | Group ID for new cache directory. Default `0`. 新缓存目录的组 ID。默认为 `0`。 |

Contents of the cache directories persists between builder invocations without invalidating the instruction cache. Cache mounts should only be used for better performance. Your build should work with any contents of the cache directory as another build may overwrite the files or GC may clean it if more storage space is needed.

​	缓存目录的内容在构建器调用之间保持而不会使指令缓存失效。缓存挂载应仅用于提高性能。您的构建应在缓存目录的任意内容下工作，因为另一个构建可能会覆盖文件，或者在需要更多存储空间时垃圾回收系统可能会清理它。

#### 示例：缓存 Go 包 Example: cache Go packages



```dockerfile
# syntax=docker/dockerfile:1
FROM golang
RUN --mount=type=cache,target=/root/.cache/go-build \
  go build ...
```

#### 示例：缓存 apt 包 Example: cache apt packages



```dockerfile
# syntax=docker/dockerfile:1
FROM ubuntu
RUN rm -f /etc/apt/apt.conf.d/docker-clean; echo 'Binary::apt::APT::Keep-Downloaded-Packages "true";' > /etc/apt/apt.conf.d/keep-cache
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
  --mount=type=cache,target=/var/lib/apt,sharing=locked \
  apt update && apt-get --no-install-recommends install -y gcc
```

Apt needs exclusive access to its data, so the caches use the option `sharing=locked`, which will make sure multiple parallel builds using the same cache mount will wait for each other and not access the same cache files at the same time. You could also use `sharing=private` if you prefer to have each build create another cache directory in this case.

​	apt 需要独占访问其数据，因此缓存使用 `sharing=locked` 选项，这将确保多个使用相同缓存挂载的并行构建相互等待，而不会同时访问相同的缓存文件。如果希望每个构建创建不同的缓存目录，也可以使用 `sharing=private`。

### RUN `--mount=type=tmpfs`

This mount type allows mounting `tmpfs` in the build container.

​	此挂载类型允许在构建容器中挂载 `tmpfs`。

| Option 选项                              | Description 描述                                             |
| ---------------------------------------- | ------------------------------------------------------------ |
| `target`, `dst`, `destination`[1](#fn:1) | Mount path. 挂载路径。                                       |
| `size`                                   | Specify an upper limit on the size of the filesystem. 指定文件系统大小的上限。 |

### RUN `--mount=type=secret`

This mount type allows the build container to access secret values, such as tokens or private keys, without baking them into the image.

​	此挂载类型允许构建容器访问机密值，如令牌或私钥，而无需将它们嵌入到镜像中。

By default, the secret is mounted as a file. You can also mount the secret as an environment variable by setting the `env` option.

​	默认情况下，机密会作为文件挂载。也可以通过设置 `env` 选项将机密挂载为环境变量。

| Option 选项                    | Description 描述                                             |
| ------------------------------ | ------------------------------------------------------------ |
| `id`                           | ID of the secret. Defaults to basename of the target path. 机密的 ID。默认为目标路径的基本名称。 |
| `target`, `dst`, `destination` | Mount the secret to the specified path. Defaults to `/run/secrets/` + `id` if unset and if `env` is also unset. 将机密挂载到指定路径。如果未设置且 `env` 也未设置，默认路径为 `/run/secrets/` + `id`。 |
| `env`                          | Mount the secret to an environment variable instead of a file, or both. (since Dockerfile v1.10.0) 将机密挂载为环境变量而不是文件，或者两者都挂载。（自 Dockerfile v1.10.0 起支持） |
| `required`                     | If set to `true`, the instruction errors out when the secret is unavailable. Defaults to `false`. 如果设置为 `true`，当机密不可用时指令会报错。默认为 `false`。 |
| `mode`                         | File mode for secret file in octal. Default `0400`. 机密文件的八进制模式。默认值为 `0400`。 |
| `uid`                          | User ID for secret file. Default `0`. 机密文件的用户 ID。默认值为 `0`。 |
| `gid`                          | Group ID for secret file. Default `0`. 机密文件的组 ID。默认值为 `0`。 |

#### 示例：访问 S3 Example: access to S3



```dockerfile
# syntax=docker/dockerfile:1
FROM python:3
RUN pip install awscli
RUN --mount=type=secret,id=aws,target=/root/.aws/credentials \
  aws s3 cp s3://... ...
```



```console
$ docker buildx build --secret id=aws,src=$HOME/.aws/credentials .
```

#### 示例：挂载为环境变量 Example: Mount as environment variable

The following example takes the secret `API_KEY` and mounts it as an environment variable with the same name.

​	以下示例将机密 `API_KEY` 挂载为同名的环境变量。

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN --mount=type=secret,id=API_KEY,env=API_KEY \
    some-command --token-from-env API_KEY
```

Assuming that the `API_KEY` environment variable is set in the build environment, you can build this with the following command:

​	假设构建环境中设置了 `API_KEY` 环境变量，可以使用以下命令构建：

```console
$ docker buildx build --secret id=API_KEY .
```

### RUN `--mount=type=ssh`

This mount type allows the build container to access SSH keys via SSH agents, with support for passphrases.

​	此挂载类型允许构建容器通过 SSH 代理访问 SSH 密钥，并支持密码短语。

| Option 选项                    | Description 描述                                             |
| ------------------------------ | ------------------------------------------------------------ |
| `id`                           | ID of SSH agent socket or key. Defaults to "default". SSH 代理套接字或密钥的 ID。默认为 "default"。 |
| `target`, `dst`, `destination` | SSH agent socket path. Defaults to `/run/buildkit/ssh_agent.${N}`. SSH 代理套接字路径。默认为 `/run/buildkit/ssh_agent.${N}`。 |
| `required`                     | If set to `true`, the instruction errors out when the key is unavailable. Defaults to `false`. 如果设置为 `true`，当密钥不可用时指令会报错。默认为 `false`。 |
| `mode`                         | File mode for socket in octal. Default `0600`. 套接字的八进制文件模式。默认值为 `0600`。 |
| `uid`                          | User ID for socket. Default `0`. 套接字的用户 ID。默认值为 `0`。 |
| `gid`                          | Group ID for socket. Default `0`. 套接字的组 ID。默认值为 `0`。 |

#### 示例：访问 GitLab - Example: access to GitLab



```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN apk add --no-cache openssh-client
RUN mkdir -p -m 0700 ~/.ssh && ssh-keyscan gitlab.com >> ~/.ssh/known_hosts
RUN --mount=type=ssh \
  ssh -q -T git@gitlab.com 2>&1 | tee /hello
# "Welcome to GitLab, @GITLAB_USERNAME_ASSOCIATED_WITH_SSHKEY" should be printed here
# with the type of build progress is defined as `plain`.
```



```console
$ eval $(ssh-agent)
$ ssh-add ~/.ssh/id_rsa
(Input your passphrase here)
$ docker buildx build --ssh default=$SSH_AUTH_SOCK .
```

You can also specify a path to `*.pem` file on the host directly instead of `$SSH_AUTH_SOCK`. However, pem files with passphrases are not supported.

​	您也可以直接指定主机上的 `*.pem` 文件路径，而不是 `$SSH_AUTH_SOCK`。但是，不支持带密码短语的 pem 文件。

### RUN `--network`



```dockerfile
RUN --network=TYPE
```

`RUN --network` allows control over which networking environment the command is run in.

​	`RUN --network` 允许控制运行命令的网络环境。

The supported network types are:

​	支持的网络类型有：

| Type 类型                                    | Description 描述                                             |
| -------------------------------------------- | ------------------------------------------------------------ |
| [`default`](#run---networkdefault) (default) | Run in the default network. 在默认网络中运行。               |
| [`none`](#run---networknone)                 | Run with no network access. 无网络访问运行。                 |
| [`host`](#run---networkhost)                 | Run in the host's network environment. 在主机的网络环境中运行。 |

### RUN `--network=default`

Equivalent to not supplying a flag at all, the command is run in the default network for the build.

​	等同于不提供标志的情况，命令在构建的默认网络中运行。

### RUN `--network=none`

The command is run with no network access (`lo` is still available, but is isolated to this process)

​	命令在无网络访问的情况下运行（`lo` 仍然可用，但仅限于此进程）。

#### 示例：隔离外部影响 Example: isolating external effects



```dockerfile
# syntax=docker/dockerfile:1
FROM python:3.6
ADD mypackage.tgz wheels/
RUN --network=none pip install --find-links wheels mypackage
```

`pip` will only be able to install the packages provided in the tarfile, which can be controlled by an earlier build stage.

​	此时，`pip` 只能安装 tar 文件中提供的包，这可以通过前一个构建阶段来控制。

### RUN `--network=host`

The command is run in the host's network environment (similar to `docker build --network=host`, but on a per-instruction basis)

​	命令在主机的网络环境中运行（类似于 `docker build --network=host`，但是在每个指令的基础上设置）。

> **Warning**
>
> The use of `--network=host` is protected by the `network.host` entitlement, which needs to be enabled when starting the buildkitd daemon with `--allow-insecure-entitlement network.host` flag or in [buildkitd config](https://github.com/moby/buildkit/blob/master/docs/buildkitd.toml.md), and for a build request with [`--allow network.host` flag](https://docs.docker.com/engine/reference/commandline/buildx_build/#allow).
>
> ​	使用 `--network=host` 受 `network.host` 权限保护，需在启动 buildkitd 守护进程时启用 `--allow-insecure-entitlement network.host` 标志，或在 [buildkitd 配置](https://github.com/moby/buildkit/blob/master/docs/buildkitd.toml.md) 中启用，并在构建请求中使用 [`--allow network.host` 标志](https://docs.docker.com/engine/reference/commandline/buildx_build/#allow)。

### RUN `--security`

> **Note**
>
> Not yet available in stable syntax, use [`docker/dockerfile:1-labs`](#syntax) version.
>
> ​	尚未在稳定语法中提供，使用 [`docker/dockerfile:1-labs`](#syntax) 版本。



```dockerfile
RUN --security=<sandbox|insecure>
```

The default security mode is `sandbox`. With `--security=insecure`, the builder runs the command without sandbox in insecure mode, which allows to run flows requiring elevated privileges (e.g. containerd). This is equivalent to running `docker run --privileged`.

​	默认安全模式为 `sandbox`。使用 `--security=insecure` 时，构建器将在不安全模式下运行命令，允许执行需要提升权限的流程（例如 containerd）。这等效于运行 `docker run --privileged`。

> **Warning**
>
> In order to access this feature, entitlement `security.insecure` should be enabled when starting the buildkitd daemon with `--allow-insecure-entitlement security.insecure` flag or in [buildkitd config](https://github.com/moby/buildkit/blob/master/docs/buildkitd.toml.md), and for a build request with [`--allow security.insecure` flag](https://docs.docker.com/engine/reference/commandline/buildx_build/#allow).
>
> ​	要访问此功能，需要在启动 buildkitd 守护进程时启用 `security.insecure` 权限，通过 `--allow-insecure-entitlement security.insecure` 标志，或在 [buildkitd 配置](https://github.com/moby/buildkit/blob/master/docs/buildkitd.toml.md) 中启用，并在构建请求中使用 [`--allow security.insecure` 标志](https://docs.docker.com/engine/reference/commandline/buildx_build/#allow)。

Default sandbox mode can be activated via `--security=sandbox`, but that is no-op.

​	默认的 sandbox 模式可以通过 `--security=sandbox` 启用，但该操作无效。

#### 示例：检查权限 Example: check entitlements



```dockerfile
# syntax=docker/dockerfile:1-labs
FROM ubuntu
RUN --security=insecure cat /proc/self/status | grep CapEff
```



```text
#84 0.093 CapEff:	0000003fffffffff
```

## CMD

The `CMD` instruction sets the command to be executed when running a container from an image.

​	`CMD` 指令用于设置从镜像运行容器时要执行的命令。

You can specify `CMD` instructions using [shell or exec forms](#shell-and-exec-form):

​	可以使用 [shell 或 exec 形式](#shell-and-exec-form) 来指定 `CMD` 指令：

- `CMD ["executable","param1","param2"]` (exec form) （exec 形式）
- `CMD ["param1","param2"]` (exec form, as default parameters to `ENTRYPOINT`) （exec 形式，作为 `ENTRYPOINT` 的默认参数）
- `CMD command param1 param2` (shell form) （shell 形式）

There can only be one `CMD` instruction in a Dockerfile. If you list more than one `CMD`, only the last one takes effect.

​	在 Dockerfile 中只能有一个 `CMD` 指令。如果列出多个 `CMD`，则只有最后一个生效。

The purpose of a `CMD` is to provide defaults for an executing container. These defaults can include an executable, or they can omit the executable, in which case you must specify an `ENTRYPOINT` instruction as well.

​	`CMD` 的作用是为执行容器提供默认值。默认值可以包括一个可执行文件，或者省略可执行文件，在这种情况下必须指定 `ENTRYPOINT` 指令。

If you would like your container to run the same executable every time, then you should consider using `ENTRYPOINT` in combination with `CMD`. See [`ENTRYPOINT`](#entrypoint). If the user specifies arguments to `docker run` then they will override the default specified in `CMD`, but still use the default `ENTRYPOINT`.

​	如果希望容器每次都运行相同的可执行文件，建议结合使用 `ENTRYPOINT` 和 `CMD`。参见 [`ENTRYPOINT`](#entrypoint)。如果用户为 `docker run` 指定了参数，则会覆盖 `CMD` 中指定的默认值，但仍会使用默认的 `ENTRYPOINT`。

If `CMD` is used to provide default arguments for the `ENTRYPOINT` instruction, both the `CMD` and `ENTRYPOINT` instructions should be specified in the [exec form](#exec-form).

​	如果 `CMD` 用于为 `ENTRYPOINT` 指令提供默认参数，则 `CMD` 和 `ENTRYPOINT` 指令应使用 [exec 形式](#exec-form)。

> **Note**
>
> Don't confuse `RUN` with `CMD`. `RUN` actually runs a command and commits the result; `CMD` doesn't execute anything at build time, but specifies the intended command for the image.
>
> ​	不要将 `RUN` 与 `CMD` 混淆。`RUN` 实际上会运行命令并提交结果；而 `CMD` 在构建时不会执行任何操作，只是为镜像指定预期的命令。

## LABEL



```dockerfile
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```

The `LABEL` instruction adds metadata to an image. A `LABEL` is a key-value pair. To include spaces within a `LABEL` value, use quotes and backslashes as you would in command-line parsing. A few usage examples:

​	`LABEL` 指令为镜像添加元数据。`LABEL` 是一个键值对。要在 `LABEL` 值中包含空格，可以像命令行解析那样使用引号和反斜杠。一些示例如下：



```dockerfile
LABEL "com.example.vendor"="ACME Incorporated"
LABEL com.example.label-with-value="foo"
LABEL version="1.0"
LABEL description="This text illustrates \
that label-values can span multiple lines."
```

An image can have more than one label. You can specify multiple labels on a single line. Prior to Docker 1.10, this decreased the size of the final image, but this is no longer the case. You may still choose to specify multiple labels in a single instruction, in one of the following two ways:

​	一个镜像可以有多个标签。可以在一行中指定多个标签。在 Docker 1.10 之前，这种做法会减少最终镜像的大小，但现在不再是这样。您仍可以选择在单个指令中指定多个标签，方法如下：



```dockerfile
LABEL multi.label1="value1" multi.label2="value2" other="value3"
```



```dockerfile
LABEL multi.label1="value1" \
      multi.label2="value2" \
      other="value3"
```

> **Note**
>
> Be sure to use double quotes and not single quotes. Particularly when you are using string interpolation (e.g. `LABEL example="foo-$ENV_VAR"`), single quotes will take the string as is without unpacking the variable's value.
>
> ​	确保使用双引号而不是单引号。尤其是在使用字符串插值时（例如 `LABEL example="foo-$ENV_VAR"`），单引号会将字符串按原样处理，不会解析变量的值。

Labels included in base or parent images (images in the `FROM` line) are inherited by your image. If a label already exists but with a different value, the most-recently-applied value overrides any previously-set value.

​	基础镜像或父镜像（`FROM` 行中的镜像）中包含的标签会被继承。如果标签已经存在但值不同，则最新应用的值会覆盖先前设置的值。

To view an image's labels, use the `docker image inspect` command. You can use the `--format` option to show just the labels;

​	要查看镜像的标签，使用 `docker image inspect` 命令。可以使用 `--format` 选项仅显示标签：



```console
$ docker image inspect --format='{{json .Config.Labels}}' myimage
```



```json
{
  "com.example.vendor": "ACME Incorporated",
  "com.example.label-with-value": "foo",
  "version": "1.0",
  "description": "This text illustrates that label-values can span multiple lines.",
  "multi.label1": "value1",
  "multi.label2": "value2",
  "other": "value3"
}
```

## MAINTAINER (deprecated)



```dockerfile
MAINTAINER <name>
```

The `MAINTAINER` instruction sets the *Author* field of the generated images. The `LABEL` instruction is a much more flexible version of this and you should use it instead, as it enables setting any metadata you require, and can be viewed easily, for example with `docker inspect`. To set a label corresponding to the `MAINTAINER` field you could use:

​	`MAINTAINER` 指令设置生成镜像的 *Author* 字段。`LABEL` 指令是它的更灵活版本，建议使用 `LABEL`，因为它可以设置任何需要的元数据，并且可以轻松查看，例如通过 `docker inspect`。要设置对应于 `MAINTAINER` 字段的标签，可以使用：



```dockerfile
LABEL org.opencontainers.image.authors="SvenDowideit@home.org.au"
```

This will then be visible from `docker inspect` with the other labels.

​	这样，标签将与其他标签一起在 `docker inspect` 中可见。

## EXPOSE



```dockerfile
EXPOSE <port> [<port>/<protocol>...]
```

The `EXPOSE` instruction informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if you don't specify a protocol.

​	`EXPOSE` 指令告知 Docker 容器在运行时监听指定的网络端口。可以指定端口监听的是 TCP 还是 UDP，默认是 TCP。

The `EXPOSE` instruction doesn't actually publish the port. It functions as a type of documentation between the person who builds the image and the person who runs the container, about which ports are intended to be published. To publish the port when running the container, use the `-p` flag on `docker run` to publish and map one or more ports, or the `-P` flag to publish all exposed ports and map them to high-order ports.

​	`EXPOSE` 指令并不真正发布端口，而是用于在构建镜像的人和运行容器的人之间起到一种文档作用，说明哪些端口应发布。在运行容器时，可以使用 `docker run` 的 `-p` 标志发布和映射一个或多个端口，或使用 `-P` 标志发布所有已暴露的端口并映射到高位端口。

By default, `EXPOSE` assumes TCP. You can also specify UDP:

​	默认情况下，`EXPOSE` 假定是 TCP。也可以指定 UDP：



```dockerfile
EXPOSE 80/udp
```

To expose on both TCP and UDP, include two lines:

​	要同时暴露 TCP 和 UDP，可以包含两行：



```dockerfile
EXPOSE 80/tcp
EXPOSE 80/udp
```

In this case, if you use `-P` with `docker run`, the port will be exposed once for TCP and once for UDP. Remember that `-P` uses an ephemeral high-ordered host port on the host, so TCP and UDP doesn't use the same port.

​	在这种情况下，如果使用 `docker run` 的 `-P`，该端口将分别暴露一次用于 TCP 和 UDP。请注意，`-P` 使用主机上的高位端口，因此 TCP 和 UDP 端口不会使用相同的端口。

Regardless of the `EXPOSE` settings, you can override them at runtime by using the `-p` flag. For example

​	无论 `EXPOSE` 设置如何，都可以在运行时通过 `-p` 标志覆盖。例如：



```console
$ docker run -p 80:80/tcp -p 80:80/udp ...
```

To set up port redirection on the host system, see [using the -P flag](https://docs.docker.com/reference/cli/docker/container/run/#publish). The `docker network` command supports creating networks for communication among containers without the need to expose or publish specific ports, because the containers connected to the network can communicate with each other over any port. For detailed information, see the [overview of this feature](https://docs.docker.com/engine/userguide/networking/).

​	要在主机系统上设置端口重定向，请参见[使用 -P 标志](https://docs.docker.com/reference/cli/docker/container/run/#publish)。`docker network` 命令支持创建用于容器间通信的网络，而无需暴露或发布特定端口，因为连接到网络的容器可以通过任意端口互相通信。详细信息请参见[该功能概述](https://docs.docker.com/engine/userguide/networking/)。

## ENV



```dockerfile
ENV <key>=<value> ...
```

The `ENV` instruction sets the environment variable `<key>` to the value `<value>`. This value will be in the environment for all subsequent instructions in the build stage and can be [replaced inline](#environment-replacement) in many as well. The value will be interpreted for other environment variables, so quote characters will be removed if they are not escaped. Like command line parsing, quotes and backslashes can be used to include spaces within values.

​	`ENV` 指令将环境变量 `<key>` 设置为值 `<value>`。该值将在构建阶段的所有后续指令中生效，并且在许多情况下可以[内联替换](#environment-replacement)。该值将会解释为其他环境变量，因此如果引号未转义则会被移除。像命令行解析一样，可以使用引号和反斜杠在值中包含空格。

Example: 示例：

​	

```dockerfile
ENV MY_NAME="John Doe"
ENV MY_DOG=Rex\ The\ Dog
ENV MY_CAT=fluffy
```

The `ENV` instruction allows for multiple `<key>=<value> ...` variables to be set at one time, and the example below will yield the same net results in the final image:

​	`ENV` 指令允许一次设置多个 `<key>=<value> ...` 变量，以下示例将生成相同的最终镜像结果：



```dockerfile
ENV MY_NAME="John Doe" MY_DOG=Rex\ The\ Dog \
    MY_CAT=fluffy
```

The environment variables set using `ENV` will persist when a container is run from the resulting image. You can view the values using `docker inspect`, and change them using `docker run --env <key>=<value>`.

​	使用 `ENV` 设置的环境变量在从生成的镜像运行容器时会保留。可以使用 `docker inspect` 查看这些值，并使用 `docker run --env <key>=<value>` 更改它们。

A stage inherits any environment variables that were set using `ENV` by its parent stage or any ancestor. Refer to the [multi-stage builds section]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}}) in the manual for more information.

​	一个阶段会继承其父阶段或任何祖先阶段通过 `ENV` 设置的环境变量。有关更多信息，请参阅手册中的[多阶段构建部分]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}})。

Environment variable persistence can cause unexpected side effects. For example, setting `ENV DEBIAN_FRONTEND=noninteractive` changes the behavior of `apt-get`, and may confuse users of your image.

​	环境变量的持久性可能会导致意外的副作用。例如，设置 `ENV DEBIAN_FRONTEND=noninteractive` 会更改 `apt-get` 的行为，可能会使您的镜像用户感到困惑。

If an environment variable is only needed during build, and not in the final image, consider setting a value for a single command instead:

​	如果环境变量仅在构建期间需要而不需要保留在最终镜像中，考虑仅为单个命令设置值：



```dockerfile
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y ...
```

Or using [`ARG`](#arg), which is not persisted in the final image:

​	或者使用 [`ARG`](#arg)，它不会在最终镜像中保留：



```dockerfile
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y ...
```

> **Note**
>
> **Alternative syntax 替代语法**
>
> The `ENV` instruction also allows an alternative syntax `ENV <key> <value>`, omitting the `=`. For example:
>
> ​	`ENV` 指令也允许替代语法 `ENV <key> <value>`，省略 `=`。例如：
>
> ```dockerfile
> ENV MY_VAR my-value
> ```
>
> This syntax does not allow for multiple environment-variables to be set in a single `ENV` instruction, and can be confusing. For example, the following sets a single environment variable (`ONE`) with value `"TWO= THREE=world"`:
>
> ​	这种语法不允许在单个 `ENV` 指令中设置多个环境变量，可能会产生混淆。例如，以下内容会设置一个值为 `"TWO= THREE=world"` 的环境变量 (`ONE`)：
>
> ```dockerfile
> ENV ONE TWO= THREE=world
> ```
>
> The alternative syntax is supported for backward compatibility, but discouraged for the reasons outlined above, and may be removed in a future release.
>
> ​	这种替代语法是为了向后兼容而支持的，但不推荐使用，因为存在上述原因，并且可能在未来版本中移除。

## ADD

ADD has two forms. The latter form is required for paths containing whitespace.

​	ADD 有两种形式。后一种形式适用于包含空格的路径。



```dockerfile
ADD [OPTIONS] <src> ... <dest>
ADD [OPTIONS] ["<src>", ... "<dest>"]
```

The available `[OPTIONS]` are:

​	可用的 `[OPTIONS]` 选项有：

| Option 选项                             | Minimum Dockerfile version 最低 Dockerfile 版本 |
| --------------------------------------- | ----------------------------------------------- |
| [`--keep-git-dir`](#add---keep-git-dir) | 1.1                                             |
| [`--checksum`](#add---checksum)         | 1.6                                             |
| [`--chown`](#add---chown---chmod)       |                                                 |
| [`--chmod`](#add---chown---chmod)       | 1.2                                             |
| [`--link`](#add---link)                 | 1.4                                             |
| [`--exclude`](#add---exclude)           | 1.7                                             |

The `ADD` instruction copies new files or directories from `<src>` and adds them to the filesystem of the image at the path `<dest>`. Files and directories can be copied from the build context, a remote URL, or a Git repository.

​	`ADD` 指令将新文件或目录从 `<src>` 复制并添加到镜像文件系统的 `<dest>` 路径。可以从构建上下文、远程 URL 或 Git 仓库复制文件和目录。

The `ADD` and `COPY` instructions are functionally similar, but serve slightly different purposes. Learn more about the [differences between `ADD` and `COPY`](https://docs.docker.com/build/building/best-practices/#add-or-copy).

​	`ADD` 和 `COPY` 指令在功能上类似，但用途略有不同。了解更多有关 [`ADD` 和 `COPY` 的区别](https://docs.docker.com/build/building/best-practices/#add-or-copy)。

### Source

You can specify multiple source files or directories with `ADD`. The last argument must always be the destination. For example, to add two files, `file1.txt` and `file2.txt`, from the build context to `/usr/src/things/` in the build container:

​	可以使用 `ADD` 指定多个源文件或目录。最后一个参数必须始终是目标。例如，要将构建上下文中的两个文件 `file1.txt` 和 `file2.txt` 添加到构建容器中的 `/usr/src/things/`：



```dockerfile
ADD file1.txt file2.txt /usr/src/things/
```

If you specify multiple source files, either directly or using a wildcard, then the destination must be a directory (must end with a slash `/`).

​	如果直接指定多个源文件或使用通配符，则目标必须是一个目录（必须以 `/` 结尾）。

To add files from a remote location, you can specify a URL or the address of a Git repository as the source. For example:

​	要从远程位置添加文件，可以指定 URL 或 Git 仓库的地址作为源。例如：

```dockerfile
ADD https://example.com/archive.zip /usr/src/things/
ADD git@github.com:user/repo.git /usr/src/things/
```

BuildKit detects the type of `<src>` and processes it accordingly.

​	BuildKit 检测 `<src>` 的类型并相应处理：

- If `<src>` is a local file or directory, the contents of the directory are copied to the specified destination. See [Adding files from the build context](#adding-files-from-the-build-context).
  - 如果 `<src>` 是本地文件或目录，目录内容将被复制到指定目标。参见[从构建上下文添加文件](#adding-files-from-the-build-context)。

- If `<src>` is a local tar archive, it is decompressed and extracted to the specified destination. See [Adding local tar archives](#adding-local-tar-archives).
  - 如果 `<src>` 是本地 tar 存档，将解压缩并提取到指定目标。参见[添加本地 tar 存档](#adding-local-tar-archives)。

- If `<src>` is a URL, the contents of the URL are downloaded and placed at the specified destination. See [Adding files from a URL](#adding-files-from-a-url).
  - 如果 `<src>` 是 URL，将下载 URL 内容并放置在指定目标。参见[从 URL 添加文件](#adding-files-from-a-url)。

- If `<src>` is a Git repository, the repository is cloned to the specified destination. See [Adding files from a Git repository](#adding-files-from-a-git-repository).
  - 如果 `<src>` 是 Git 仓库，将克隆仓库到指定目标。参见[从 Git 仓库添加文件](#adding-files-from-a-git-repository)。

#### 从构建上下文添加文件 Adding files from the build context

Any relative or local path that doesn't begin with a `http://`, `https://`, or `git@` protocol prefix is considered a local file path. The local file path is relative to the build context. For example, if the build context is the current directory, `ADD file.txt /` adds the file at `./file.txt` to the root of the filesystem in the build container.

​	任何不以 `http://`、`https://` 或 `git@` 协议前缀开头的相对或本地路径都被视为本地文件路径。本地文件路径相对于构建上下文。例如，如果构建上下文是当前目录，`ADD file.txt /` 会将 `./file.txt` 文件添加到构建容器文件系统的根目录。

When adding source files from the build context, their paths are interpreted as relative to the root of the context. If you specify a relative path leading outside of the build context, such as `ADD ../something /something`, parent directory paths are stripped out automatically. The effective source path in this example becomes `ADD something /something`.

​	当从构建上下文添加源文件时，它们的路径解释为相对于上下文根目录。如果指定指向构建上下文之外的相对路径，例如 `ADD ../something /something`，父目录路径将自动剥离。在此示例中，有效的源路径变为 `ADD something /something`。

If the source is a directory, the contents of the directory are copied, including filesystem metadata. The directory itself isn't copied, only its contents. If it contains subdirectories, these are also copied, and merged with any existing directories at the destination. Any conflicts are resolved in favor of the content being added, on a file-by-file basis, except if you're trying to copy a directory onto an existing file, in which case an error is raised.

​	如果源是目录，则目录内容（包括文件系统元数据）将被复制。仅复制目录内容，而不是目录本身。如果目录包含子目录，这些子目录也会被复制，并与目标中的现有目录合并。如果有冲突，逐文件处理，并优先添加新内容，除非试图将目录复制到现有文件上，这种情况下会引发错误。

If the source is a file, the file and its metadata are copied to the destination. File permissions are preserved. If the source is a file and a directory with the same name exists at the destination, an error is raised.

​	如果源是文件，则文件及其元数据将被复制到目标。文件权限将被保留。如果源是文件且目标存在同名目录，将引发错误。

If you pass a Dockerfile through stdin to the build (`docker build - < Dockerfile`), there is no build context. In this case, you can only use the `ADD` instruction to copy remote files. You can also pass a tar archive through stdin: (`docker build - < archive.tar`), the Dockerfile at the root of the archive and the rest of the archive will be used as the context of the build.

​	如果通过标准输入将 Dockerfile 传递给构建（`docker build - < Dockerfile`），则没有构建上下文。这种情况下只能使用 `ADD` 指令复制远程文件。您也可以通过标准输入传递 tar 存档（`docker build - < archive.tar`），存档根目录的 Dockerfile 和其余内容将用作构建的上下文。

##### 模式匹配 Pattern matching

For local files, each `<src>` may contain wildcards and matching will be done using Go's [filepath.Match](https://golang.org/pkg/path/filepath#Match) rules.

​	对于本地文件，每个 `<src>` 可以包含通配符，并使用 Go 的 [filepath.Match](https://golang.org/pkg/path/filepath#Match) 规则进行匹配。

For example, to add all files and directories in the root of the build context ending with `.png`:

​	例如，要添加构建上下文根目录中以 `.png` 结尾的所有文件和目录：

```dockerfile
ADD *.png /dest/
```

In the following example, `?` is a single-character wildcard, matching e.g. `index.js` and `index.ts`.

​	在以下示例中，`?` 是单字符通配符，可匹配 `index.js` 和 `index.ts`。

```dockerfile
ADD index.?s /dest/
```

When adding files or directories that contain special characters (such as `[` and `]`), you need to escape those paths following the Golang rules to prevent them from being treated as a matching pattern. For example, to add a file named `arr[0].txt`, use the following;

​	当添加包含特殊字符（如 `[` 和 `]`）的文件或目录时，需要按照 Golang 规则转义这些路径，以防止它们被视为匹配模式。例如，要添加名为 `arr[0].txt` 的文件，请使用以下命令：



```dockerfile
ADD arr[[]0].txt /dest/
```

#### 添加本地 tar 存档 Adding local tar archives

When using a local tar archive as the source for `ADD`, and the archive is in a recognized compression format (`gzip`, `bzip2` or `xz`, or uncompressed), the archive is decompressed and extracted into the specified destination. Only local tar archives are extracted. If the tar archive is a remote URL, the archive is not extracted, but downloaded and placed at the destination.

​	当使用本地 tar 存档作为 `ADD` 的源时，如果该存档是已识别的压缩格式（如 `gzip`、`bzip2` 或 `xz`，或未压缩），将解压并提取到指定目标。仅本地 tar 存档会被提取。如果 tar 存档是远程 URL，则不会提取，而是下载并放置在目标位置。

When a directory is extracted, it has the same behavior as `tar -x`. The result is the union of:

​	当提取目录时，其行为类似 `tar -x`。结果为：

1. Whatever existed at the destination path, and 目标路径中已有的内容，以及
2. The contents of the source tree, with conflicts resolved in favor of the content being added, on a file-by-file basis. 源树的内容，按文件逐个处理，冲突时优先添加新内容。

> **Note**
>
> Whether a file is identified as a recognized compression format or not is done solely based on the contents of the file, not the name of the file. For example, if an empty file happens to end with `.tar.gz` this isn't recognized as a compressed file and doesn't generate any kind of decompression error message, rather the file will simply be copied to the destination.
>
> ​	文件是否识别为压缩格式仅基于文件内容，而非文件名。例如，如果一个空文件以 `.tar.gz` 结尾，它不会被识别为压缩文件，也不会生成任何解压错误消息，而是直接复制到目标。

#### 从 URL 添加文件 Adding files from a URL

In the case where source is a remote file URL, the destination will have permissions of 600. If the HTTP response contains a `Last-Modified` header, the timestamp from that header will be used to set the `mtime` on the destination file. However, like any other file processed during an `ADD`, `mtime` isn't included in the determination of whether or not the file has changed and the cache should be updated.

​	当源是远程文件 URL 时，目标的权限将为 600。如果 HTTP 响应包含 `Last-Modified` 标头，将使用该标头的时间戳设置目标文件的 `mtime`。但是，像 `ADD` 处理的其他文件一样，`mtime` 不会用于确定文件是否更改，也不会影响缓存更新。

If the destination ends with a trailing slash, then the filename is inferred from the URL path. For example, `ADD http://example.com/foobar /` would create the file `/foobar`. The URL must have a nontrivial path so that an appropriate filename can be discovered (`http://example.com` doesn't work).

​	如果目标以斜杠结尾，则文件名会从 URL 路径中推断。例如，`ADD http://example.com/foobar /` 会创建文件 `/foobar`。URL 必须有非空路径，以便发现合适的文件名（`http://example.com` 不行）。

If the destination doesn't end with a trailing slash, the destination path becomes the filename of the file downloaded from the URL. For example, `ADD http://example.com/foo /bar` creates the file `/bar`.

​	如果目标不以斜杠结尾，目标路径则成为从 URL 下载文件的文件名。例如，`ADD http://example.com/foo /bar` 创建文件 `/bar`。

If your URL files are protected using authentication, you need to use `RUN wget`, `RUN curl` or use another tool from within the container as the `ADD` instruction doesn't support authentication.

​	如果您的 URL 文件受身份验证保护，需要在容器中使用 `RUN wget`、`RUN curl` 或其他工具，因为 `ADD` 指令不支持身份验证。

#### 从 Git 仓库添加文件 Adding files from a Git repository

To use a Git repository as the source for `ADD`, you can reference the repository's HTTP or SSH address as the source. The repository is cloned to the specified destination in the image.

​	要使用 Git 仓库作为 `ADD` 的源，可以使用仓库的 HTTP 或 SSH 地址作为源。仓库会被克隆到镜像中的指定目标位置。

```dockerfile
ADD https://github.com/user/repo.git /mydir/
```

You can use URL fragments to specify a specific branch, tag, commit, or subdirectory. For example, to add the `docs` directory of the `v0.14.1` tag of the `buildkit` repository:

​	可以使用 URL 片段指定特定分支、标签、提交或子目录。例如，要添加 `buildkit` 仓库 `v0.14.1` 标签的 `docs` 目录：

```dockerfile
ADD git@github.com:moby/buildkit.git#v0.14.1:docs /buildkit-docs
```

For more information about Git URL fragments, see [URL fragments](https://docs.docker.com/build/building/context/#url-fragments).

​	有关 Git URL 片段的更多信息，请参见[URL 片段](https://docs.docker.com/build/building/context/#url-fragments)。

When adding from a Git repository, the permissions bits for files are 644. If a file in the repository has the executable bit set, it will have permissions set to 755. Directories have permissions set to 755.

​	当从 Git 仓库添加文件时，文件的权限位为 644。如果仓库中文件设置了可执行位，权限将设置为 755。目录权限设置为 755。

When using a Git repository as the source, the repository must be accessible from the build context. To add a repository via SSH, whether public or private, you must pass an SSH key for authentication. For example, given the following Dockerfile:

​	使用 Git 仓库作为源时，仓库必须能够从构建上下文访问。要通过 SSH 添加仓库，无论公共或私有，都需要传递 SSH 密钥进行身份验证。例如，给定以下 Dockerfile：

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
ADD git@git.example.com:foo/bar.git /bar
```

To build this Dockerfile, pass the `--ssh` flag to the `docker build` to mount the SSH agent socket to the build. For example:

​	构建此 Dockerfile 时，需传递 `--ssh` 标志给 `docker build`，以挂载 SSH 代理套接字至构建。例如：

```console
$ docker build --ssh default .
```

For more information about building with secrets, see [Build secrets]({{< ref "/manuals/DockerBuild/Building/Secrets" >}}).

​	关于使用密钥构建的更多信息，请参见[构建密钥]({{< ref "/manuals/DockerBuild/Building/Secrets" >}})。

### Destination

If the destination path begins with a forward slash, it's interpreted as an absolute path, and the source files are copied into the specified destination relative to the root of the current build stage.

​	如果目标路径以斜杠开头，则被解释为绝对路径，源文件将相对于当前构建阶段的根目录复制到指定目标位置。

```dockerfile
# create /abs/test.txt
ADD test.txt /abs/
```

Trailing slashes are significant. For example, `ADD test.txt /abs` creates a file at `/abs`, whereas `ADD test.txt /abs/` creates `/abs/test.txt`.

​	斜杠结尾具有特定含义。例如，`ADD test.txt /abs` 在 `/abs` 处创建文件，而 `ADD test.txt /abs/` 创建 `/abs/test.txt`。

If the destination path doesn't begin with a leading slash, it's interpreted as relative to the working directory of the build container.

​	如果目标路径不以斜杠开头，则被解释为相对于构建容器的工作目录。

```dockerfile
WORKDIR /usr/src/app
# create /usr/src/app/rel/test.txt
ADD test.txt rel/
```

If destination doesn't exist, it's created, along with all missing directories in its path.

​	如果目标不存在，则会创建它，以及路径中所有缺失的目录。

If the source is a file, and the destination doesn't end with a trailing slash, the source file will be written to the destination path as a file.

​	如果源是文件且目标不以斜杠结尾，源文件将写入目标路径作为文件。

### ADD `--keep-git-dir`



```dockerfile
ADD [--keep-git-dir=<boolean>] <src> ... <dir>
```

When `<src>` is the HTTP or SSH address of a remote Git repository, BuildKit adds the contents of the Git repository to the image excluding the `.git` directory by default.

​	当 `<src>` 是远程 Git 仓库的 HTTP 或 SSH 地址时，BuildKit 默认将仓库内容添加到镜像中，但不包括 `.git` 目录。

The `--keep-git-dir=true` flag lets you preserve the `.git` directory.

​	`--keep-git-dir=true` 标志允许保留 `.git` 目录。



```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
ADD --keep-git-dir=true https://github.com/moby/buildkit.git#v0.10.1 /buildkit
```

### ADD `--checksum`



```dockerfile
ADD [--checksum=<hash>] <src> ... <dir>
```

The `--checksum` flag lets you verify the checksum of a remote resource. The checksum is formatted as `<algorithm>:<hash>`. The supported algorithms are `sha256`, `sha384`, and `sha512`.

​	`--checksum` 标志允许您验证远程资源的校验和。校验和格式为 `<algorithm>:<hash>`。支持的算法有 `sha256`、`sha384` 和 `sha512`。



```dockerfile
ADD --checksum=sha256:24454f830cdb571e2c4ad15481119c43b3cafd48dd869a9b2945d1036d1dc68d https://mirrors.edge.kernel.org/pub/linux/kernel/Historic/linux-0.01.tar.gz /
```

The `--checksum` flag only supports HTTP(S) sources.

​	`--checksum` 标志仅支持 HTTP(S) 源。

### ADD `--chown --chmod`

See [`COPY --chown --chmod`](#copy---chown---chmod).

### ADD `--link`

See [`COPY --link`](#copy---link).

### ADD --exclude

See [`COPY --exclude`](#copy---exclude).

## COPY

COPY has two forms. The latter form is required for paths containing whitespace.

​	COPY 有两种格式。路径包含空格时，必须使用第二种格式。



```dockerfile
COPY [OPTIONS] <src> ... <dest>
COPY [OPTIONS] ["<src>", ... "<dest>"]
```

The available `[OPTIONS]` are:

​	可用的 `[OPTIONS]` 如下：

| Option 选项                        | Minimum Dockerfile version 最低 Dockerfile 版本 |
| ---------------------------------- | ----------------------------------------------- |
| [`--from`](#copy---from)           |                                                 |
| [`--chown`](#copy---chown---chmod) |                                                 |
| [`--chmod`](#copy---chown---chmod) | 1.2                                             |
| [`--link`](#copy---link)           | 1.4                                             |
| [`--parents`](#copy---parents)     | 1.7                                             |
| [`--exclude`](#copy---exclude)     | 1.7                                             |

The `COPY` instruction copies new files or directories from `<src>` and adds them to the filesystem of the image at the path `<dest>`. Files and directories can be copied from the build context, build stage, named context, or an image.

​	`COPY` 指令从 `<src>` 复制新文件或目录，并将其添加到镜像文件系统的 `<dest>` 路径。可以从构建上下文、构建阶段、命名上下文或镜像中复制文件和目录。

The `ADD` and `COPY` instructions are functionally similar, but serve slightly different purposes. Learn more about the [differences between `ADD` and `COPY`](https://docs.docker.com/build/building/best-practices/#add-or-copy).

​	`ADD` 和 `COPY` 指令在功能上相似，但用处略有不同。了解更多关于 [ADD 和 COPY 的区别](https://docs.docker.com/build/building/best-practices/#add-or-copy)。

### Source

You can specify multiple source files or directories with `COPY`. The last argument must always be the destination. For example, to copy two files, `file1.txt` and `file2.txt`, from the build context to `/usr/src/things/` in the build container:

​	可以使用 `COPY` 指定多个源文件或目录。最后一个参数必须始终是目标。例如，要将构建上下文中的两个文件 `file1.txt` 和 `file2.txt` 复制到构建容器中的 `/usr/src/things/`：



```dockerfile
COPY file1.txt file2.txt /usr/src/things/
```

If you specify multiple source files, either directly or using a wildcard, then the destination must be a directory (must end with a slash `/`).

​	如果直接指定多个源文件或使用通配符，则目标必须是一个目录（必须以 `/` 结尾）。

`COPY` accepts a flag `--from=<name>` that lets you specify the source location to be a build stage, context, or image. The following example copies files from a stage named `build`:

​	`COPY` 接受 `--from=<name>` 标志，允许您将源位置指定为构建阶段、上下文或镜像。以下示例从名为 `build` 的阶段复制文件：



```dockerfile
FROM golang AS build
WORKDIR /app
RUN --mount=type=bind,target=. go build -o /myapp ./cmd

COPY --from=build /myapp /usr/bin/
```

For more information about copying from named sources, see the [`--from` flag](#copy---from).

​	有关从命名源复制的更多信息，请参见 [`--from` 标志](#copy---from)。

#### 从构建上下文复制 Copying from the build context

When copying source files from the build context, their paths are interpreted as relative to the root of the context. If you specify a relative path leading outside of the build context, such as `COPY ../something /something`, parent directory paths are stripped out automatically. The effective source path in this example becomes `COPY something /something`.

​	从构建上下文复制源文件时，其路径解释为相对于上下文根目录。如果指定指向构建上下文之外的相对路径，例如 `COPY ../something /something`，父目录路径会自动剥离。在此示例中，有效的源路径变为 `COPY something /something`。

If the source is a directory, the contents of the directory are copied, including filesystem metadata. The directory itself isn't copied, only its contents. If it contains subdirectories, these are also copied, and merged with any existing directories at the destination. Any conflicts are resolved in favor of the content being added, on a file-by-file basis, except if you're trying to copy a directory onto an existing file, in which case an error is raised.

​	如果源是目录，则复制该目录的内容（包括文件系统元数据）。仅复制目录内容，而不是目录本身。如果包含子目录，这些子目录也会被复制，并与目标中的现有目录合并。所有冲突将按文件逐个处理，优先保留新内容，除非试图将目录复制到现有文件上，这种情况下会引发错误。

If the source is a file, the file and its metadata are copied to the destination. File permissions are preserved. If the source is a file and a directory with the same name exists at the destination, an error is raised.

​	如果源是文件，则文件及其元数据将被复制到目标。文件权限保留。如果源是文件且目标中存在同名目录，将引发错误。

If you pass a Dockerfile through stdin to the build (`docker build - < Dockerfile`), there is no build context. In this case, you can only use the `COPY` instruction to copy files from other stages, named contexts, or images, using the [`--from` flag](#copy---from). You can also pass a tar archive through stdin: (`docker build - < archive.tar`), the Dockerfile at the root of the archive and the rest of the archive will be used as the context of the build.

​	通过标准输入将 Dockerfile 传递给构建（`docker build - < Dockerfile`）时，没有构建上下文。这种情况下，您只能使用 `COPY` 指令从其他阶段、命名上下文或镜像复制文件，并使用 [`--from` 标志](#copy---from)。您也可以通过标准输入传递 tar 存档（`docker build - < archive.tar`），存档根目录的 Dockerfile 和其余内容将用作构建上下文。

When using a Git repository as the build context, the permissions bits for copied files are 644. If a file in the repository has the executable bit set, it will have permissions set to 755. Directories have permissions set to 755.

​	使用 Git 仓库作为构建上下文时，复制的文件权限位为 644。如果仓库中的文件设置了可执行位，权限将设置为 755。目录权限设置为 755。

##### 模式匹配 Pattern matching

For local files, each `<src>` may contain wildcards and matching will be done using Go's [filepath.Match](https://golang.org/pkg/path/filepath#Match) rules.

​	对于本地文件，每个 `<src>` 可以包含通配符，并使用 Go 的 [filepath.Match](https://golang.org/pkg/path/filepath#Match) 规则进行匹配。

For example, to add all files and directories in the root of the build context ending with `.png`:

​	例如，要添加构建上下文根目录中以 `.png` 结尾的所有文件和目录：

```dockerfile
COPY *.png /dest/
```

In the following example, `?` is a single-character wildcard, matching e.g. `index.js` and `index.ts`.

​	在以下示例中，`?` 是单字符通配符，可匹配 `index.js` 和 `index.ts`。

```dockerfile
COPY index.?s /dest/
```

When adding files or directories that contain special characters (such as `[` and `]`), you need to escape those paths following the Golang rules to prevent them from being treated as a matching pattern. For example, to add a file named `arr[0].txt`, use the following;

​	添加包含特殊字符（如 `[` 和 `]`）的文件或目录时，需要按照 Golang 规则转义这些路径，以防止它们被视为匹配模式。例如，要添加名为 `arr[0].txt` 的文件，请使用以下命令：



```dockerfile
COPY arr[[]0].txt /dest/
```

### Destination

If the destination path begins with a forward slash, it's interpreted as an absolute path, and the source files are copied into the specified destination relative to the root of the current build stage.

​	如果目标路径以斜杠开头，则被解释为绝对路径，源文件将相对于当前构建阶段的根目录复制到指定目标位置。



```dockerfile
# create /abs/test.txt
COPY test.txt /abs/
```

Trailing slashes are significant. For example, `COPY test.txt /abs` creates a file at `/abs`, whereas `COPY test.txt /abs/` creates `/abs/test.txt`.

​	斜杠结尾具有特定含义。例如，`COPY test.txt /abs` 在 `/abs` 处创建文件，而 `COPY test.txt /abs/` 创建 `/abs/test.txt`。

If the destination path doesn't begin with a leading slash, it's interpreted as relative to the working directory of the build container.

​	如果目标路径不以斜杠开头，则被解释为相对于构建容器的工作目录。



```dockerfile
WORKDIR /usr/src/app
# create /usr/src/app/rel/test.txt
COPY test.txt rel/
```

If destination doesn't exist, it's created, along with all missing directories in its path.

​	如果目标路径不存在，它会被创建，且会同时创建路径中所有缺失的目录。

If the source is a file, and the destination doesn't end with a trailing slash, the source file will be written to the destination path as a file.

​	如果源是文件且目标路径没有以斜杠结尾，源文件将被写入到目标路径作为一个文件。

### `COPY --from`

By default, the `COPY` instruction copies files from the build context. The `COPY --from` flag lets you copy files from an image, a build stage, or a named context instead.

​	默认情况下，`COPY` 指令从构建上下文中复制文件。`COPY --from` 标志允许您从镜像、构建阶段或命名上下文中复制文件。

```dockerfile
COPY [--from=<image|stage|context>] <src> ... <dest>
```

To copy from a build stage in a [multi-stage build]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}}), specify the name of the stage you want to copy from. You specify stage names using the `AS` keyword with the `FROM` instruction.

​	在多阶段构建中从某个构建阶段复制文件时，指定您想要复制的阶段名称。您可以在 `FROM` 指令中使用 `AS` 关键字来指定阶段名称。



```dockerfile
# syntax=docker/dockerfile:1
FROM alpine AS build
COPY . .
RUN apk add clang
RUN clang -o /hello hello.c

FROM scratch
COPY --from=build /hello /
```

You can also copy files directly from named contexts (specified with `--build-context <name>=<source>`) or images. The following example copies an `nginx.conf` file from the official Nginx image.

​	您还可以直接从命名上下文（通过 `--build-context <name>=<source>` 指定）或镜像中复制文件。以下示例从官方 Nginx 镜像中复制 `nginx.conf` 文件。



```dockerfile
COPY --from=nginx:latest /etc/nginx/nginx.conf /nginx.conf
```

The source path of `COPY --from` is always resolved from filesystem root of the image or stage that you specify.

​	`COPY --from` 的源路径始终从您指定的镜像或阶段的文件系统根目录解析。

### `COPY --chown --chmod`

> **Note**
>
> Only octal notation is currently supported. Non-octal support is tracked in [moby/buildkit#1951](https://github.com/moby/buildkit/issues/1951).
>
> ​	当前仅支持八进制表示法。非八进制支持正在 [moby/buildkit#1951](https://github.com/moby/buildkit/issues/1951) 中跟踪。



```dockerfile
COPY [--chown=<user>:<group>] [--chmod=<perms> ...] <src> ... <dest>
```

The `--chown` and `--chmod` features are only supported on Dockerfiles used to build Linux containers, and doesn't work on Windows containers. Since user and group ownership concepts do not translate between Linux and Windows, the use of `/etc/passwd` and `/etc/group` for translating user and group names to IDs restricts this feature to only be viable for Linux OS-based containers.

​	`--chown` 和 `--chmod` 功能仅支持用于构建 Linux 容器的 Dockerfile，不适用于 Windows 容器。由于用户和组所有权的概念无法在 Linux 和 Windows 间互相翻译，Linux 容器中使用的 `/etc/passwd` 和 `/etc/group` 文件用于将用户名和组名转换为 ID，因此此功能仅适用于基于 Linux 操作系统的容器。

All files and directories copied from the build context are created with a UID and GID of `0` unless the optional `--chown` flag specifies a given username, groupname, or UID/GID combination to request specific ownership of the copied content. The format of the `--chown` flag allows for either username and groupname strings or direct integer UID and GID in any combination. Providing a username without groupname or a UID without GID will use the same numeric UID as the GID. If a username or groupname is provided, the container's root filesystem `/etc/passwd` and `/etc/group` files will be used to perform the translation from name to integer UID or GID respectively. The following examples show valid definitions for the `--chown` flag:

​	从构建上下文中复制的所有文件和目录的默认 UID 和 GID 均为 `0`，除非通过可选的 `--chown` 标志指定特定的用户名、组名或 UID/GID 组合来请求内容的特定所有权。`--chown` 标志的格式允许使用用户名和组名字符串或直接使用整数 UID 和 GID 的任意组合。如果仅提供用户名或 UID 而不提供组名或 GID，则 UID 和 GID 相同。以下示例展示了 `--chown` 标志的有效定义：

```dockerfile
COPY --chown=55:mygroup files* /somedir/
COPY --chown=bin files* /somedir/
COPY --chown=1 files* /somedir/
COPY --chown=10:11 files* /somedir/
COPY --chown=myuser:mygroup --chmod=644 files* /somedir/
```

If the container root filesystem doesn't contain either `/etc/passwd` or `/etc/group` files and either user or group names are used in the `--chown` flag, the build will fail on the `COPY` operation. Using numeric IDs requires no lookup and does not depend on container root filesystem content.

​	如果容器根文件系统中没有 `/etc/passwd` 或 `/etc/group` 文件，且 `--chown` 标志中使用了用户名或组名，`COPY` 操作将失败。使用数字 ID 不需要查找，并且不依赖于容器根文件系统内容。

With the Dockerfile syntax version 1.10.0 and later, the `--chmod` flag supports variable interpolation, which lets you define the permission bits using build arguments:

​	对于 Dockerfile 语法版本 1.10.0 及更高版本，`--chmod` 标志支持变量插值，这允许您使用构建参数定义权限位：



```dockerfile
# syntax=docker/dockerfile:1.10
FROM alpine
WORKDIR /src
ARG MODE=440
COPY --chmod=$MODE . .
```

### COPY --link



```dockerfile
COPY [--link[=<boolean>]] <src> ... <dest>
```

Enabling this flag in `COPY` or `ADD` commands allows you to copy files with enhanced semantics where your files remain independent on their own layer and don't get invalidated when commands on previous layers are changed.

​	在 `COPY` 或 `ADD` 指令中启用该标志后，允许您以增强的方式复制文件，文件保持独立于各自的层，并且在更改前一层中的命令时不会失效。

When `--link` is used your source files are copied into an empty destination directory. That directory is turned into a layer that is linked on top of your previous state.

​	使用 `--link` 时，源文件会被复制到一个空的目标目录中，该目录会被转为一层并与之前的状态链接。

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
COPY --link /foo /bar
```

Is equivalent of doing two builds:

​	等价于两次构建：

```dockerfile
FROM alpine
```

and 和



```dockerfile
FROM scratch
COPY /foo /bar
```

and merging all the layers of both images together.

​	并将两个镜像的所有层合并。

#### 使用 `--link` 的优势 Benefits of using `--link`

Use `--link` to reuse already built layers in subsequent builds with `--cache-from` even if the previous layers have changed. This is especially important for multi-stage builds where a `COPY --from` statement would previously get invalidated if any previous commands in the same stage changed, causing the need to rebuild the intermediate stages again. With `--link` the layer the previous build generated is reused and merged on top of the new layers. This also means you can easily rebase your images when the base images receive updates, without having to execute the whole build again. In backends that support it, BuildKit can do this rebase action without the need to push or pull any layers between the client and the registry. BuildKit will detect this case and only create new image manifest that contains the new layers and old layers in correct order.

​	使用 `--link` 即使在前一层已更改时，仍可在随后的构建中使用 `--cache-from` 复用已构建的层。此功能在多阶段构建中尤为重要，以前 `COPY --from` 语句会因为同一阶段中的任意命令更改而失效，导致需要重新构建中间阶段。使用 `--link`，之前构建生成的层会被复用并合并到新层上。这意味着您可以轻松地在基础镜像收到更新时重新基线您的镜像，而无需重新执行整个构建过程。在支持该功能的后端中，BuildKit 可以执行此重新基线操作，而无需在客户端和注册表之间推送或拉取任何层。BuildKit 会检测这种情况，并仅创建包含新层和旧层的正确顺序的新镜像清单。

The same behavior where BuildKit can avoid pulling down the base image can also happen when using `--link` and no other commands that would require access to the files in the base image. In that case BuildKit will only build the layers for the `COPY` commands and push them to the registry directly on top of the layers of the base image.

​	在不需要访问基础镜像中文件的情况下，BuildKit 还可以避免拉取基础镜像。在这种情况下，BuildKit 仅会构建 `COPY` 指令的层并将其直接推送到基础镜像层之上。

#### `--link=false` 的不兼容性 Incompatibilities with `--link=false`

When using `--link` the `COPY/ADD` commands are not allowed to read any files from the previous state. This means that if in previous state the destination directory was a path that contained a symlink, `COPY/ADD` can not follow it. In the final image the destination path created with `--link` will always be a path containing only directories.

​	使用 `--link` 时，不允许 `COPY/ADD` 命令读取前一状态中的任何文件。如果在前一状态中，目标目录路径包含符号链接，`COPY/ADD` 将无法跟随该链接。在最终镜像中，使用 `--link` 创建的目标路径将始终是仅包含目录的路径。

If you don't rely on the behavior of following symlinks in the destination path, using `--link` is always recommended. The performance of `--link` is equivalent or better than the default behavior and, it creates much better conditions for cache reuse.

​	如果您不依赖于目标路径中跟随符号链接的行为，建议始终使用 `--link`。`--link` 的性能等同于或优于默认行为，并且可以创建更好的缓存重用条件。

### `COPY --parents`

> **Note**
>
> Not yet available in stable syntax, use [`docker/dockerfile:1.7-labs`](#syntax) version.
>
> ​	尚未在稳定语法中可用，请使用 [`docker/dockerfile:1.7-labs`](#syntax) 版本。



```dockerfile
COPY [--parents[=<boolean>]] <src> ... <dest>
```

The `--parents` flag preserves parent directories for `src` entries. This flag defaults to `false`.

​	`--parents` 标志可保留 `src` 条目的父目录。该标志默认为 `false`。



```dockerfile
# syntax=docker/dockerfile:1.7-labs
FROM scratch

COPY ./x/a.txt ./y/a.txt /no_parents/
COPY --parents ./x/a.txt ./y/a.txt /parents/

# /no_parents/a.txt
# /parents/x/a.txt
# /parents/y/a.txt
```

This behavior is similar to the [Linux `cp` utility's](https://www.man7.org/linux/man-pages/man1/cp.1.html) `--parents` or [`rsync`](https://man7.org/linux/man-pages/man1/rsync.1.html) `--relative` flag.

​	此行为类似于 [Linux `cp` 工具的](https://www.man7.org/linux/man-pages/man1/cp.1.html) `--parents` 或 [`rsync`](https://man7.org/linux/man-pages/man1/rsync.1.html) 的 `--relative` 标志。

As with Rsync, it is possible to limit which parent directories are preserved by inserting a dot and a slash (`./`) into the source path. If such point exists, only parent directories after it will be preserved. This may be especially useful copies between stages with `--from` where the source paths need to be absolute.

​	与 Rsync 类似，可以在源路径中插入一个点和斜杠（`./`）来限制保留的父目录。这样在不同阶段间使用 `--from` 进行复制时尤其有用，因为源路径需要是绝对路径。



```dockerfile
# syntax=docker/dockerfile:1.7-labs
FROM scratch

COPY --parents ./x/./y/*.txt /parents/

# Build context:
# ./x/y/a.txt
# ./x/y/b.txt
#
# Output:
# /parents/y/a.txt
# /parents/y/b.txt
```

Note that, without the `--parents` flag specified, any filename collision will fail the Linux `cp` operation with an explicit error message (`cp: will not overwrite just-created './x/a.txt' with './y/a.txt'`), where the Buildkit will silently overwrite the target file at the destination.

​	请注意，如果未指定 `--parents` 标志，任何文件名冲突都会导致 Linux `cp` 操作失败并显示明确的错误信息，而 Buildkit 将会默默地覆盖目标路径中的文件。

While it is possible to preserve the directory structure for `COPY` instructions consisting of only one `src` entry, usually it is more beneficial to keep the layer count in the resulting image as low as possible. Therefore, with the `--parents` flag, the Buildkit is capable of packing multiple `COPY` instructions together, keeping the directory structure intact.

​	虽然可以在 `COPY` 指令中仅包含一个 `src` 条目时保留目录结构，但通常保持结果镜像的层数尽可能少更为有利。因此，使用 `--parents` 标志时，Buildkit 可以将多个 `COPY` 指令打包在一起，从而保留目录结构。

### COPY --exclude

> **Note**
>
> Not yet available in stable syntax, use [`docker/dockerfile:1.7-labs`](#syntax) version.
>
> ​	尚未在稳定语法中可用，请使用 [`docker/dockerfile:1.7-labs`](#syntax) 版本。



```dockerfile
COPY [--exclude=<path> ...] <src> ... <dest>
```

The `--exclude` flag lets you specify a path expression for files to be excluded.

​	`--exclude` 标志允许您指定一个路径表达式来排除文件。

The path expression follows the same format as `<src>`, supporting wildcards and matching using Go's [filepath.Match](https://golang.org/pkg/path/filepath#Match) rules. For example, to add all files starting with "hom", excluding files with a `.txt` extension:

​	路径表达式遵循与 `<src>` 相同的格式，支持通配符并使用 Go 的 [filepath.Match](https://golang.org/pkg/path/filepath#Match) 规则。例如，添加所有以 "hom" 开头的文件，排除 `.txt` 扩展名的文件：



```dockerfile
COPY --exclude=*.txt hom* /mydir/
```

You can specify the `--exclude` option multiple times for a `COPY` instruction. Multiple `--excludes` are files matching its patterns not to be copied, even if the files paths match the pattern specified in `<src>`. To add all files starting with "hom", excluding files with either `.txt` or `.md` extensions:

​	您可以在 `COPY` 指令中多次指定 `--exclude` 选项。多个 `--exclude` 将排除匹配其模式的文件，即使这些文件路径与 `<src>` 中的模式匹配。要添加所有以 "hom" 开头的文件，排除 `.txt` 或 `.md` 扩展名的文件：



```dockerfile
COPY --exclude=*.txt --exclude=*.md hom* /mydir/
```

## ENTRYPOINT

An `ENTRYPOINT` allows you to configure a container that will run as an executable.

​	`ENTRYPOINT` 指令允许您配置一个作为可执行文件运行的容器。

`ENTRYPOINT` has two possible forms:

​	`ENTRYPOINT` 有两种形式：

- The exec form, which is the preferred form: 推荐的执行形式：

  

  ```dockerfile
  ENTRYPOINT ["executable", "param1", "param2"]
  ```

- The shell form: Shell 形式：

  

  ```dockerfile
  ENTRYPOINT command param1 param2
  ```

For more information about the different forms, see [Shell and exec form](#shell-and-exec-form).

​	关于不同形式的更多信息，请参见 [Shell 和 exec 形式](#shell-and-exec-form)。

The following command starts a container from the `nginx` with its default content, listening on port 80:

​	以下命令启动一个带有默认内容的 `nginx` 容器，并在端口 80 上侦听：

```console
$ docker run -i -t --rm -p 80:80 nginx
```

Command line arguments to `docker run <image>` will be appended after all elements in an exec form `ENTRYPOINT`, and will override all elements specified using `CMD`.

​	命令行参数会追加到 exec 形式的 `ENTRYPOINT` 指令后，并会覆盖通过 `CMD` 指定的所有元素。

This allows arguments to be passed to the entry point, i.e., `docker run <image> -d` will pass the `-d` argument to the entry point. You can override the `ENTRYPOINT` instruction using the `docker run --entrypoint` flag.

​	这允许将参数传递给入口点，例如 `docker run <image> -d` 会将 `-d` 参数传递给入口点。您可以使用 `docker run --entrypoint` 标志覆盖 `ENTRYPOINT` 指令。

The shell form of `ENTRYPOINT` prevents any `CMD` command line arguments from being used. It also starts your `ENTRYPOINT` as a subcommand of `/bin/sh -c`, which does not pass signals. This means that the executable will not be the container's `PID 1`, and will not receive Unix signals. In this case, your executable doesn't receive a `SIGTERM` from `docker stop <container>`.

​	使用 `ENTRYPOINT` 的 shell 形式会阻止任何 `CMD` 命令行参数的使用，并且会将 `ENTRYPOINT` 作为 `/bin/sh -c` 的子命令启动，不会传递信号。这意味着可执行文件不会成为容器的 `PID 1`，也不会接收 Unix 信号。在这种情况下，您的可执行文件不会从 `docker stop <container>` 收到 `SIGTERM`。

Only the last `ENTRYPOINT` instruction in the Dockerfile will have an effect.

​	Dockerfile 中仅最后一个 `ENTRYPOINT` 指令有效。

### Exec 形式的 ENTRYPOINT 示例 Exec form ENTRYPOINT example

You can use the exec form of `ENTRYPOINT` to set fairly stable default commands and arguments and then use either form of `CMD` to set additional defaults that are more likely to be changed.

​	您可以使用 `ENTRYPOINT` 的 exec 形式设置较稳定的默认命令和参数，并使用任意形式的 `CMD` 设置更易更改的附加默认值。



```dockerfile
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]
```

When you run the container, you can see that `top` is the only process:

​	运行容器时，您可以看到 `top` 是唯一的进程：

```console
$ docker run -it --rm --name test  top -H

top - 08:25:00 up  7:27,  0 users,  load average: 0.00, 0.01, 0.05
Threads:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.1 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:   2056668 total,  1616832 used,   439836 free,    99352 buffers
KiB Swap:  1441840 total,        0 used,  1441840 free.  1324440 cached Mem

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
    1 root      20   0   19744   2336   2080 R  0.0  0.1   0:00.04 top
```

To examine the result further, you can use `docker exec`:

​	可以使用 `docker exec` 进一步检查结果：

```console
$ docker exec -it test ps aux

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  2.6  0.1  19752  2352 ?        Ss+  08:24   0:00 top -b -H
root         7  0.0  0.1  15572  2164 ?        R+   08:25   0:00 ps aux
```

And you can gracefully request `top` to shut down using `docker stop test`.

​	您可以使用 `docker stop test` 友好地请求 `top` 关闭。

The following Dockerfile shows using the `ENTRYPOINT` to run Apache in the foreground (i.e., as `PID 1`):

​	以下 Dockerfile 示例展示了如何使用 `ENTRYPOINT` 使 Apache 以前台模式运行（即作为 `PID 1`）：

```dockerfile
FROM debian:stable
RUN apt-get update && apt-get install -y --force-yes apache2
EXPOSE 80 443
VOLUME ["/var/www", "/var/log/apache2", "/etc/apache2"]
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

If you need to write a starter script for a single executable, you can ensure that the final executable receives the Unix signals by using `exec` and `gosu` commands:

​	如果您需要为单个可执行文件编写启动脚本，可以通过 `exec` 和 `gosu` 命令确保最终的可执行文件接收 Unix 信号：

```bash
#!/usr/bin/env bash
set -e

if [ "$1" = 'postgres' ]; then
    chown -R postgres "$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"
fi

exec "$@"
```

Lastly, if you need to do some extra cleanup (or communicate with other containers) on shutdown, or are co-ordinating more than one executable, you may need to ensure that the `ENTRYPOINT` script receives the Unix signals, passes them on, and then does some more work:

​	最后，如果您需要在关闭时执行一些额外的清理操作（或与其他容器通信），或者协调多个可执行文件，可以确保 `ENTRYPOINT` 脚本接收 Unix 信号，传递它们并进行进一步操作。

```bash
#!/bin/sh
# Note: I've written this using sh so it works in the busybox container too

# USE the trap if you need to also do manual cleanup after the service is stopped,
#     or need to start multiple services in the one container
trap "echo TRAPed signal" HUP INT QUIT TERM

# start service in background here
/usr/sbin/apachectl start

echo "[hit enter key to exit] or run 'docker stop <container>'"
read

# stop service and clean up here
echo "stopping apache"
/usr/sbin/apachectl stop

echo "exited $0"
```

If you run this image with `docker run -it --rm -p 80:80 --name test apache`, you can then examine the container's processes with `docker exec`, or `docker top`, and then ask the script to stop Apache:

​	如果你使用 `docker run -it --rm -p 80:80 --name test apache` 运行此镜像，可以通过 `docker exec` 或 `docker top` 检查容器的进程，然后请求脚本停止 Apache：

```console
$ docker exec -it test ps aux

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.1  0.0   4448   692 ?        Ss+  00:42   0:00 /bin/sh /run.sh 123 cmd cmd2
root        19  0.0  0.2  71304  4440 ?        Ss   00:42   0:00 /usr/sbin/apache2 -k start
www-data    20  0.2  0.2 360468  6004 ?        Sl   00:42   0:00 /usr/sbin/apache2 -k start
www-data    21  0.2  0.2 360468  6000 ?        Sl   00:42   0:00 /usr/sbin/apache2 -k start
root        81  0.0  0.1  15572  2140 ?        R+   00:44   0:00 ps aux

$ docker top test

PID                 USER                COMMAND
10035               root                {run.sh} /bin/sh /run.sh 123 cmd cmd2
10054               root                /usr/sbin/apache2 -k start
10055               33                  /usr/sbin/apache2 -k start
10056               33                  /usr/sbin/apache2 -k start

$ /usr/bin/time docker stop test

test
real	0m 0.27s
user	0m 0.03s
sys	0m 0.03s
```

> **Note**
>
> You can override the `ENTRYPOINT` setting using `--entrypoint`, but this can only set the binary to exec (no `sh -c` will be used).
>
> ​	可以使用 `--entrypoint` 覆盖 `ENTRYPOINT` 设置，但只能设置要执行的二进制文件（不会使用 `sh -c`）。

### Shell 形式 ENTRYPOINT 示例 Shell form ENTRYPOINT example

You can specify a plain string for the `ENTRYPOINT` and it will execute in `/bin/sh -c`. This form will use shell processing to substitute shell environment variables, and will ignore any `CMD` or `docker run` command line arguments. To ensure that `docker stop` will signal any long running `ENTRYPOINT` executable correctly, you need to remember to start it with `exec`:

​	可以为 `ENTRYPOINT` 指定一个普通字符串，它将在 `/bin/sh -c` 中执行。此形式将使用 Shell 处理来替换 Shell 环境变量，并忽略任何 `CMD` 或 `docker run` 的命令行参数。为了确保 `docker stop` 正确传递信号给运行时间较长的 `ENTRYPOINT` 可执行文件，需要记得以 `exec` 启动它：



```dockerfile
FROM ubuntu
ENTRYPOINT exec top -b
```

When you run this image, you'll see the single `PID 1` process:

​	运行此镜像时，会看到单个 `PID 1` 进程：

```console
$ docker run -it --rm --name test top

Mem: 1704520K used, 352148K free, 0K shrd, 0K buff, 140368121167873K cached
CPU:   5% usr   0% sys   0% nic  94% idle   0% io   0% irq   0% sirq
Load average: 0.08 0.03 0.05 2/98 6
  PID  PPID USER     STAT   VSZ %VSZ %CPU COMMAND
    1     0 root     R     3164   0%   0% top -b
```

Which exits cleanly on `docker stop`:

​	在 `docker stop` 时干净退出：

```console
$ /usr/bin/time docker stop test

test
real	0m 0.20s
user	0m 0.02s
sys	0m 0.04s
```

If you forget to add `exec` to the beginning of your `ENTRYPOINT`:

​	如果忘了在 `ENTRYPOINT` 的开头添加 `exec`：

```dockerfile
FROM ubuntu
ENTRYPOINT top -b
CMD -- --ignored-param1
```

You can then run it (giving it a name for the next step):

​	然后可以运行它（赋予名称，以便下一步操作）：

```console
$ docker run -it --name test top --ignored-param2

top - 13:58:24 up 17 min,  0 users,  load average: 0.00, 0.00, 0.00
Tasks:   2 total,   1 running,   1 sleeping,   0 stopped,   0 zombie
%Cpu(s): 16.7 us, 33.3 sy,  0.0 ni, 50.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   1990.8 total,   1354.6 free,    231.4 used,    404.7 buff/cache
MiB Swap:   1024.0 total,   1024.0 free,      0.0 used.   1639.8 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
    1 root      20   0    2612    604    536 S   0.0   0.0   0:00.02 sh
    6 root      20   0    5956   3188   2768 R   0.0   0.2   0:00.00 top
```

You can see from the output of `top` that the specified `ENTRYPOINT` is not `PID 1`.

​	从 `top` 的输出可以看出指定的 `ENTRYPOINT` 不是 `PID 1`。

If you then run `docker stop test`, the container will not exit cleanly - the `stop` command will be forced to send a `SIGKILL` after the timeout:

​	然后运行 `docker stop test`，容器不会正常退出——在超时后 `stop` 命令将被迫发送 `SIGKILL`：

```console
$ docker exec -it test ps waux

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.4  0.0   2612   604 pts/0    Ss+  13:58   0:00 /bin/sh -c top -b --ignored-param2
root         6  0.0  0.1   5956  3188 pts/0    S+   13:58   0:00 top -b
root         7  0.0  0.1   5884  2816 pts/1    Rs+  13:58   0:00 ps waux

$ /usr/bin/time docker stop test

test
real	0m 10.19s
user	0m 0.04s
sys	0m 0.03s
```

### 理解 CMD 和 ENTRYPOINT 的交互 Understand how CMD and ENTRYPOINT interact

Both `CMD` and `ENTRYPOINT` instructions define what command gets executed when running a container. There are few rules that describe their co-operation.

​	`CMD` 和 `ENTRYPOINT` 指令共同定义了容器运行时执行的命令，以下规则描述了它们的配合：

1. Dockerfile should specify at least one of `CMD` or `ENTRYPOINT` commands. Dockerfile 中应至少指定一个 `CMD` 或 `ENTRYPOINT`。
2. `ENTRYPOINT` should be defined when using the container as an executable. 当使用容器作为可执行文件时应定义 `ENTRYPOINT`。
3. `CMD` should be used as a way of defining default arguments for an `ENTRYPOINT` command or for executing an ad-hoc command in a container. `CMD` 用于为 `ENTRYPOINT` 命令定义默认参数，或在容器中执行临时命令。
4. `CMD` will be overridden when running the container with alternative arguments. 当使用替代参数运行容器时，`CMD` 会被覆盖。

The table below shows what command is executed for different `ENTRYPOINT` / `CMD` combinations:

​	下表显示了不同 `ENTRYPOINT` / `CMD` 组合执行的命令：

|                                | No ENTRYPOINT              | ENTRYPOINT exec_entry p1_entry | ENTRYPOINT ["exec_entry", "p1_entry"]          |
| :----------------------------- | :------------------------- | :----------------------------- | :--------------------------------------------- |
| **No CMD**                     | error, not allowed         | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry                            |
| **CMD ["exec_cmd", "p1_cmd"]** | exec_cmd p1_cmd            | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry exec_cmd p1_cmd            |
| **CMD exec_cmd p1_cmd**        | /bin/sh -c exec_cmd p1_cmd | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry /bin/sh -c exec_cmd p1_cmd |

> **Note**
>
> If `CMD` is defined from the base image, setting `ENTRYPOINT` will reset `CMD` to an empty value. In this scenario, `CMD` must be defined in the current image to have a value.
>
> ​	如果从基础镜像定义了 `CMD`，则设置 `ENTRYPOINT` 会将 `CMD` 重置为空值。在这种情况下，必须在当前镜像中定义 `CMD` 才能有效。

## VOLUME



```dockerfile
VOLUME ["/data"]
```

The `VOLUME` instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers. The value can be a JSON array, `VOLUME ["/var/log/"]`, or a plain string with multiple arguments, such as `VOLUME /var/log` or `VOLUME /var/log /var/db`. For more information/examples and mounting instructions via the Docker client, refer to [*Share Directories via Volumes*](https://docs.docker.com/storage/volumes/) documentation.

​	`VOLUME` 指令创建一个具有指定名称的挂载点，并将其标记为用于从主机或其他容器进行外部挂载。值可以是 JSON 数组，如 `VOLUME ["/var/log/"]`，或带有多个参数的普通字符串，如 `VOLUME /var/log /var/db`。更多信息和挂载说明请参考 [*通过卷共享目录*](https://docs.docker.com/storage/volumes/) 文档。

The `docker run` command initializes the newly created volume with any data that exists at the specified location within the base image. For example, consider the following Dockerfile snippet:

​	`docker run` 命令在创建的新卷中初始化基础镜像指定位置的任何数据。以下 Dockerfile 代码片段为例：



```dockerfile
FROM ubuntu
RUN mkdir /myvol
RUN echo "hello world" > /myvol/greeting
VOLUME /myvol
```

This Dockerfile results in an image that causes `docker run` to create a new mount point at `/myvol` and copy the `greeting` file into the newly created volume.

​	此 Dockerfile 生成的镜像会在 `docker run` 时于 `/myvol` 创建一个新的挂载点，并将 `greeting` 文件复制到新创建的卷中。

### 关于指定卷的注意事项 Notes about specifying volumes

Keep the following things in mind about volumes in the Dockerfile.

​	关于 Dockerfile 中的卷需要注意以下几点：

- **Volumes on Windows-based containers**: When using Windows-based containers, the destination of a volume inside the container must be one of: **基于 Windows 容器的卷**：当使用基于 Windows 的容器时，卷在容器内的目标必须是以下之一：
  - a non-existing or empty directory 一个不存在或为空的目录
  - a drive other than `C:` 驱动器（例如 `C:` 以外的其他驱动器）
- **Changing the volume from within the Dockerfile**: If any build steps change the data within the volume after it has been declared, those changes will be discarded. **在 Dockerfile 中更改卷**：如果任何构建步骤在声明卷后更改其内的数据，这些更改将被丢弃。
- **JSON formatting**: The list is parsed as a JSON array. You must enclose words with double quotes (`"`) rather than single quotes (`'`). **JSON 格式**：列表会作为 JSON 数组进行解析。必须使用双引号（`"`）而不是单引号（`'`）来包含单词。
- **The host directory is declared at container run-time**: The host directory (the mountpoint) is, by its nature, host-dependent. This is to preserve image portability, since a given host directory can't be guaranteed to be available on all hosts. For this reason, you can't mount a host directory from within the Dockerfile. The `VOLUME` instruction does not support specifying a `host-dir` parameter. You must specify the mountpoint when you create or run the container. **主机目录在容器运行时声明**：主机目录（挂载点）依赖于主机。这是为了保持镜像的可移植性，因为不能保证给定的主机目录在所有主机上都可用。因此，不能在 Dockerfile 中从主机挂载目录。`VOLUME` 指令不支持指定 `host-dir` 参数。必须在创建或运行容器时指定挂载点。

## USER



```dockerfile
USER <user>[:<group>]
```

or



```dockerfile
USER UID[:GID]
```

The `USER` instruction sets the user name (or UID) and optionally the user group (or GID) to use as the default user and group for the remainder of the current stage. The specified user is used for `RUN` instructions and at runtime, runs the relevant `ENTRYPOINT` and `CMD` commands.

​	`USER` 指令设置用户名（或 UID）和可选的用户组（或 GID），用于当前阶段的剩余部分作为默认用户和组。指定的用户用于 `RUN` 指令，并在运行时执行相关的 `ENTRYPOINT` 和 `CMD` 命令。

> Note that when specifying a group for the user, the user will have *only* the specified group membership. Any other configured group memberships will be ignored.
>
> ​	请注意，当为用户指定一个组时，该用户将*仅*具有指定的组成员身份，其他配置的组成员身份将被忽略。

> **Warning**
>
> When the user doesn't have a primary group then the image (or the next instructions) will be run with the `root` group.
>
> ​	当用户没有主要组时，镜像（或后续指令）将以 `root` 组运行。
>
> On Windows, the user must be created first if it's not a built-in account. This can be done with the `net user` command called as part of a Dockerfile.
>
> ​	在 Windows 上，如果用户不是内置账户，必须先创建。可以通过 Dockerfile 中的 `net user` 命令完成。



```dockerfile
FROM microsoft/windowsservercore
# Create Windows user in the container
RUN net user /add patrick
# Set it for subsequent commands
USER patrick
```

## WORKDIR



```dockerfile
WORKDIR /path/to/workdir
```

The `WORKDIR` instruction sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions that follow it in the Dockerfile. If the `WORKDIR` doesn't exist, it will be created even if it's not used in any subsequent Dockerfile instruction.

​	`WORKDIR` 指令设置 Dockerfile 中后续 `RUN`、`CMD`、`ENTRYPOINT`、`COPY` 和 `ADD` 指令的工作目录。如果 `WORKDIR` 不存在，将会被创建，即使在后续 Dockerfile 指令中未使用它。

The `WORKDIR` instruction can be used multiple times in a Dockerfile. If a relative path is provided, it will be relative to the path of the previous `WORKDIR` instruction. For example:

​	`WORKDIR` 指令可以在 Dockerfile 中多次使用。如果提供相对路径，它将相对于上一个 `WORKDIR` 指令的路径。例如：



```dockerfile
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```

The output of the final `pwd` command in this Dockerfile would be `/a/b/c`.

​	最终 `pwd` 命令在这个 Dockerfile 中的输出将是 `/a/b/c`。

The `WORKDIR` instruction can resolve environment variables previously set using `ENV`. You can only use environment variables explicitly set in the Dockerfile. For example:

​	`WORKDIR` 指令可以解析先前通过 `ENV` 设置的环境变量。你只能使用在 Dockerfile 中显式设置的环境变量。例如：



```dockerfile
ENV DIRPATH=/path
WORKDIR $DIRPATH/$DIRNAME
RUN pwd
```

The output of the final `pwd` command in this Dockerfile would be `/path/$DIRNAME`

​	在这个 Dockerfile 中，最终 `pwd` 命令的输出将是 `/path/$DIRNAME`。

If not specified, the default working directory is `/`. In practice, if you aren't building a Dockerfile from scratch (`FROM scratch`), the `WORKDIR` may likely be set by the base image you're using.

​	如果没有指定，默认工作目录是 `/`。实际上，如果你不是从头开始构建 Dockerfile（`FROM scratch`），那么你使用的基础镜像可能已经设置了 `WORKDIR`。

Therefore, to avoid unintended operations in unknown directories, it's best practice to set your `WORKDIR` explicitly.

​	因此，为避免在未知目录中执行不必要的操作，最好显式设置你的 `WORKDIR`。

## ARG



```dockerfile
ARG <name>[=<default value>]
```

The `ARG` instruction defines a variable that users can pass at build-time to the builder with the `docker build` command using the `--build-arg <varname>=<value>` flag.

​	`ARG` 指令定义了一个变量，用户可以通过在 `docker build` 命令中使用 `--build-arg <varname>=<value>` 标志在构建时传递给构建器。

> **Warning**
>
> It isn't recommended to use build arguments for passing secrets such as user credentials, API tokens, etc. Build arguments are visible in the `docker history` command and in `max` mode provenance attestations, which are attached to the image by default if you use the Buildx GitHub Actions and your GitHub repository is public.
>
> ​	不建议使用构建参数传递诸如用户凭据、API 令牌等秘密信息。构建参数在 `docker history` 命令和 `max` 模式的溯源证明中是可见的。如果你使用 Buildx GitHub Actions 并且 GitHub 仓库是公开的，这些信息将默认附加到镜像上。
>
> Refer to the [`RUN --mount=type=secret`](#run---mounttypesecret) section to learn about secure ways to use secrets when building images.
>
> ​	请参阅 [`RUN --mount=type=secret`](#run---mounttypesecret) 部分，了解构建镜像时使用秘密的安全方法。

A Dockerfile may include one or more `ARG` instructions. For example, the following is a valid Dockerfile:

​	Dockerfile 可以包含一个或多个 `ARG` 指令。例如，以下是一个有效的 Dockerfile：



```dockerfile
FROM busybox
ARG user1
ARG buildno
# ...
```

### 默认值 Default values

An `ARG` instruction can optionally include a default value:

​	`ARG` 指令可以选择性地包含默认值：

```dockerfile
FROM busybox
ARG user1=someuser
ARG buildno=1
# ...
```

If an `ARG` instruction has a default value and if there is no value passed at build-time, the builder uses the default.

​	如果 `ARG` 指令具有默认值并且在构建时没有传递任何值，则构建器使用默认值。

### Scope

An `ARG` variable definition comes into effect from the line on which it is defined in the Dockerfile not from the argument's use on the command-line or elsewhere. For example, consider this Dockerfile:

​	`ARG` 变量定义从 Dockerfile 中定义它的行开始生效，而不是从命令行或其他位置使用该参数的地方开始生效。例如，考虑这个 Dockerfile：



```dockerfile
FROM busybox
USER ${username:-some_user}
ARG username
USER $username
# ...
```

A user builds this file by calling:

​	用户通过以下命令构建该文件：



```console
$ docker build --build-arg username=what_user .
```

The `USER` at line 2 evaluates to `some_user` as the `username` variable is defined on the subsequent line 3. The `USER` at line 4 evaluates to `what_user`, as the `username` argument is defined and the `what_user` value was passed on the command line. Prior to its definition by an `ARG` instruction, any use of a variable results in an empty string.

​	第 2 行的 `USER` 评估为 `some_user`，因为 `username` 变量在第 3 行才被定义。第 4 行的 `USER` 评估为 `what_user`，因为 `username` 参数已定义，且在命令行中传递了 `what_user` 值。在 `ARG` 指令定义之前，使用变量会导致结果为空字符串。

An `ARG` instruction goes out of scope at the end of the build stage where it was defined. To use an argument in multiple stages, each stage must include the `ARG` instruction.

​	`ARG` 指令在其定义的构建阶段结束时失效。要在多个阶段中使用参数，每个阶段都必须包含 `ARG` 指令。



```dockerfile
FROM busybox
ARG SETTINGS
RUN ./run/setup $SETTINGS

FROM busybox
ARG SETTINGS
RUN ./run/other $SETTINGS
```

### 使用 ARG 变量 Using ARG variables

You can use an `ARG` or an `ENV` instruction to specify variables that are available to the `RUN` instruction. Environment variables defined using the `ENV` instruction always override an `ARG` instruction of the same name. Consider this Dockerfile with an `ENV` and `ARG` instruction.

​	可以使用 `ARG` 或 `ENV` 指令指定可用于 `RUN` 指令的变量。使用 `ENV` 指令定义的环境变量始终会覆盖同名的 `ARG` 指令。例如：

```dockerfile
FROM ubuntu
ARG CONT_IMG_VER
ENV CONT_IMG_VER=v1.0.0
RUN echo $CONT_IMG_VER
```

Then, assume this image is built with this command:

​	假设使用以下命令构建此镜像：

```console
$ docker build --build-arg CONT_IMG_VER=v2.0.1 .
```

In this case, the `RUN` instruction uses `v1.0.0` instead of the `ARG` setting passed by the user:`v2.0.1` This behavior is similar to a shell script where a locally scoped variable overrides the variables passed as arguments or inherited from environment, from its point of definition.

​	在这种情况下，`RUN` 指令使用 `v1.0.0` 而不是用户传递的 `ARG` 设置 `v2.0.1`。这种行为类似于 Shell 脚本中局部变量覆盖命令行或环境中的变量。

Using the example above but a different `ENV` specification you can create more useful interactions between `ARG` and `ENV` instructions:

​	使用上例但更改 `ENV` 指令可以创建更有用的 `ARG` 和 `ENV` 交互：

```dockerfile
FROM ubuntu
ARG CONT_IMG_VER
ENV CONT_IMG_VER=${CONT_IMG_VER:-v1.0.0}
RUN echo $CONT_IMG_VER
```

Unlike an `ARG` instruction, `ENV` values are always persisted in the built image. Consider a docker build without the `--build-arg` flag:

​	与 `ARG` 指令不同，`ENV` 值始终在构建的镜像中持久化。例如未使用 `--build-arg` 标志的构建：



```console
$ docker build .
```

Using this Dockerfile example, `CONT_IMG_VER` is still persisted in the image but its value would be `v1.0.0` as it is the default set in line 3 by the `ENV` instruction.

​	在此 Dockerfile 示例中，`CONT_IMG_VER` 仍会持久化在镜像中，但其值为 `v1.0.0`，这是第 3 行的 `ENV` 指令设置的默认值。

The variable expansion technique in this example allows you to pass arguments from the command line and persist them in the final image by leveraging the `ENV` instruction. Variable expansion is only supported for [a limited set of Dockerfile instructions.](#environment-replacement)

​	此示例中的变量扩展技术允许通过 `ENV` 指令从命令行传递参数并将其持久化到最终镜像中。变量扩展仅支持[有限的 Dockerfile 指令。](#environment-replacement)

### 预定义的 ARGs - Predefined ARGs

Docker has a set of predefined `ARG` variables that you can use without a corresponding `ARG` instruction in the Dockerfile.

​	Docker 提供了一组预定义的 `ARG` 变量，无需在 Dockerfile 中对应的 `ARG` 指令即可使用。

- `HTTP_PROXY`
- `http_proxy`
- `HTTPS_PROXY`
- `https_proxy`
- `FTP_PROXY`
- `ftp_proxy`
- `NO_PROXY`
- `no_proxy`
- `ALL_PROXY`
- `all_proxy`

To use these, pass them on the command line using the `--build-arg` flag, for example:

​	使用这些变量时，可通过 `--build-arg` 标志传递。例如：



```console
$ docker build --build-arg HTTPS_PROXY=https://my-proxy.example.com .
```

By default, these pre-defined variables are excluded from the output of `docker history`. Excluding them reduces the risk of accidentally leaking sensitive authentication information in an `HTTP_PROXY` variable.

​	默认情况下，这些预定义变量会从 `docker history` 输出中排除。这减少了意外泄露 `HTTP_PROXY` 变量中敏感信息的风险。

For example, consider building the following Dockerfile using `--build-arg HTTP_PROXY=http://user:pass@proxy.lon.example.com`

​	例如，使用 `--build-arg HTTP_PROXY=http://user:pass@proxy.lon.example.com` 构建以下 Dockerfile：

```dockerfile
FROM ubuntu
RUN echo "Hello World"
```

In this case, the value of the `HTTP_PROXY` variable is not available in the `docker history` and is not cached. If you were to change location, and your proxy server changed to `http://user:pass@proxy.sfo.example.com`, a subsequent build does not result in a cache miss.

​	在这种情况下，`HTTP_PROXY` 变量的值不会在 `docker history` 中可见，也不会被缓存。如果更换位置，代理服务器更改为 `http://user:pass@proxy.sfo.example.com`，后续构建不会导致缓存未命中。

If you need to override this behaviour then you may do so by adding an `ARG` statement in the Dockerfile as follows:

​	如果需要覆盖此行为，可以在 Dockerfile 中添加 `ARG` 声明，如下所示：



```dockerfile
FROM ubuntu
ARG HTTP_PROXY
RUN echo "Hello World"
```

When building this Dockerfile, the `HTTP_PROXY` is preserved in the `docker history`, and changing its value invalidates the build cache.

​	构建此 Dockerfile 时，`HTTP_PROXY` 会保存在 `docker history` 中，并且更改其值会使构建缓存失效。

### 全局范围内的自动平台 ARGs - Automatic platform ARGs in the global scope

This feature is only available when using the [BuildKit]({{< ref "/manuals/DockerBuild/BuildKit" >}}) backend.

​	此功能仅在使用 [BuildKit]({{< ref "/manuals/DockerBuild/BuildKit" >}}) 后端时可用。

BuildKit supports a predefined set of `ARG` variables with information on the platform of the node performing the build (build platform) and on the platform of the resulting image (target platform). The target platform can be specified with the `--platform` flag on `docker build`.

​	BuildKit 支持一组预定义的 `ARG` 变量，包含有关执行构建的节点平台（构建平台）和生成的镜像平台（目标平台）的信息。可以使用 `docker build` 的 `--platform` 标志指定目标平台。

The following `ARG` variables are set automatically:

​	以下 `ARG` 变量会自动设置：

- `TARGETPLATFORM` - platform of the build result. Eg `linux/amd64`, `linux/arm/v7`, `windows/amd64`.
  - `TARGETPLATFORM` - 构建结果的平台。例如 `linux/amd64`、`linux/arm/v7`、`windows/amd64`。

- `TARGETOS` - OS component of TARGETPLATFORM
  - `TARGETOS` - TARGETPLATFORM 的操作系统组件

- `TARGETARCH` - architecture component of TARGETPLATFORM
  - `TARGETARCH` - TARGETPLATFORM 的架构组件

- `TARGETVARIANT` - variant component of TARGETPLATFORM
  - `TARGETVARIANT` - TARGETPLATFORM 的变体组件

- `BUILDPLATFORM` - platform of the node performing the build.
  - `BUILDPLATFORM` - 执行构建的节点的平台。

- `BUILDOS` - OS component of BUILDPLATFORM
  - `BUILDOS` - BUILDPLATFORM 的操作系统组件

- `BUILDARCH` - architecture component of BUILDPLATFORM
  - `BUILDARCH` - BUILDPLATFORM 的架构组件

- `BUILDVARIANT` - variant component of BUILDPLATFORM
  - `BUILDVARIANT` - BUILDPLATFORM 的变体组件


These arguments are defined in the global scope so are not automatically available inside build stages or for your `RUN` commands. To expose one of these arguments inside the build stage redefine it without value.

​	这些参数在全局范围内定义，因此在构建阶段或 `RUN` 命令中无法自动使用。要在构建阶段公开这些参数之一，请重新定义它而不设置值。

For example: 

​	例如：

```dockerfile
FROM alpine
ARG TARGETPLATFORM
RUN echo "I'm building for $TARGETPLATFORM"
```

### BuildKit 内置构建参数 BuildKit built-in build args

| Arg                               | Type 类型 | Description 描述                                             |
| --------------------------------- | --------- | ------------------------------------------------------------ |
| `BUILDKIT_CACHE_MOUNT_NS`         | String    | Set optional cache ID namespace. 设置可选的缓存 ID 命名空间。 |
| `BUILDKIT_CONTEXT_KEEP_GIT_DIR`   | Bool      | Trigger Git context to keep the `.git` directory. 触发 Git 上下文保留 `.git` 目录。 |
| `BUILDKIT_INLINE_CACHE`[2](#fn:2) | Bool      | Inline cache metadata to image config or not. 内联缓存元数据到镜像配置。 |
| `BUILDKIT_MULTI_PLATFORM`         | Bool      | Opt into deterministic output regardless of multi-platform output or not. 是否启用多平台输出的确定性输出。 |
| `BUILDKIT_SANDBOX_HOSTNAME`       | String    | Set the hostname (default `buildkitsandbox`) 设置主机名（默认 `buildkitsandbox`） |
| `BUILDKIT_SYNTAX`                 | String    | Set frontend image 设置前端镜像                              |
| `SOURCE_DATE_EPOCH`               | Int       | Set the Unix timestamp for created image and layers. More info from [reproducible builds](https://reproducible-builds.org/docs/source-date-epoch/). Supported since Dockerfile 1.5, BuildKit 0.11  设置镜像和层的创建 Unix 时间戳。详细信息见[可重现构建](https://reproducible-builds.org/docs/source-date-epoch/)。支持 Dockerfile 1.5、BuildKit 0.11 |

#### 示例：保留 `.git` 目录 Example: keep `.git` dir

When using a Git context, `.git` dir is not kept on checkouts. It can be useful to keep it around if you want to retrieve git information during your build:

​	使用 Git 上下文时，`.git` 目录在检出时不会保留。如果你希望在构建过程中获取 Git 信息，可以选择保留它：

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
WORKDIR /src
RUN --mount=target=. \
  make REVISION=$(git rev-parse HEAD) build
```



```console
$ docker build --build-arg BUILDKIT_CONTEXT_KEEP_GIT_DIR=1 https://github.com/user/repo.git#main
```

### 构建缓存的影响 Impact on build caching

`ARG` variables are not persisted into the built image as `ENV` variables are. However, `ARG` variables do impact the build cache in similar ways. If a Dockerfile defines an `ARG` variable whose value is different from a previous build, then a "cache miss" occurs upon its first usage, not its definition. In particular, all `RUN` instructions following an `ARG` instruction use the `ARG` variable implicitly (as an environment variable), thus can cause a cache miss. All predefined `ARG` variables are exempt from caching unless there is a matching `ARG` statement in the Dockerfile.

​	`ARG` 变量不会像 `ENV` 变量一样保留在构建的镜像中。然而，`ARG` 变量仍会对构建缓存产生类似的影响。如果 Dockerfile 定义了一个 `ARG` 变量且其值与之前构建不同，那么在其第一次使用时（而不是定义时）会触发“缓存未命中”。尤其是所有跟在 `ARG` 指令后面的 `RUN` 指令都会隐式使用该 `ARG` 变量（作为环境变量），因此可能导致缓存未命中。所有预定义的 `ARG` 变量都不会影响缓存，除非在 Dockerfile 中有匹配的 `ARG` 声明。

For example, consider these two Dockerfile:

​	例如，考虑以下两个 Dockerfile：

```dockerfile
FROM ubuntu
ARG CONT_IMG_VER
RUN echo $CONT_IMG_VER
```



```dockerfile
FROM ubuntu
ARG CONT_IMG_VER
RUN echo hello
```

If you specify `--build-arg CONT_IMG_VER=<value>` on the command line, in both cases, the specification on line 2 doesn't cause a cache miss; line 3 does cause a cache miss. `ARG CONT_IMG_VER` causes the `RUN` line to be identified as the same as running `CONT_IMG_VER=<value> echo hello`, so if the `<value>` changes, you get a cache miss.

​	如果在命令行上指定 `--build-arg CONT_IMG_VER=<value>`，在这两种情况下，第 2 行不会导致缓存未命中；第 3 行会导致缓存未命中。`ARG CONT_IMG_VER` 将 `RUN` 行识别为运行 `CONT_IMG_VER=<value> echo hello`，因此如果 `<value>` 更改，将会导致缓存未命中。

Consider another example under the same command line:

​	另一个示例如下：

```dockerfile
FROM ubuntu
ARG CONT_IMG_VER
ENV CONT_IMG_VER=$CONT_IMG_VER
RUN echo $CONT_IMG_VER
```

In this example, the cache miss occurs on line 3. The miss happens because the variable's value in the `ENV` references the `ARG` variable and that variable is changed through the command line. In this example, the `ENV` command causes the image to include the value.

​	在此示例中，缓存未命中发生在第 3 行。因为 `ENV` 中的变量值引用了 `ARG` 变量并通过命令行发生了变化，导致该变量值发生变化并包含在镜像中。

If an `ENV` instruction overrides an `ARG` instruction of the same name, like this Dockerfile:

​	如果 `ENV` 指令覆盖了同名的 `ARG` 指令，例如以下 Dockerfile：



```dockerfile
FROM ubuntu
ARG CONT_IMG_VER
ENV CONT_IMG_VER=hello
RUN echo $CONT_IMG_VER
```

Line 3 doesn't cause a cache miss because the value of `CONT_IMG_VER` is a constant (`hello`). As a result, the environment variables and values used on the `RUN` (line 4) doesn't change between builds.

​	第 3 行不会导致缓存未命中，因为 `CONT_IMG_VER` 的值是一个常量（`hello`）。因此，`RUN` 行（第 4 行）中使用的环境变量和值在构建之间不会变化。

## ONBUILD



```dockerfile
ONBUILD INSTRUCTION
```

The `ONBUILD` instruction adds to the image a trigger instruction to be executed at a later time, when the image is used as the base for another build. The trigger will be executed in the context of the downstream build, as if it had been inserted immediately after the `FROM` instruction in the downstream Dockerfile.

​	`ONBUILD` 指令会在镜像中添加一个触发指令，稍后在该镜像用作其他构建的基础镜像时执行。触发指令会在下游构建的上下文中执行，就像它被立即插入到下游 Dockerfile 的 `FROM` 指令之后一样。

Any build instruction can be registered as a trigger.

​	任何构建指令都可以注册为触发器。

This is useful if you are building an image which will be used as a base to build other images, for example an application build environment or a daemon which may be customized with user-specific configuration.

​	这在构建将用作其他镜像基础的镜像时很有用，例如构建应用程序环境或可能需要用户特定配置的守护程序。

For example, if your image is a reusable Python application builder, it will require application source code to be added in a particular directory, and it might require a build script to be called after that. You can't just call `ADD` and `RUN` now, because you don't yet have access to the application source code, and it will be different for each application build. You could simply provide application developers with a boilerplate Dockerfile to copy-paste into their application, but that's inefficient, error-prone and difficult to update because it mixes with application-specific code.

​	例如，如果你的镜像是一个可复用的 Python 应用构建器，它需要在特定目录中添加应用源代码，还可能需要调用一个构建脚本。你不能在此时直接调用 `ADD` 和 `RUN`，因为还没有访问到应用源代码，并且每次应用构建都可能不同。你可以为应用开发人员提供一个模板 Dockerfile，以便复制到他们的应用中，但这效率低下，易出错，难以更新。

The solution is to use `ONBUILD` to register advance instructions to run later, during the next build stage.

​	解决方法是使用 `ONBUILD` 在下一个构建阶段注册提前的指令。

Here's how it works:

​	工作方式如下：

1. When it encounters an `ONBUILD` instruction, the builder adds a trigger to the metadata of the image being built. The instruction doesn't otherwise affect the current build. 当遇到 `ONBUILD` 指令时，构建器将触发器添加到正在构建的镜像的元数据中，该指令不会影响当前构建。
2. At the end of the build, a list of all triggers is stored in the image manifest, under the key `OnBuild`. They can be inspected with the `docker inspect` command. 在构建结束时，所有触发器的列表存储在镜像清单中，键为 `OnBuild`。可以通过 `docker inspect` 命令查看。
3. Later the image may be used as a base for a new build, using the `FROM` instruction. As part of processing the `FROM` instruction, the downstream builder looks for `ONBUILD` triggers, and executes them in the same order they were registered. If any of the triggers fail, the `FROM` instruction is aborted which in turn causes the build to fail. If all triggers succeed, the `FROM` instruction completes and the build continues as usual. 该镜像后来可以作为新构建的基础镜像使用，并在 `FROM` 指令处理中，查找 `ONBUILD` 触发器并按注册顺序执行。任何触发器失败将导致 `FROM` 指令中止，并使构建失败。如果所有触发器都成功，则 `FROM` 指令完成，构建继续。
4. Triggers are cleared from the final image after being executed. In other words they aren't inherited by "grand-children" builds. 触发器在执行后从最终镜像中清除，即它们不会继承给“孙”构建。

For example you might add something like this:

​	例如，你可以添加如下内容：



```dockerfile
ONBUILD ADD . /app/src
ONBUILD RUN /usr/local/bin/python-build --dir /app/src
```

### ONBUILD 限制 ONBUILD limitations

- Chaining `ONBUILD` instructions using `ONBUILD ONBUILD` isn't allowed. 不允许使用 `ONBUILD ONBUILD` 链式指令。
- The `ONBUILD` instruction may not trigger `FROM` or `MAINTAINER` instructions. `ONBUILD` 指令不能触发 `FROM` 或 `MAINTAINER` 指令。
- `ONBUILD COPY --from` is [not supported](https://github.com/moby/buildkit/issues/816). `ONBUILD COPY --from` [不支持](https://github.com/moby/buildkit/issues/816)。

## STOPSIGNAL



```dockerfile
STOPSIGNAL signal
```

The `STOPSIGNAL` instruction sets the system call signal that will be sent to the container to exit. This signal can be a signal name in the format `SIG<NAME>`, for instance `SIGKILL`, or an unsigned number that matches a position in the kernel's syscall table, for instance `9`. The default is `SIGTERM` if not defined.

​	`STOPSIGNAL` 指令设置发送到容器的系统调用信号以退出。此信号可以是格式为 `SIG<NAME>` 的信号名称，例如 `SIGKILL`，或者是内核系统调用表中匹配位置的无符号数字，例如 `9`。默认值为 `SIGTERM`（如果未定义）。

The image's default stopsignal can be overridden per container, using the `--stop-signal` flag on `docker run` and `docker create`.

​	镜像的默认停止信号可以通过容器的 `--stop-signal` 标志覆盖，例如 `docker run` 和 `docker create`。

## HEALTHCHECK

The `HEALTHCHECK` instruction has two forms:

​	`HEALTHCHECK` 指令有两种形式：

- `HEALTHCHECK [OPTIONS] CMD command` (check container health by running a command inside the container) - `HEALTHCHECK [OPTIONS] CMD command`（通过在容器内运行命令检查容器的健康状态）
- `HEALTHCHECK NONE` (disable any healthcheck inherited from the base image) `HEALTHCHECK NONE`（禁用从基础镜像继承的任何健康检查）

The `HEALTHCHECK` instruction tells Docker how to test a container to check that it's still working. This can detect cases such as a web server stuck in an infinite loop and unable to handle new connections, even though the server process is still running.

​	`HEALTHCHECK` 指令告诉 Docker 如何检查容器以确保其仍在正常运行。这可以检测到例如 Web 服务器卡住在无限循环中无法处理新连接的情况，即使服务器进程仍在运行。

When a container has a healthcheck specified, it has a health status in addition to its normal status. This status is initially `starting`. Whenever a health check passes, it becomes `healthy` (whatever state it was previously in). After a certain number of consecutive failures, it becomes `unhealthy`.

​	当容器指定了健康检查时，除了正常状态外还有一个健康状态。此状态最初为 `starting`。每次健康检查通过后，其状态会变为 `healthy`。经过一定次数的连续失败后，其状态变为 `unhealthy`。

The options that can appear before `CMD` are:

​	可以在 `CMD` 之前设置的选项有：

- `--interval=DURATION` (default: `30s`)
- `--timeout=DURATION` (default: `30s`)
- `--start-period=DURATION` (default: `0s`)
- `--start-interval=DURATION` (default: `5s`)
- `--retries=N` (default: `3`)

The health check will first run **interval** seconds after the container is started, and then again **interval** seconds after each previous check completes.

​	健康检查会在容器启动后 **interval** 秒内首次运行，然后每次检查完成后再次运行。

If a single run of the check takes longer than **timeout** seconds then the check is considered to have failed.

​	如果单次检查运行时间超过 **timeout** 秒，则检查视为失败。

It takes **retries** consecutive failures of the health check for the container to be considered `unhealthy`.

​	需要 **retries** 次连续健康检查失败后，容器状态才会被视为 `unhealthy`。

**start period** provides initialization time for containers that need time to bootstrap. Probe failure during that period will not be counted towards the maximum number of retries. However, if a health check succeeds during the start period, the container is considered started and all consecutive failures will be counted towards the maximum number of retries.

​	**start period** 为需要引导时间的容器提供初始化时间。在此期间探测失败不会计入最大重试次数。然而，如果健康检查在启动期间成功，则容器被视为已启动，所有后续失败将计入最大重试次数。

**start interval** is the time between health checks during the start period. This option requires Docker Engine version 25.0 or later.

​	**start interval** 是启动期间健康检查之间的时间间隔。此选项需要 Docker Engine 版本 25.0 或更高版本。

There can only be one `HEALTHCHECK` instruction in a Dockerfile. If you list more than one then only the last `HEALTHCHECK` will take effect.

​	Dockerfile 中只能有一个 `HEALTHCHECK` 指令。如果列出多个，则只有最后一个生效。

The command after the `CMD` keyword can be either a shell command (e.g. `HEALTHCHECK CMD /bin/check-running`) or an exec array (as with other Dockerfile commands; see e.g. `ENTRYPOINT` for details).

​	在 `CMD` 关键字后面的命令可以是 shell 命令（例如 `HEALTHCHECK CMD /bin/check-running`）或 exec 数组（与其他 Dockerfile 命令相同；参见例如 `ENTRYPOINT` 的详细信息）。

The command's exit status indicates the health status of the container. The possible values are:

​	命令的退出状态表示容器的健康状态。可能的值是：

- 0: success - the container is healthy and ready for use
  - 0：成功 - 容器健康且可用

- 1: unhealthy - the container isn't working correctly
  - 1：不健康 - 容器未正常工作

- 2: reserved - don't use this exit code
  - 2：保留 - 不要使用此退出代码


For example, to check every five minutes or so that a web-server is able to serve the site's main page within three seconds:

​	例如，每五分钟检查一次 Web 服务器是否能在三秒内提供主页面服务：

```dockerfile
HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

To help debug failing probes, any output text (UTF-8 encoded) that the command writes on stdout or stderr will be stored in the health status and can be queried with `docker inspect`. Such output should be kept short (only the first 4096 bytes are stored currently).

​	为帮助调试失败的探测，命令在 stdout 或 stderr 上写入的任何文本（UTF-8 编码）将存储在健康状态中，并可通过 `docker inspect` 查询。建议将输出保持简短（当前仅存储前 4096 字节）。

When the health status of a container changes, a `health_status` event is generated with the new status.

​	当容器的健康状态发生变化时，会生成带有新状态的 `health_status` 事件。



## SHELL



```dockerfile
SHELL ["executable", "parameters"]
```

The `SHELL` instruction allows the default shell used for the shell form of commands to be overridden. The default shell on Linux is `["/bin/sh", "-c"]`, and on Windows is `["cmd", "/S", "/C"]`. The `SHELL` instruction must be written in JSON form in a Dockerfile.

​	`SHELL` 指令允许覆盖命令的默认 shell。Linux 上的默认 shell 是 `["/bin/sh", "-c"]`，Windows 上则是 `["cmd", "/S", "/C"]`。`SHELL` 指令必须以 JSON 形式编写。

The `SHELL` instruction is particularly useful on Windows where there are two commonly used and quite different native shells: `cmd` and `powershell`, as well as alternate shells available including `sh`.

​	`SHELL` 指令在 Windows 上特别有用，因其有两个常用且差别较大的原生 shell：`cmd` 和 `powershell`，以及其他 shell，如 `sh`。

The `SHELL` instruction can appear multiple times. Each `SHELL` instruction overrides all previous `SHELL` instructions, and affects all subsequent instructions. For example:

​	`SHELL` 指令可以出现多次。每个 `SHELL` 指令会覆盖所有之前的，并影响后续所有指令。例如：

```dockerfile
FROM microsoft/windowsservercore

# Executed as cmd /S /C echo default
RUN echo default

# Executed as cmd /S /C powershell -command Write-Host default
RUN powershell -command Write-Host default

# Executed as powershell -command Write-Host hello
SHELL ["powershell", "-command"]
RUN Write-Host hello

# Executed as cmd /S /C echo hello
SHELL ["cmd", "/S", "/C"]
RUN echo hello
```

The following instructions can be affected by the `SHELL` instruction when the shell form of them is used in a Dockerfile: `RUN`, `CMD` and `ENTRYPOINT`.

​	在 Dockerfile 中使用 shell 形式的以下指令会受到 `SHELL` 指令影响：`RUN`、`CMD` 和 `ENTRYPOINT`。

The following example is a common pattern found on Windows which can be streamlined by using the `SHELL` instruction:

​	以下示例展示了在 Windows 上使用 `SHELL` 指令可以简化的常见模式：



```dockerfile
RUN powershell -command Execute-MyCmdlet -param1 "c:\foo.txt"
```

The command invoked by the builder will be:

​	构建器调用的命令为：

```powershell
cmd /S /C powershell -command Execute-MyCmdlet -param1 "c:\foo.txt"
```

This is inefficient for two reasons. First, there is an unnecessary `cmd.exe` command processor (aka shell) being invoked. Second, each `RUN` instruction in the shell form requires an extra `powershell -command` prefixing the command.

​	这在两个方面效率低下。首先，调用了不必要的 `cmd.exe` 命令处理器（即 shell）。其次，每个 shell 形式的 `RUN` 指令都需要额外的 `powershell -command` 前缀。

To make this more efficient, one of two mechanisms can be employed. One is to use the JSON form of the `RUN` command such as:

​	要提高效率，可采用两种方法之一。一个是使用 `RUN` 命令的 JSON 形式，如：

```dockerfile
RUN ["powershell", "-command", "Execute-MyCmdlet", "-param1 \"c:\\foo.txt\""]
```

While the JSON form is unambiguous and does not use the unnecessary `cmd.exe`, it does require more verbosity through double-quoting and escaping. The alternate mechanism is to use the `SHELL` instruction and the shell form, making a more natural syntax for Windows users, especially when combined with the `escape` parser directive:

​	虽然 JSON 形式明确，不使用多余的 `cmd.exe`，但需要通过双引号和转义来提高可读性。另一种方法是使用 `SHELL` 指令结合 `escape` 解析指令，为 Windows 用户提供更自然的语法：

```dockerfile
# escape=`

FROM microsoft/nanoserver
SHELL ["powershell","-command"]
RUN New-Item -ItemType Directory C:\Example
ADD Execute-MyCmdlet.ps1 c:\example\
RUN c:\example\Execute-MyCmdlet -sample 'hello world'
```

Resulting in:

​	这会生成以下输出：

```console
PS E:\myproject> docker build -t shell .

Sending build context to Docker daemon 4.096 kB
Step 1/5 : FROM microsoft/nanoserver
 ---> 22738ff49c6d
Step 2/5 : SHELL powershell -command
 ---> Running in 6fcdb6855ae2
 ---> 6331462d4300
Removing intermediate container 6fcdb6855ae2
Step 3/5 : RUN New-Item -ItemType Directory C:\Example
 ---> Running in d0eef8386e97


    Directory: C:\


Mode         LastWriteTime              Length Name
----         -------------              ------ ----
d-----       10/28/2016  11:26 AM              Example


 ---> 3f2fbf1395d9
Removing intermediate container d0eef8386e97
Step 4/5 : ADD Execute-MyCmdlet.ps1 c:\example\
 ---> a955b2621c31
Removing intermediate container b825593d39fc
Step 5/5 : RUN c:\example\Execute-MyCmdlet 'hello world'
 ---> Running in be6d8e63fe75
hello world
 ---> 8e559e9bf424
Removing intermediate container be6d8e63fe75
Successfully built 8e559e9bf424
PS E:\myproject>
```

The `SHELL` instruction could also be used to modify the way in which a shell operates. For example, using `SHELL cmd /S /C /V:ON|OFF` on Windows, delayed environment variable expansion semantics could be modified.

​	`SHELL` 指令还可用于修改 shell 操作方式。例如，在 Windows 上使用 `SHELL cmd /S /C /V:ON|OFF`，可更改延迟环境变量扩展语义。

The `SHELL` instruction can also be used on Linux should an alternate shell be required such as `zsh`, `csh`, `tcsh` and others.

​	在 Linux 上也可以使用 `SHELL` 指令，如需要 `zsh`、`csh`、`tcsh` 等 shell。

## Here-Documents

Here-documents allow redirection of subsequent Dockerfile lines to the input of `RUN` or `COPY` commands. If such command contains a [here-document](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_07_04) the Dockerfile considers the next lines until the line only containing a here-doc delimiter as part of the same command.

​	Here-documents 允许将接下来的 Dockerfile 行重定向到 `RUN` 或 `COPY` 命令的输入中。如果命令包含 [here-document](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_07_04)，Dockerfile 会将直到只包含 here-doc 分隔符的行之前的所有行视为同一命令的一部分。

### 示例：运行多行脚本 Example: Running a multi-line script



```dockerfile
# syntax=docker/dockerfile:1
FROM debian
RUN <<EOT bash
  set -ex
  apt-get update
  apt-get install -y vim
EOT
```

If the command only contains a here-document, its contents is evaluated with the default shell.

​	如果命令只包含 here-document，其内容会使用默认 shell 进行求值。

```dockerfile
# syntax=docker/dockerfile:1
FROM debian
RUN <<EOT
  mkdir -p foo/bar
EOT
```

Alternatively, shebang header can be used to define an interpreter.

​	另外，可以使用 shebang 头来定义解释器。

```dockerfile
# syntax=docker/dockerfile:1
FROM python:3.6
RUN <<EOT
#!/usr/bin/env python
print("hello world")
EOT
```

More complex examples may use multiple here-documents.

​	更复杂的示例可以使用多个 here-document。

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN <<FILE1 cat > file1 && <<FILE2 cat > file2
I am
first
FILE1
I am
second
FILE2
```

### 示例：创建内联文件 Example: Creating inline files

With `COPY` instructions, you can replace the source parameter with a here-doc indicator to write the contents of the here-document directly to a file. The following example creates a `greeting.txt` file containing `hello world` using a `COPY` instruction.

​	使用 `COPY` 指令，可以用 here-doc 指示符替换源参数，将 here-document 的内容直接写入文件。以下示例使用 `COPY` 指令创建了一个包含“hello world”的 `greeting.txt` 文件。

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
COPY <<EOF greeting.txt
hello world
EOF
```

Regular here-doc [variable expansion and tab stripping rules](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_07_04) apply. The following example shows a small Dockerfile that creates a `hello.sh` script file using a `COPY` instruction with a here-document.

​	正常的 here-doc [变量扩展和制表符去除规则](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_07_04)适用。以下示例展示了一个小的 Dockerfile，它使用带有 here-document 的 `COPY` 指令创建了一个 `hello.sh` 脚本文件。

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
ARG FOO=bar
COPY <<-EOT /script.sh
  echo "hello ${FOO}"
EOT
ENTRYPOINT ash /script.sh
```

In this case, file script prints "hello bar", because the variable is expanded when the `COPY` instruction gets executed.

​	在此情况下，脚本文件输出“hello bar”，因为在执行 `COPY` 指令时变量已被扩展。

```console
$ docker build -t heredoc .
$ docker run heredoc
hello bar
```

If instead you were to quote any part of the here-document word `EOT`, the variable would not be expanded at build-time.

​	如果你给 here-document 的任何部分加引号，那么变量将在构建时不会被扩展。

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
ARG FOO=bar
COPY <<-"EOT" /script.sh
  echo "hello ${FOO}"
EOT
ENTRYPOINT ash /script.sh
```

Note that `ARG FOO=bar` is excessive here, and can be removed. The variable gets interpreted at runtime, when the script is invoked:

​	注意这里的 `ARG FOO=bar` 是多余的，可以省略。变量会在脚本运行时解析：

```console
$ docker build -t heredoc .
$ docker run -e FOO=world heredoc
hello world
```

## Dockerfile 示例 Dockerfile examples

For examples of Dockerfiles, refer to:

​	有关 Dockerfile 的示例，请参阅：

- The ["build images" section]({{< ref "/manuals/DockerBuild/Building/Bestpractices">}})
- The ["get started" tutorial]({{< ref "/get-started">}})
- The [language-specific getting started guides](https://docs.docker.com/language/)
- The [build guide](https://docs.docker.com/build/guide/)

------

1. Value required 需要值
2. For Docker-integrated [BuildKit](https://docs.docker.com/build/buildkit/#getting-started) and `docker buildx build` - 对于 Docker 集成的 [BuildKit](https://docs.docker.com/build/buildkit/#getting-started) 和 `docker buildx build`
