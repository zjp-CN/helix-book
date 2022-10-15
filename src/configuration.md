# 配置

要覆盖全局配置参数，请在你的配置目录中创建一个 `config.toml` 文件：

* Linux 和 Mac: `~/.config/helix/config.toml`
* Windows: `%AppData%\helix\config.toml`

> 提示：在 Helix 正常模式下输入 `:config-open` 即可轻松打开配置文件。

示例配置：

```toml
theme = "onedark"

[editor]
line-number = "relative"
mouse = false

[editor.cursor-shape]
insert = "bar"
normal = "block"
select = "underline"

[editor.file-picker]
hidden = false
```

你也可以使用 `-c` 或 `--config` 命令行参数指定配置文件的路径：`hx -c path/to/custom-config.toml`。

还可以通过向 Helix 进程发送 `USR1` 信号来触发配置文件重新加载，例如 `pkill -USR1 hx`。这仅在 Unix 操作系统上支持。

> 译者注：在已打开的 Helix 中可以使用 `:set`（或 `:set-option`）设置本页面罗列的键的值，如
> `:set line-number relative`、`:set lsp.display-messages true`。当你按 `:set <Tab>` 和输入命令的过程中，会弹出可配置的键。

## editor 及其子表

### `[editor]`

| 键                       | 描述                                                                                                                                    | 默认值                                            |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| `scrolloff`              | 滚动时屏幕边缘周围的填充行数                                                                                                            | `5`                                               |
| `mouse`                  | 启用鼠标模式                                                                                                                            | `true`                                            |
| `middle-click-paste`     | 启用中键单击粘贴                                                                                                                        | `true`                                            |
| `scroll-lines`           | 每次滚轮所滚动的行数                                                                                                                    | `3`                                               |
| `shell`                  | 运行外部命令时使用的 shell 程序                                                                                                         | Unix: `["sh", "-c"]`<br/>Windows: `["cmd", "/C"]` |
| `line-number`            | 显示行号：`absolute` 每行的编号，`relative` 离当前行的距离。未聚焦或处于 insert mode 时，`relative` 仍显示绝对行号                      | `absolute`                                        |
| `cursorline`             | 高亮光标所在行                                                                                                                          | `false`                                           |
| `cursorcolumn`           | 高亮光标所在列                                                                                                                          | `false`                                           |
| `gutters`                | 侧条显示的内容：可选值有 `diagnostics`、`line-numbers`、`spacer`。注意 `diagnostics` 包含其他功能，比如断点。如果为空，则填充一个宽度。 | `["diagnostics", "line-numbers"]`                 |
| `auto-completion`        | 自动补全时自动 pop up                                                                                                                   | `true`                                            |
| `auto-format`            | 保存时自动格式化                                                                                                                        | `true`                                            |
| `idle-timeout`           | 自上次按键后空闲计时器触发前的时间，以毫秒为单位。用于自动补全，设置为 0 表示即时补全。                                                 | `400`                                             |
| `completion-trigger-len` | 光标下触发自动补全的最小单词长度                                                                                                        | `2`                                               |
| `auto-info`              | 是否显示信息框                                                                                                                          | `true`                                            |
| `true-color`             | 在未能检测到终端真彩色时，设置此项为 `true` 来表明使用真彩色                                                                            | `false`                                           |
| `rulers`                 | 显示标尺的列位置列表。可以在 `languages.toml` 中使用特定语言的 `rulers` 键覆盖这里的设置。                                              | `[]`                                              |
| `bufferline`             | 在编辑器顶部显示一行显示已打开的缓冲区，可以是 `always`、`never` 或 `multiple`（只在超过一个缓冲区存在时显式）                          | `never`                                           |
| `color-modes`            | 根据模式本身用不同的颜色给模式指示器上色                                                                                                | `false`                                           |

### `[editor.statusline]`

配置在编辑器底部的状态栏。状态栏分成三个区域：

`[ ... ... LEFT ... ... | ... ... ... ... CENTER ... ... ... ... | ... ... RIGHT ... ... ]`

状态栏元素可以按如下方式定义：

```toml
[editor.statusline]
left = ["mode", "spinner"]
center = ["file-name"]
right = ["diagnostics", "selections", "position", "file-encoding", "file-line-ending", "file-type"]
separator = "│"
mode.normal = "NORMAL"
mode.insert = "INSERT"
mode.select = "SELECT"
```

`[editor.statusline]` 的子键如下：

| 键            | 描述                     | 默认值                                                       |
|---------------|--------------------------|--------------------------------------------------------------|
| `left`        | 左端对齐的列表           | `["mode", "spinner", "file-name"]`                           |
| `center`      | 中间对齐的列表           | `[]`                                                         |
| `right`       | 右端对齐的列表           | `["diagnostics", "selections", "position", "file-encoding"]` |
| `separator`   | 分隔元素的字符           | `"│"`                                                        |
| `mode.normal` | normal mode 下显示的文字 | `"NOR"`                                                      |
| `mode.insert` | insert mode 下显示的文字 | `"INS"`                                                      |
| `mode.select` | select mode 下显示的文字 | `"SEL"`                                                      |

可配置以下元素：

