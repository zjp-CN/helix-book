# 主题

要使用主题，请在配置文件 [`config.toml`](./configuration.md) 最顶端，在第一个部分之前添加 `theme = "<name>"`，或者在运行中使用 `:theme <name>` 选择它。

## 创建主题

创建一个以你的主题名称为文件名的文件，即 `mytheme.toml`，并将其放置在你的 `themes` 目录下（即 `~/.config/helix/hemes`）。该目录可能必须事先创建。

`default` 和 `base16_default` 是为内置主题保留的，不能被用户定义的主题覆盖。

默认的 `theme.toml` 可以在 [这里][theme-toml] 找到，用户提交的主题在 [这里][user-theme]。

[theme-toml]: https://github.com/helix-editor/helix/blob/master/theme.toml
[user-theme]: https://github.com/helix-editor/helix/blob/master/runtime/themes

主题文件中的每一行如下：

```toml
key = { fg = "#ffffff", bg = "#000000", modifiers = ["bold", "italic"] }
```

其中 `key` 表示你要设置的样式，`fg` 指定前景色，`bg` 表示背景色，`modifiers` 是样式修饰符列表。可以省略 `bg` 和 `modifier` 来使用默认设置。

仅指定前景色，请执行以下操作：

```toml
key = "#ffffff"
```

