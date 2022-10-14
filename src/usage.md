# 使用

（目前尚未完整记录，更多信息见 [按键映射](./keymap.md)。）

类似于 vimtutor 的介绍见 [tutor] （可通过 `hx --tutor` 或 `:tutor` 访问）。

[tutor]: https://github.com/helix-editor/helix/blob/master/runtime/tutor

## 寄存器

使用类似 Vim 的寄存器 (registers) 来拖拽 (yank) 和存储文本，以便稍后粘贴。使用方法也与 Vim 类似，按 `"` 来选择一个寄存器：

* `"ay`：将当前选择拖到寄存器 `a`
* `"op`：在选择后粘贴寄存器 `o` 中的文本

如果在调用修改或删除文本命令之前选择了寄存器，则将选择的内容存储在寄存器中，并执行操作：

* `"hc`：将所选的文本存储在寄存器 `h` 中，然后修改所选的文本（即删除文本并进入插入模式）
* `"md`：将所选文本存储在寄存器 `m` 中，并删除所选的文本

### 特殊寄存器

| 寄存器字符 | 内容           |
|------------|----------------|
| `/`        | 上次搜索的文本 |
| `:`        | 上次执行的命令 |
| `"`        | 上次拖拽的文本 |
| `_`        | 黑洞           |

> Helix 没有用于复制到系统剪贴板的特殊寄存器，而是提供了特殊的命令和按键绑定。详细信息见 [space mode](keymap.md#space-mode)。
>
> 黑洞寄存器用作无操作 (no-op) 寄存器，这意味着不会向其写入数据或从中读取数据。

## 环绕

[vim-surround]: https://github.com/tpope/vim-surround 
[vim-sandwich]: https://github.com/machakann/vim-sandwich

类似于 [vim-surround] 的功能已被内置到 Helix 中。按键映射的灵感来自 [vim-sandwich]：

![surround demo](https://user-images.githubusercontent.com/23398472/122865801-97073180-d344-11eb-8142-8f43809982c6.gif)

* `ms`：添加环绕字符，作用于选定内容，因此先选择文本，然后按 `ms<char>`
* `mr`：替换环绕字符，用于找到的最接近的配对，所以并不需要选择文本；使用计数[^counts]作用于外层配对
* `md`：删除环绕字符，使用与 `mr` 类似

[^counts]: 译者注：比如 `[[{aaa}]]` 光标在 a 上，按 `2mr])` 把第 2 层 `]` 替换成 `)`，得到 `([{aaa}])`。

它还可以作用于多光标选择（耶！）。例如，将每次出现的 `(use)` 改为 `[use]`：

* 按 `%` 选择整个文件
* 再按 `s` 根据搜索词拆分并选中文本
* 输入 `use` 并按 `Enter`
* 按 `mr([` 用方括号替换圆括号

当前不支持多个字符，但计划支持。

## 基于语法树移动选区

按 `Alt-p`、`Alt-o`、`Alt-i` 和 `Alt-n`（或 `Alt` 和四个方向键之一）可根据语法树的位置来移动选择区域。

让我们通过一个示例来熟悉它们。许多语言对于函数调用都有类似这样的语法：

```
func(arg1, arg2, arg3)
```

函数调用可能会被 tree-sitter 解析为如下语法树：

```tsq
(call
  function: (identifier) ; func
  arguments:
    (arguments           ; (arg1, arg2, arg3)
      (identifier)       ; arg1
      (identifier)       ; arg2
      (identifier)))     ; arg3
```

使用 `:tree-sitter-subtree` 查看所选的语法树。用更直观的树形结构表示成：

```
            ┌────┐
            │call│
      ┌─────┴────┴─────┐
      │                │
┌─────▼────┐      ┌────▼────┐
│identifier│      │arguments│
│  "func"  │ ┌────┴───┬─────┴───┐
└──────────┘ │        │         │
             │        │         │
   ┌─────────▼┐  ┌────▼─────┐  ┌▼─────────┐
   │identifier│  │identifier│  │identifier│
   │  "arg1"  │  │  "arg2"  │  │  "arg3"  │
   └──────────┘  └──────────┘  └──────────┘
```

假设我们光标放在 `arg1` 上，那么就选择了上面树中的 `arg1` 叶节点上（方括号表示选区）：

```
func([arg1], arg2, arg3)
```

按 `Alt-n` （或者 `Alt-→`） 将选择语法树中的下一个同级 (sibling) `arg2`：

```
func(arg1, [arg2], arg3)
```

而按 `Alt-o` （或 `Alt-↑`） 会将选择扩展到父节点。在上面的树中，我们将选择 `arguments` 节点。

```
func[(arg1, arg2, arg3)]
```

还有一些细微差别的行为可以防止你停留在没有同级节点的节点上。如果选择 `arg1`，那么按 `Alt-p` （或 `Alt-←`）
将移动到上一个子节点。但是，由于 `arg1` 在其左侧没有同级项，所以往语法树父级走，然后选择前一个的节点。因此，`Alt-p`
最终把选区移到 `func` 这个标识符上。

```
[func](arg1, arg2, arg3)
```

## 文本对象

![textobject-demo](https://user-images.githubusercontent.com/23398472/124231131-81a4bb00-db2d-11eb-9d10-8e577ca7b177.gif)
![textobject-treesitter-demo](https://user-images.githubusercontent.com/23398472/132537398-2a2e0a54-582b-44ab-a77f-eb818942203d.gif)

上面的演示分别使用以下按键来选择文本对象 (textobject)：

* `ma` 在对象周围选择 （Vim 中为 `va`，Kakoune 中为 `<alt-a>`）
* `mi` 在对象内部选择 （Vim 中为 `vi`，Kakoune 中为 `<alt-i>`）

| 在 `mi` 或 `ma` 之后按 | 选择的文本对象       |
|------------------------|----------------------|
| `w`                    | 由空格或符号分隔的词 |
| `W`                    | 由空格分隔的词       |
| `p`                    | 段落                 |
| `(`、`[`、`'` 等       | 指定的环绕配对符号   |
| `m`                    | 最近的环绕配对符号   |
| `f`                    | 函数                 |
| `c`                    | 类                   |
| `a`                    | 参数                 |
| `o`                    | 注释                 |
| `t`                    | 测试                 |

> 注意：`f`、`c` 等功能需要当前文档的 tree-sitter 语法和特殊的 tree-sitter 查询文件才能正常工作。目前只有
> [一些语法](./lang-support.md) 有查询文件。欢迎贡献！

## 基于文本对象的导航

通过 tree-sitter 和文本对象查询，可以在函数、类、参数等之间导航。

例如，使用 `]f` 移动到下一个函数，使用 `[c` 移动到上一个类，等等。

![tree-sitter-nav-demo](https://user-images.githubusercontent.com/23398472/152332550-7dfff043-36a2-4aec-b8f2-77c13eb56d6f.gif)

对此的完整信息请参考 [unimpaired](./keymap.md#unimpaired)。

> 注意：此功能依赖于 tree-sitter 的文本对象，因此需要相应的查询文件才能正常工作。