| 元素                  | 描述                                                        |
|-----------------------|-------------------------------------------------------------|
| `mode`                | 当前模式 `mode.normal`、`mode.insert`、`mode.select`        |
| `spinner`             | LSP 活动进度指示                                            |
| `file-name`           | 文件路径或名称                                              |
| `file-encoding`       | 如果不是 UTF-8 编码，则显示文件编码                         |
| `file-line-ending`    | 换行符 CRLF 或 LF                                           |
| `total-line-numbers`  | 总行数                                                      |
| `file-type`           | 文件类型                                                    |
| `diagnostics`         | warnings 和/或 errors 的数量                                |
| `selections`          | 活动选区的数量                                              |
| `position`            | 光标位置                                                    |
| `position-percentage` | 光标位置占总行数的百分比                                    |
| `separator`           | 定义在 `editor.statusline.separator` 的字符串，默认为 `│`   |
| `spacer`              | 在元素之间插入空格，可以指定多个/连续的空格                 |

### `[editor.lsp]`

| 键                            | 描述                                                   | 默认值  |
|-------------------------------|--------------------------------------------------------|---------|
| `display-messages`            | 在 statusline 下方显示 LSP 进度消息[^display-messages] | `false` |
| `auto-signature-help`         | 自动弹出签名帮助（和参数提示）                         | `true`  |
| `display-signature-help-docs` | 在签名帮助弹出菜单中显示文档                           | `true`  |

[^display-messages]: 默认情况下，进度旋钮显示在文件路径旁边的 statusline 中。

### `[editor.cursor-shape]`

定义每种模式下光标的形状。注意，由于终端环境的限制，只有主光标可以更改形状。这些选项的有效值为
`block`、`bar`、`underline` 或 `hidden`。

| 键       | 描述                       | 默认值  |
|----------|----------------------------|---------|
| `normal` | [normal mode] 下的光标形状 | `block` |
| `insert` | [insert mode] 下的光标形状 | `block` |
| `select` | [select mode] 下的光标形状 | `block` |

[normal mode]: ./keymap.md#normal-mode
[insert mode]: ./keymap.md#insert-mode
[select mode]: ./keymap.md#select--extend-mode

### `[editor.file-picker]`

设置文件选取器和全局搜索的选项。下面列出的默认文件选取器配置中，除了最后一个键之外，所有键都具有 IgnoreOptions：
Helix 文件选取器和全局搜索是否无视隐藏文件和 ignore 文件中列出的文件。还有一个尚未定义的键 `max-depth`。

所有与 git 相关的选项仅在 git 存储库中启用。

| 键            | 描述                                            | 默认值 |
|---------------|-------------------------------------------------|--------|
| `hidden`      | 忽略隐藏文件                                    | `true` |
| `parents`     | 父目录读取 ignored 文件                         | `true` |
| `ignore`      | 读取 `.ignore`                                  | `true` |
| `git-ignore`  | 读取 `.gitignore`                               | `true` |
| `git-global`  | 读取全局 `.gitignore`，其路径由 git config 指定 | `true` |
| `git-exclude` | 读取 `.git/info/exclude` 文件                   | `true` |
| `max-depth`   | 为递归的最大深度设置一个整数值                  | `None` |

### `[editor.auto-pairs]`

启用自动插入配对符号，如圆括号、方括号等。可以是简单的布尔值，也可以是单字符配对映射。

要完全关闭自动配对，将 `auto-pairs` 设置为 `false`：

```toml
[editor]
auto-pairs = false # defaults to `true`
```

默认配对为 <code>(){}\[\]''""``</code>，但可以通过将 `auto-pairs` 设置为 TOML 表进行自定义：

```toml
[editor.auto-pairs]
'(' = ')'
'{' = '}'
'[' = ']'
'"' = '"'
'`' = '`'
'<' = '>'
```

此外，此设置可在语言配置中使用。除非编辑器设置为 `false`，否则语言配置将覆盖 editor 配置。

添加 `<>` 和删除 `''` 的示例，在 `languages.toml` 中：

```toml
[[language]]
name = "rust"

[language.auto-pairs]
'(' = ')'
'{' = '}'
'[' = ']'
'"' = '"'
'`' = '`'
'<' = '>'
```

### `[editor.search]`

搜索选项。


| 键            | 描述                                                           | 默认值 |
|---------------|----------------------------------------------------------------|--------|
| `smart-case`  | 正则表达式启用智能大小写：除非包含大写字符，否则对大小写不敏感 | `true` |
| `wrap-around` | 匹配达到最后一个之后是否回到第一个                             | `true` |

### `[editor.whitespace]`

通过可见字符渲染空格。使用 `:set whitespace.render all` 可临时开启可见空格。

| 键           | 描述                                                                                      | 默认值       |
|--------------|-------------------------------------------------------------------------------------------|--------------|
| `render`     | 是否渲染空格，要么是 `all` 或 `none`，要么是带子键的表，子键有 `space`、`tab`、`newline`. | `none`       |
| `characters` | 渲染空格的字符；子键为 `tab`、`space`、`nbsp`、`newline`、`tabpad` 之一                   | 见下面的例子 |

示例：

```toml
[editor.whitespace]
render = "all"
# or control each character
[editor.whitespace.render]
space = "all"
tab = "all"
newline = "none"

[editor.whitespace.characters]
space = "·"
nbsp = "⍽"
tab = "→"
newline = "⏎"
tabpad = "·" # Tabs will look like "→···" (depending on tab width)
```

### `[editor.indent-guides]`

渲染垂直缩进辅助线。

| 键            | 描述               | 默认值  |
|---------------|--------------------|---------|
| `render`      | 是否渲染缩进辅助线 | `false` |
| `character`   | 渲染辅助线的字符   | `│`     |
| `skip-levels` | 跳过的缩进级别数   | `0`     |

示例：

```toml
[editor.indent-guides]
render = true
character = "╎" # Some characters that work well: "▏", "┆", "┊", "⸽"
skip-levels = 1
```