如果键包含点 `'.'`，则必须用引号将其引起来，以防止它被解析为 [点键](https://toml.io/en/v1.0.0#keys)。

```toml
"key.key" = "#ffffff"
```

### palette

建议定义命名颜色的调色板 (palette)，并从主题中的配置值时指向它们。在你的主题文件中添加一个名为 `palette` 的表：

```toml
"ui.background" = "white"
"ui.text" = "black"

[palette]
white = "#ffffff"
black = "#000000"
```

请记住，`[palette]` 表在其表头之后包含所有键，因此你应该在普通主题选项之后定义调色板。

默认调色板使用终端的默认 16 种颜色，颜色名称如下所示。配置文件中的 `[palette]` 部分优先于它，并合并到默认的调色板中。

| Color           | 中文     |
|-----------------|----------|
| `black`         | 黑色     |
| `red`           | 红色     |
| `green`         | 绿色     |
| `yellow`        | 黄色     |
| `blue`          | 蓝调     |
| `magenta`       | 洋红色   |
| `cyan`          | 青色     |
| `gray`          | 灰色     |
| `light-red`     | 浅红色   |
| `light-green`   | 淡绿色   |
| `light-yellow`  | 淡黄色   |
| `light-blue`    | 淡蓝色   |
| `light-magenta` | 浅品红色 |
| `light-cyan`    | 淡蓝色   |
| `light-gray`    | 浅灰色   |
| `white`         | 白色     |

### Modifiers

下列值可以用作修饰符 (modifiers)。你的终端仿真器可能不支持不太常见的修饰符。

| Modifier      | 中文     |
|---------------|----------|
| `bold`        | 粗体     |
| `dim`         | 暗淡     |
| `italic`      | 斜体     |
| `underlined`  | 带下划线 |
| `slow_blink`  | 慢闪动   |
| `rapid_blink` | 快速闪动 |
| `reversed`    | 反色     |
| `hidden`      | 隐藏     |
| `crossed_out` | 划线     |

### 继承现有主题

通过将 `inherits` 属性设置为现有主题来扩展其他主题。

```toml
inherits = "boo_berry"

# Override the theming for "keyword"s:
"keyword" = { fg = "gold" }

# Override colors in the palette:
[palette]
berry = "#2A2A4D"
```

### 作用域

以下是可用于设置样式的作用域列表。

#### 语法高亮

[tree-sitter scopes]: https://tree-sitter.github.io/tree-sitter/syntax-highlighting#theme
[SublimeText]: https://www.sublimetext.com/docs/scope_naming.html
[TextMate]: https://macromates.com/manual/en/language_grammars

这些键与 [tree-sitter 作用域][tree-sitter scopes]相匹配。

对于所产生的给定高亮，样式将基于最长匹配主题键来确定。例如，高亮显示的 `function.builtin.static` 将匹配关键字 `function.builtin`，而不是 `function`。

我们使用一组类似 [SublimeText] 的作用域。另见 [TextMate] 作用域。

* `attribute` 类属性、html 标记属性
* `type` 类型
  * `builtin` 语言提供的原始类型，如 `int`、`usize`
* `constructor`
- `constant` (TODO: constant.other.placeholder for %v)
  - `builtin` 语言提供的特殊常量，如 `true`、`false`、`nil`
    - `boolean`
  - `character`
    - `escape`
  - `numeric`
    - `integer`
    - `float`

- `string` (TODO: string.quoted.{single, double}, string.raw/.unquoted)?
  - `regexp` 正则表达式
  - `special`
    - `path`
    - `url`
    - `symbol` Erlang/Elixir atoms, Ruby symbols, Clojure keywords

- `comment` 注释
  - `line` 单行注释，如 `//`
  - `block` 块注释，如 `/* */`
    - `documentation` 文档注释，如 Rust 中的 `///`

- `variable` 变量
  - `builtin` 语言保留的变量，如 `self`、`this`、`super`
  - `parameter` 函数参数
  - `other`
    - `member` 复核数据结构的字段，如结构体、unions

- `label`

- `punctuation`
  - `delimiter` 逗号、冒号
  - `bracket` 圆括号、尖括号等
  - `special` 字符串插值的大括号

- `keyword`
  - `control`
    - `conditional` - `if`, `else`
    - `repeat` - `for`, `while`, `loop`
    - `import` - `import`, `export`
    - `return`
    - `exception`
  - `operator` - `or`, `in`
  - `directive` 前端处理程序指令，如 C 中的 `#if`
  - `function` - `fn`, `func`
  - `storage` 描述存储数据的关键字
    - `type` 类型之类的 `class`、`function`、`var`、`let`
    - `modifier` 存储修饰符，如 `static`、`mut`、`const`、`ref`

- `operator` - `||`, `+=`, `>`

- `function`
  - `builtin`
  - `method`
  - `macro`
  - `special` C 中的 preprocessor

- `tag` 标签，如 HTML 的 `<body>`

- `namespace`

- `markup`
  - `heading`
    - `marker`
    - `1`, `2`, `3`, `4`, `5`, `6` h1-h6 的标题文本
  - `list`
    - `unnumbered`
    - `numbered`
  - `bold`
  - `italic`
  - `link`
    - `url` 指向链接的 urls
    - `label` 非 url 链接引用
    - `text` url 和链接描述的图片
  - `quote`
  - `raw`
    - `inline`
    - `block`

- `diff` 版本控制更改
  - `plus` 增
  - `minus` 删
  - `delta` 改
    - `moved` 重命名或移动后的文件或修改

#### 界面

这些作用域用于编辑器界面的主题化。

* `markup`
  * `normal`
    * `completion` 补全弹出的文档窗口 ui
    * `hover` 悬浮弹出窗口 ui
  * `heading`
    * `completion` 补全弹出的文档窗口 ui
    * `hover` 悬浮弹出窗口 ui
  * `raw`
    * `inline`
      * `completion` 补全弹出的文档窗口 ui
      * `hover` 悬浮弹出窗口 ui

| 键                          | 含义                                                                      |
|-----------------------------|---------------------------------------------------------------------------|
| `ui.background`             |                                                                           |
| `ui.background.separator`   | 输入行下方的选取器分隔符                                                  |
| `ui.cursor`                 |                                                                           |
| `ui.cursor.insert`          |                                                                           |
| `ui.cursor.select`          |                                                                           |
| `ui.cursor.match`           | 匹配的括号等                                                              |
| `ui.cursor.primary`         | 主选区的光标                                                              |
| `ui.gutter`                 | 侧边栏                                                                    |
| `ui.gutter.selected`        | 光标所在行的侧边栏                                                        |
| `ui.linenr`                 | 行号                                                                      |
| `ui.linenr.selected`        | 光标所在行的行号                                                          |
| `ui.statusline`             | Statusline                                                                |
| `ui.statusline.inactive`    | 当前光标不在文档的 (unfocused) statusline                                 |
| `ui.statusline.normal`      | normal 模式下的 statusline，需开启 [`editor.color-modes`][editor-section] |
| `ui.statusline.insert`      | insert 模式下的 statusline，需开启 [`editor.color-modes`][editor-section] |
| `ui.statusline.select`      | select 模式下的 statusline，需开启 [`editor.color-modes`][editor-section] |
| `ui.statusline.separator`   | statusline 中的分隔符                                                     |
| `ui.popup`                  | 文档弹出窗口（按 `<space>k`）                                             |
| `ui.popup.info`             | 多按键选项的提示框                                                        |
| `ui.window`                 | 单独分隔的边界线                                                          |
| `ui.help`                   | 命令的描述框                                                              |:
| `ui.text`                   | 命令提示框、弹出文本等                                                    |
| `ui.text.focus`             |                                                                           |
| `ui.text.info`              | 按键：`ui.popup.info` 框内的命令文本                                      |
| `ui.virtual.ruler`          | 标尺列，见 [editor-section]                                               |
| `ui.virtual.whitespace`     | 可见的空白字符                                                            |
| `ui.virtual.indent-guide`   | 垂直缩进宽度参考线                                                        |
| `ui.menu`                   | 代码和命令补全菜单                                                        |
| `ui.menu.selected`          | 自动补全中的所选项                                                        |
| `ui.menu.scroll`            | `fg` 设置 thumb color；`bg` 设置滚动条的 track color                      |
| `ui.selection`              | 编辑区域的选区                                                            |
| `ui.selection.primary`      |                                                                           |
| `ui.cursorline.primary`     | 主光标所在行，需开启 [cursorline][editor-section]                         |
| `ui.cursorline.secondary`   | 任何其他光标所在行，需开启 [cursorline][editor-section]                   |
| `ui.cursorcolumn.primary`   | 主光标所在列，需开启 [cursorcolumn][editor-section]                       |
| `ui.cursorcolumn.secondary` | 任何其他光标所在列，需开启 [cursorcolumn][editor-section]                 |
| `warning`                   | warning 诊断（侧边栏）                                            |
| `error`                     | error 诊断（侧边栏）                                              |
| `info`                      | info 诊断（侧边栏）                                               |
| `hint`                      | hint 诊断（侧边栏）                                               |
| `diagnostic`                | fallback 诊断style （编辑区域）                                   |
| `diagnostic.hint`           | hint 诊断（编辑区域）                                             |
| `diagnostic.info`           | info 诊断（编辑区域）                                             |
| `diagnostic.warning`        | warning 诊断（编辑区域）                                          |
| `diagnostic.error`          | error 诊断（编辑区域）                                            |

[editor-section]: ./configuration.md#editor

你可以检查你的主题是否与规范相符合

```shell
cargo xtask themelint onedark  # replace onedark with <name>
```
