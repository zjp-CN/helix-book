# 语言 / LSP 配置

特定的语言设置和语言服务器的设置在 `languages.toml` 文件中进行配置。

## `languages.toml` 文件

有三个可能的 `languages.toml` 文件。第一个被编译进 Helix，在 Helix 仓库[可看到][master/languages.toml]。这提供了语言和语言服务器的默认配置。

[master/languages.toml]: https://github.com/helix-editor/helix/blob/master/languages.toml

你可以在 [配置目录](./configuration.md) 中定义自己的 `languages.toml`，它会覆盖内置语言配置中的值。例如，禁用 Rust 的自动 LSP 代码格式化：

```toml
# in <config_dir>/helix/languages.toml

[[language]]
name = "rust"
auto-format = false
```

也可以在项目目录的 `.helix` 文件夹下创建 `languages.toml` 文件来覆盖语言配置。其设置将与配置目录中的语言配置和内置配置合并。

## 语言配置

每种语言都是通过在 `languages.toml` 文件中添加 `[[Language]]` 来配置的。例如：

```toml
[[language]]
name = "mylang"
scope = "source.mylang"
injection-regex = "^mylang$"
file-types = ["mylang", "myl"]
comment-token = "#"
indent = { tab-width = 2, unit = "  " }
language-server = { command = "mylang-lsp", args = ["--stdio"] }
formatter = { command = "mylang-formatter" , args = ["--stdin"] }
```

支持以下配置键：

| 键                    | 描述                                                                                                                                                                  |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                | 语言的名称                                                                                                                                                            |
| `scope`               | 像 `source.js` 的字符串，它标识了语言。目前我们尽量把 scope name 匹配常用的 TextMate 语法和 Linguist 库；通常是 `source.<name>` 或 `text.<name>` 形式（以防标记语言） |
| `injection-regex`     | 针对语言名称进行测试的正则模式，用来确定这个语言是否应该使用 [language injection][treesitter-language-injection]                                                      |
| `file-types`          | 语言的文件类型，比如 `["yml", "yaml"]`；支持拓展名和完整的文件名                                                                                                      |
| `shebangs`            | shebang 行的解释器，比如 `["sh", "bash"]`                                                                                                                             |
| `roots`               | 一组用于搜索标记文件，以找到工作空间的根目录比如 `Cargo.lock`、 `yarn.lock`                                                                                           |
| `auto-format`         | 是否保存文件是自动格式化                                                                                                                                              |
| `diagnostic-severity` | 用于显示的最低程度的诊断；允许的值有 `Error`, `Warning`, `Info`, `Hint`                                                                                               |
| `comment-token`       | 用于注释的标记                                                                                                                                                        |
| `indent`              | 使用什么样的缩进；子键有 `tab-width` 和 `unit`                                                                                                                        |
| `language-server`     | 所运行的语言服务器，见下文                                                                                                                                            |
| `config`              | 语言服务器配置                                                                                                                                                        |
| `grammar`             | 使用的 tree-sitter 语法；默认为 `name` 的值                                                                                                                           |
| `formatter`           | 格式化程序，定义之后优先于 lsp 使用；它必须从 stdin 输入源文件，从 stdout 写出成格式化之后的文件                                                                    |
| `max-line-length`     | 一行的最大长度，用于 `:reflow` 命令                                                                                                                                   |

[treesitter-language-injection]: https://tree-sitter.github.io/tree-sitter/syntax-highlighting#language-injection

### 语言服务器配置

`language-server` 键采用以下键：

| 按键          | 描述                                                                                               |
|---------------|----------------------------------------------------------------------------------------------------|
| `command`     | 要执行的语言服务器二进制文件的名称，二进制文件必须位于 `$PATH`                                     |
| `args`        | 传递给语言服务器二进制文件的参数列表                                                               |
| `timeout`     | 向语言服务器发出请求可能需要的最长时间，以秒为单位，默认为 20                                      |
| `language-id` | 传递给语言服务器的语言名称：某些语言服务器支持多种语言，并使用此字段来确定缓冲区中提供的是哪种语言 |

顶级 `config` 键用于配置 LSP 初始化选项。 `config` 中的 `format` 子表用于将额外的格式化选项传递给
[文档格式化请求][Document Formatting Requests]。例如，对于 typescript：

[Document Formatting Requests]: https://github.com/microsoft/language-server-protocol/blob/gh-pages/_specifications/specification-3-16.md#document-formatting-request--leftwards_arrow_with_hook

```toml
[[language]]
name = "typescript"
auto-format = true
# pass format options according to https://github.com/typescript-language-server/typescript-language-server#workspacedidchangeconfiguration omitting the "[language].format." prefix.
config = { format = { "semicolons" = "insert", "insertSpaceBeforeFunctionParenthesis" = true } }
```

## Tree-sitter 语法配置

语言的 tree-sitter 语法来源在 `languages.toml` 中的 `[[grammar]]` 中指定。例如：

```toml
[[grammar]]
name = "mylang"
source = { git = "https://github.com/example/mylang", rev = "a250c4582510ff34767ec3b7dcdd3c24e8c8aa68" }
```

语法配置采用以下键：

| 键       | 描述                                     |
|----------|------------------------------------------|
| `name`   | tree-sitter 语法的名字                   |
| `source` | 获取语法的方法，一个具有下面定义模式的表 |

其中，`source` 是来自 git 语法仓库的表：

| 键        | Description	描述                                                                                                                                                                    |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `git`     | 克隆语法的远程 git URL                                                                                                                                                              |
| `rev`     | 获取的修订（commit hash 或 tag）                                                                                                                                                    |
| `subpath` | 语法目录中应构建的路径。一些语法库托管多个语法，例如 `tree-sitter-typescript` 和 `tree-sitter-ocaml` 子目录。此键用于指向 `hx --grammar build` 编译到正确的路径；省略时，使用根目录 |

### 选择语法

在使用 `hx --grammar fetch` 和` hx --grammar build` 时，你可以使用顶级的 `use-grammars` 键来控制获取和构建哪些语法。

```toml
# Note: this key must come **before** the [[language]] and [[grammar]] sections
use-grammars = { only = [ "rust", "c", "cpp" ] }
# or
use-grammars = { except = [ "yaml", "json" ] }
```

如果省略，则获取并构建所有语法。
