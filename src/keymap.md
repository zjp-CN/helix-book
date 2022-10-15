# 按键映射

> 标记为 **LSP** 的映射需要该文件类型的语言服务器。

> 标记为 **TS** 的映射需要该文件类型的 tree-sitter 语法支持。

## Normal mode

### 光标移动

> 注意：与 Vim 不同的是，`f`、`F`、`t`、`T` 并不局限于当前行。

| 按键                 | 描述                                  | 命令                        |
|----------------------|---------------------------------------|-----------------------------|
| `h`, `Left`          | 左移                                  | `move_char_left`            |
| `j`, `Down`          | 下移                                  | `move_line_down`            |
| `k`, `Up`            | 上移                                  | `move_line_up`              |
| `l`, `Right`         | 右移                                  | `move_char_right`           |
| `w`                  | 移动到下一个 word 开头                | `move_next_word_start`      |
| `b`                  | 移动到上一个 word 开头                | `move_prev_word_start`      |
| `e`                  | 移动到下一个 word 结尾                | `move_next_word_end`        |
| `W`                  | 移动到下一个 WORD 开头                | `move_next_long_word_start` |
| `B`                  | 移动到上一个 WORD 结尾                | `move_prev_long_word_start` |
| `E`                  | 移动到下一个 WORD 结尾                | `move_next_long_word_end`   |
| `t`                  | 找到下个字符之前                      | `find_till_char`            |
| `f`                  | 找到下个字符                          | `find_next_char`            |
| `T`                  | 找到上个字符之后                      | `till_prev_char`            |
| `F`                  | 找到上个字符                          | `find_prev_char`            |
| `G`                  | `nG` 表示去第 `n` 行， `n` 为数字     | `goto_line`                 |
| `Alt-.`              | 重复上次光标移动 （`f`、`t`、`m` 等） | `repeat_last_motion`        |
| `Home`               | 移动到当前行开头                      | `goto_line_start`           |
| `End`                | 移动到当前行结尾                      | `goto_line_end`             |
| `Ctrl-b`, `PageUp`   | 往上翻页                              | `page_up`                   |
| `Ctrl-f`, `PageDown` | 往下翻页                              | `page_down`                 |
| `Ctrl-u`             | 往上翻半页                            | `half_page_up`              |
| `Ctrl-d`             | 往下翻半页                            | `half_page_down`            |
| `Ctrl-i`             | 移动到跳转列表上的下一项              | `jump_forward`              |
| `Ctrl-o`             | 移动到跳转列表上的上一项              | `jump_backward`             |
| `Ctrl-s`             | 保存当前选区到跳转列表                | `save_selection`            |


### 文本修改

| 按键        | 描述                                                     | 命令                      |
|-------------|----------------------------------------------------------|---------------------------|
| `r`         | 替换为一个字符                                           | `replace`                 |
| `R`         | 替换为复制的文本                                         | `replace_with_yanked`     |
| `~`         | 切换所选文本的大小写                                     | `switch_case`             |
| `` ` ``     | 将所选文本设置为小写                                     | `switch_to_lowercase`     |
| `` Alt-` `` | 将所选文本设置为大写                                     | `switch_to_uppercase`     |
| `i`         | 在所选内容之前插入                                       | `insert_mode`             |
| `a`         | 在所选内容之后插入（追加）                               | `append_mode`             |
| `I`         | 在当前行开头插入                                         | `insert_at_line_start`    |
| `A`         | 在当前行结尾插入                                         | `insert_at_line_end`      |
| `o`         | 在所选内容下方开始新的一行                               | `open_below`              |
| `O`         | 在所选内容上方开始新的一行                               | `open_above`              |
| `.`         | 重复上次插入                                             | N/A                       |
| `u`         | 撤销修改                                                 | `undo`                    |
| `U`         | 恢复修改                                                 | `redo`                    |
| `Alt-u`     | 回到上一次历史                                           | `earlier`                 |
| `Alt-U`     | 回到下一次历史                                           | `later`                   |
| `y`         | 复制选择的内容                                           | `yank`                    |
| `p`         | 在所选内容后方粘贴                                       | `paste_after`             |
| `P`         | 在所选内容前方粘贴                                       | `paste_before`            |
| `"` `<reg>` | 选择一个寄存器把文本复制到那里或者从那粘贴               | `select_register`         |
| `>`         | 缩进所选内容                                             | `indent`                  |
| `<`         | 取消缩进所选内容                                         | `unindent`                |
| `=`         | 对所选内容格式化（目前无此功能/禁用） (**LSP**)          | `format_selections`       |
| `d`         | 删除所选内容                                             | `delete_selection`        |
| `Alt-d`     | 删除所选内容，但不复制被删除的内容                       | `delete_selection_noyank` |
| `c`         | 修改所选内容（删除并进入插入模式）                       | `change_selection`        |
| `Alt-c`     | 修改所选内容（删除并进入插入模式），但不复制被删除的内容 | `change_selection_noyank` |
| `Ctrl-a`    | 对光标下的数字自增                                       | `increment`               |
| `Ctrl-x`    | 对光标下的数字自减                                       | `decrement`               |
| `Q`         | 开始/结束录制到所选寄存器的宏（实验功能）                | `record_macro`            |
| `q`         | 从所选寄存器回放录制的宏（实验功能）                     | `replay_macro`            |

#### 对选区执行 Shell 命令

| 按键                    | 描述                                                           | 命令                  |
|-------------------------|----------------------------------------------------------------|-----------------------|
| <code>&#124;</code>     | 把每个选定内容放入管道，并将 shell 命令的输出替换掉这些内容    | `shell_pipe`          |
| <code>Alt-&#124;</code> | 把每个选定内容放入管道，并忽略掉 shell 命令的输出              | `shell_pipe_to`       |
| `!`                     | 运行 shell 命令，将其结果插入到每个选定内容之前                | `shell_insert_output` |
| `Alt-!`                 | 运行 shell 命令，将其结果插入到每个选定内容之后                | `shell_append_output` |
| `$`                     | 将每个选区通过管道传输到 shell 命令中，保留命令返回为 0 的选区 | `shell_keep_pipe`     |

### 选择文本

| 按键                 | 描述                                                   | 命令                                 |
|----------------------|--------------------------------------------------------|--------------------------------------|
| `s`                  | 在选区范围内的选择所有正则表达式匹配的内容             | `select_regex`                       |
| `S`                  | 在选区范围内的选择正则表达式匹配之外的内容             | `split_selection`                    |
| `Alt-s`              | 在多行选区中对每个非空行结尾放置一个光标               | `split_selection_on_newline`         |
| `&`                  | 按列对齐选区（先使用 `Alt-s`）                         | `align_selections`                   |
| `_`                  | 从选区中移除首尾空格来缩小选取                         | `trim_selections`                    |
| `;`                  | 把选区收缩到光标（多光标选区折叠到选区各自的光标上）   | `collapse_selection`                 |
| `Alt-;`              | 反转选区光标和锚点（对应于 Vim 的 `o`）                | `flip_selections`                    |
| `Alt-:`              | 确保选区往正文本方向（即把所有选区光标放置于选区结尾） | `ensure_selections_forward`          |
| `,`                  | 只保留主选区（多光标时收缩到主光标）                   | `keep_primary_selection`             |
| `Alt-,`              | 移除主选区（多光标时移除主光标）                       | `remove_primary_selection`           |
| `C`                  | 对下一行复制选区（多光标时往下增加一个相同位置的光标） | `copy_selection_on_next_line`        |
| `Alt-C`              | 对上一行复制选区（多光标时往上增加一个相同位置的光标） | `copy_selection_on_prev_line`        |
| `(`                  | 把上一个选区作为主选区（主选区后移）                   | `rotate_selections_backward`         |
| `)`                  | 把下一个选区作为主选区（主选区前移）                   | `rotate_selections_forward`          |
| `Alt-(`              | 把每个选区内容换成其下一个选区的内容（选区内容后移）   | `rotate_selection_contents_backward` |
| `Alt-)`              | 把每个选区内容换成其上一个选区的内容（选区内容前移）   | `rotate_selection_contents_forward`  |
| `%`                  | 选择整个文件                                           | `select_all`                         |
| `x`                  | 选择当前行；如果已选择，延伸到下一行                   | `extend_line_below`                  |
| `X`                  | 将选区扩展到行边界且 line-wise[^line-wise]             | `extend_to_line_bounds`              |
| `Alt-x`              | 将选区扩展到行边界且 line-wise                         | `shrink_to_line_bounds`              |
| `J`                  | 在选取内用空格拼接行                                   | `join_selections`                    |
| `Alt-J`              | 在选取内拼接行，但连接处使用多光标                     | `join_selections_space`              |
| `K`                  | 多选区内只保留匹配正则的选区                           | `keep_selections`                    |
| `Alt-K`              | 多选区内移除匹配正则的选区                             | `remove_selections`                  |
| `Ctrl-c`             | 注释/取消注释所选内容                                  | `toggle_comments`                    |
| `Alt-o`, `Alt-up`    | 将所选内容拓展到上一级父语法节点 (**TS**)              | `expand_selection`                   |
| `Alt-i`, `Alt-down`  | 将所选内容收缩语法节点 (**TS**)                        | `shrink_selection`                   |
| `Alt-p`, `Alt-left`  | 选择语法树中的上一个同级节点 (**TS**)                  | `select_prev_sibling`                |
| `Alt-n`, `Alt-right` | 选择语法树中的下一个同级节点 (**TS**)                  | `select_next_sibling`                |

[^line-wise]: 译者注：`X` 与 `x` 的区别在于，跨行拓展时，`x` 总是把光标放于末尾，而 `X` 会感知选区的方向，这被称为
line-wise：如果光标在选区往下的方向，那么选区拓展后的光标位于末尾；如果光标在选区往上的方向，那么选区拓展后的光标位于末尾。
关于选区的方向，你可以参考我的理解：

```text
╭─────╮                ╭─────╮               ╭─────╮
│text1│ <── backward ──│text0│── forward ──> │text1│
╰─────╯                ╰─────╯               ╰─────╯

╭─────╮
│text1│
╰─────╯
   ↑
backward：选区往上/往后/反向
   |
╭─────╮
│text0│
╰─────╯
   |
forward：选区往下/往前/正向
   ↓
╭─────╮
│text1│
╰─────╯
```

### 搜索文本

默认情况下，搜索命令都在 `/` 寄存器上操作。使用 `"<char>` 来操作不同的寄存器。

| 按键 | 描述                                     | 命令               |
|------|------------------------------------------|--------------------|
| `/`  | 文本正方向正则搜索                       | `search`           |
| `?`  | 文本反方向正则搜索                       | `rsearch`          |
| `n`  | 选择下一个匹配到的搜索内容（选区会增加） | `search_next`      |
| `N`  | 选择下一个匹配到的搜索内容（选区会增加） | `search_prev`      |
| `*`  | 使用当前选中的文本作为搜索模式           | `search_selection` |

### Minor modes

这些子模式可从正常模式访问，通常在命令结束后切换回正常模式。

| 按键     | 描述                                              | 命令           |
|----------|---------------------------------------------------|----------------|
| `v`      | 进入 [select (extend) mode](#select--extend-mode) | `select_mode`  |
| `g`      | 进入 [goto mode](#goto-mode)                      | N/A            |
| `m`      | 进入 [match mode](#match-mode)                    | N/A            |
| `:`      | 进入 command mode                                 | `command_mode` |
| `z`      | 进入 [view mode](#view-mode)                      | N/A            |
| `Z`      | 进入 sticky [view mode](#view-mode)               | N/A            |
| `Ctrl-w` | 进入 [window mode](#window-mode)                  | N/A            |
| `Space`  | 进入 [space mode](#space-mode)                    | N/A            |

#### View mode

view 模式用于在不更改选区的情况下滚动和操作视图。

这种模式的 sticky （按 `Z`）方式是持久的：需使用 `Esc` 键返回到正常模式。当你只是浏览文本而不是主动编辑它时，这一方式很有用。

| Key                  | Description                | Command             |
|----------------------|----------------------------|---------------------|
| `z`, `c`             | 垂直居中当前行             | `align_view_center` |
| `t`                  | 将当前行与屏幕顶部对齐     | `align_view_top`    |
| `b`                  | 将当前行与屏幕底部对齐     | `align_view_bottom` |
| `m`                  | 将当前行与屏幕中间水平对齐 | `align_view_middle` |
| `j`, `down`          | 向下滚动视图               | `scroll_down`       |
| `k`, `up`            | 向上滚动视图               | `scroll_up`         |
| `Ctrl-f`, `PageDown` | 向下翻页                   | `page_down`         |
| `Ctrl-b`, `PageUp`   | 向上翻页                   | `page_up`           |
| `Ctrl-d`             | 向下翻半页                 | `half_page_down`    |
| `Ctrl-u`             | 向上翻半页                 | `half_page_up`      |

#### Goto mode

按 `g` 进入此模式，来跳跃到不同的位置。

| 按键 | 描述                                                   | 命令                       |
|------|--------------------------------------------------------|----------------------------|
| `g`  | 输入 `gng` 跳转到第 n 行[^nG]；不输入数字跳转到第 1 行 | `goto_file_start`          |
| `e`  | 到最后一行                                             | `goto_last_line`           |
| `f`  | 到所选文件[^gf]                                        | `goto_file`                |
| `h`  | 到当前行开头                                           | `goto_line_start`          |
| `l`  | 到当前行结尾                                           | `goto_line_end`            |
| `s`  | 到当前行第一个非空格字符                               | `goto_first_nonwhitespace` |
| `t`  | 到屏幕顶部那行                                         | `goto_window_top`          |
| `c`  | 到屏幕中间那行                                         | `goto_window_center`       |
| `b`  | 到屏幕底部那行                                         | `goto_window_bottom`       |
| `d`  | 跳转到定义 (**LSP**)                                   | `goto_definition`          |
| `y`  | 跳转到类型定义 (**LSP**)                               | `goto_type_definition`     |
| `r`  | 跳转到引用 (**LSP**)                                   | `goto_reference`           |
| `i`  | 跳转到实现 (**LSP**)                                   | `goto_implementation`      |
| `a`  | 到上次访问的/备选文件                                  | `goto_last_accessed_file`  |
| `m`  | 到上次修改的/备选文件                                  | `goto_last_modified_file`  |
| `n`  | 到下个缓冲区                                           | `goto_next_buffer`         |
| `p`  | 到上个缓冲区                                           | `goto_previous_buffer`     |
| `.`  | 到当前文件中的最后一次修改处                           | `goto_last_modification`   |

[^nG]: `gng` 等价于 `nG`，都用于跳转到第 n 行。

[^gf]: `gf` 会将所选内容视为文件路径（可以是相对路径也可以是绝对路径）；当该路径不存在，打开那个路径的缓冲区，写入即创建该文件（不写入不创建）。

#### Match mode

在 normal 模式按 `m` 进入该模式。有关 [环绕](./usage.html#环绕) 和 [文本对象](./usage.html#文本对象) 的解释，请参阅 [使用](./usage.md) 的相关部分。

| 按键             | 描述                                 | 命令                       |
|------------------|--------------------------------------|----------------------------|
| `m`              | 到匹配的括号 (**TS**)                | `match_brackets`           |
| `s` `<char>`     | 用将当前选定内容用 `<char>` 包围起来 | `surround_add`             |
| `r` `<from><to>` | 把环绕的 `<from>` 字符替换成 `<to>`  | `surround_replace`         |
| `d` `<char>`     | 删除环绕的 `<char>`                  | `surround_delete`          |
| `a` `<object>`   | 删除 textobject 文本                 | `select_textobject_around` |
| `i` `<object>`   | 删除 textobject 内部的文本           | `select_textobject_inner`  |

TODO：选择语法节点的映射（`[` 的超集）。

#### Window mode

这部分类似于 Vim 键绑定，因为 Kakoune 不支持窗口。按 `<space>w` 或者 `<Ctrl-w>` 进入此模式。

| 按键                   | 描述                               | 命令              |
|------------------------|------------------------------------|-------------------|
| `w`, `Ctrl-w`          | 切换到下一个窗口                   | `rotate_view`     |
| `v`, `Ctrl-v`          | 垂直向右拆分                       | `vsplit`          |
| `s`, `Ctrl-s`          | 水平底部拆分                       | `hsplit`          |
| `f`                    | 以水平拆分方式转到所选内容中的文件 | `goto_file`       |
| `F`                    | 以垂直拆分方式转到所选内容中的文件 | `goto_file`       |
| `h`, `Ctrl-h`, `Left`  | 移动光标到左侧拆分窗口             | `jump_view_left`  |
| `j`, `Ctrl-j`, `Down`  | 移动光标到下侧拆分窗口             | `jump_view_down`  |
| `k`, `Ctrl-k`, `Up`    | 移动光标到上侧拆分窗口             | `jump_view_up`    |
| `l`, `Ctrl-l`, `Right` | 移动光标到右侧拆分窗口             | `jump_view_right` |
| `q`, `Ctrl-q`          | 关闭当前窗口                       | `wclose`          |
| `o`, `Ctrl-o`          | 仅保留当前窗口，关闭所有其他窗口   | `wonly`           |
| `H`                    | 交换当前窗口到左侧[^window-H]      | `swap_view_left`  |
| `J`                    | 交换当前窗口到下侧                   | `swap_view_down`  |
| `K`                    | 交换当前窗口到上侧                 | `swap_view_up`    |
| `L`                    | 交换当前窗口到右侧                 | `swap_view_right` |

[^window-H]: 译者注：目前仅在左右拆分启用。下面几个也一样。

#### Space mode

该部分是一个杂乱无章的映射，主要是 picker。按 `<space>` 进入此模式。

| 按键 | 描述                                                  | 命令                                |
|------|-------------------------------------------------------|-------------------------------------|
| `f`  | 打开文件选取器                                        | `file_picker`                       |
| `F`  | 打开当前项目目录的文件选取器                          | `file_picker_in_current_directory`  |
| `b`  | 打开缓冲区选取器                                      | `buffer_picker`                     |
| `j`  | 打开跳转列表选取器                                    | `jumplist_picker`                   |
| `k`  | 在 [popup](#popup) 框中显示光标下条目的文档 (**LSP**) | `hover`                             |
| `s`  | 打开当前文档符号选取器 (**LSP**)                      | `symbol_picker`                     |
| `S`  | 打开工作区符号选取器 (**LSP**)                        | `workspace_symbol_picker`           |
| `g`  | 打开当前文档代码诊断选取器 (**LSP**)                  | `diagnostics_picker`                |
| `G`  | 打开工作区代码诊断选取器 (**LSP**)                    | `workspace_diagnostics_picker`      |
| `r`  | 重命名符号 (**LSP**)                                  | `rename_symbol`                     |
| `a`  | 执行代码操作  (**LSP**)                               | `code_action`                       |
| `'`  | 打开上次的模糊选取器                                  | `last_picker`                       |
| `w`  | 进入 [window mode](#window-mode)                      | N/A                                 |
| `p`  | 在选区后方粘贴系统剪贴板的内容                        | `paste_clipboard_after`             |
| `P`  | 在选区前方粘贴系统剪贴板的内容                        | `paste_clipboard_before`            |
| `y`  | 复制所选文本到粘贴板                                  | `yank_joined_to_clipboard`          |
| `Y`  | （多选区时）复制主选区到粘贴板                        | `yank_main_selection_to_clipboard`  |
| `R`  | 将所选文本替换成系统粘贴板的文本                      | `replace_selections_with_clipboard` |
| `/`  | 在工作区文件夹下全局搜索文本                          | `global_search`                     |
| `?`  | 打开命令选项板                                        | `command_palette`                   |

> 提示：全局搜索虽然使用命令行输入，但在模糊选取器中显示结果，所以你可以在打开文件后使用 `<space>'` 将上次搜索的结果其带回。

#### Popup

显示光标下条目的文档。

| 按键     | 描述     |
|----------|----------|
| `Ctrl-u` | 向上滚动 |
| `Ctrl-d` | 向下滚动 |

#### Unimpaired

使用 [vim-unimpaired](https://github.com/tpope/vim-unimpaired) 风格的映射来代码导航。

| 按键     | 描述                             | 命令                  |
|----------|----------------------------------|-----------------------|
| `[d`     | 到上一个诊断 (**LSP**)           | `goto_prev_diag`      |
| `]d`     | 到下一个诊断 (**LSP**)           | `goto_next_diag`      |
| `[D`     | 到本文件的第一个诊断 (**LSP**)   | `goto_first_diag`     |
| `]D`     | 到本文件的最后一个诊断 (**LSP**) | `goto_last_diag`      |
| `]f`     | 到下一个函数 (**TS**)            | `goto_next_function`  |
| `[f`     | 到上一个函数 (**TS**)            | `goto_prev_function`  |
| `]c`     | 到下一个类 (**TS**)              | `goto_next_class`     |
| `[c`     | 到上一个类 (**TS**)              | `goto_prev_class`     |
| `]a`     | 到下一个参数 (**TS**)            | `goto_next_parameter` |
| `[a`     | 到上一个参数 (**TS**)            | `goto_prev_parameter` |
| `]o`     | 到下一个注释 (**TS**)            | `goto_next_comment`   |
| `[o`     | 到上一个注释 (**TS**)            | `goto_prev_comment`   |
| `]t`     | 到下一个测试 (**TS**)            | `goto_next_test`      |
| `]t`     | 到上一个测试 (**TS**)            | `goto_prev_test`      |
| `]p`     | 到下一个段落                     | `goto_next_paragraph` |
| `[p`     | 到上一个段落                     | `goto_prev_paragraph` |
| `[Space` | 在上面添加新的一行               | `add_newline_above`   |
| `]Space` | 在下面添加新的一行               | `add_newline_below`   |

## Insert mode

默认情况下，insert mode 绑定的按键在某种程度上是最少的。 Helix 被设计成一个模式编辑器，这反映在用户体验和内部机制上。

例如，只有在从 insert mode 退出到 normal mode 时，才会保存对文本所做的更改以供撤消。出于这个原因，强烈鼓励新用户学习模式编辑范例，以获得最流畅的体验。

| 按键                      | 描述             | 命令                     |
|---------------------------|------------------|--------------------------|
| `Escape`                  | 切换到正常模式   | `normal_mode`            |
| `Ctrl-s`                  | 提交撤消检查点   | `commit_undo_checkpoint` |
| `Ctrl-x`                  | 自动补全         | `completion`             |
| `Ctrl-r`                  | 插入寄存器的内容 | `insert_register`        |
| `Ctrl-w`, `Alt-Backspace` | 删除上一个单词   | `delete_word_backward`   |
| `Alt-d`, `Alt-Delete`     | 删除下一个单词   | `delete_word_forward`    |
| `Ctrl-u`                  | 删除到行首       | `kill_to_line_start`     |
| `Ctrl-k`                  | 删除到行尾       | `kill_to_line_end`       |
| `Ctrl-h`, `Backspace`     | 删除上一个字符   | `delete_char_backward`   |
| `Ctrl-d`, `Delete`        | 删除下一个字符   | `delete_char_forward`    |
| `Ctrl-j`, `Enter`         | 插入新行         | `insert_newline`         |

不推荐使用这些快捷键，只是为不太熟悉模式编辑器的新用户提供这些。

如果你希望在更习惯使用模式编辑时在 insert mode 下禁用它们，则在 `config.toml` 中使用以下命令：

```toml
[keys.insert]
up = "no_op"
down = "no_op"
left = "no_op"
right = "no_op"
pageup = "no_op"
pagedown = "no_op"
home = "no_op"
end = "no_op"
```

## Select / extend mode

按 `v` 进入和退出此模式，此模式类似于 normal mode，但会更改任意移动以扩展选区，而不是替换这些选区。goto 移动也被更改为扩展，例如，`vgl` 将所选内容扩展到行尾。

搜索也受到了影响。默认情况下，`n` 和 `N` 会移除当前选区，并选择搜索词的下一个实例。

在按 `n` 或 `N` 之前切换此模式可以保持当前选区。在迭代式搜索时打开和关闭这个模式，可让你有选择地将搜索项添加到选区中。[^select-mode]

[^select-mode]: 译者注：例如选择 `abc`，按 `*` 将搜索 `abc`，并进入 select mode，所以按 `n` 会把下个搜索结果添加进选区，按 `v` 退回
normal mode，所以按 `n` 不再把下个搜索结果添加进选区（但仍会保留已存在的选区）。

## Picker

在选取器中使用的按键。当前不支持重新映射这些按键。

| 按键                        | 描述               |
|-----------------------------|--------------------|
| `Shift-Tab`, `Up`, `Ctrl-p` | 前一条             |
| `Tab`, `Down`, `Ctrl-n`     | 后一条             |
| `PageUp`, `Ctrl-u`          | 往上翻页           |
| `PageDown`, `Ctrl-d`        | 往下翻页           |
| `Home`                      | 到第一条           |
| `End`                       | 到最后一条         |
| `Enter`                     | 打开所选项         |
| `Ctrl-s`                    | 垂直拆分窗口再打开 |
| `Ctrl-v`                    | 水平拆分窗口再打开 |
| `Ctrl-t`                    | 切换预览           |
| `Escape`, `Ctrl-c`          | 关闭选取器         |

## Prompt

在提示框（比如按 `s` 在命令行弹出待输入的那个位置）内使用的按键，当前不支持重新映射。

| 按键                                        | 描述                                            |
|---------------------------------------------|-------------------------------------------------|
| `Escape`, `Ctrl-c`                          | 关闭提示框                                      |
| `Alt-b`, `Ctrl-Left`                        | 到上一个 word （nomal mode 下的 `b`）           |
| `Ctrl-b`, `Left`                            | 到上一个 char （nomal mode 下的 `h`）           |
| `Alt-f`, `Ctrl-Right`                       | 到下一个 word                                   |
| `Ctrl-f`, `Right`                           | 到下一个 char                                   |
| `Ctrl-e`, `End`                             | 到行结尾                                        |
| `Ctrl-a`, `Home`                            | 到行开头                                        |
| `Ctrl-w`, `Alt-Backspace`, `Ctrl-Backspace` | 删除前一个 word                                 |
| `Alt-d`, `Alt-Delete`, `Ctrl-Delete`        | 删除下一个 word                                 |
| `Ctrl-u`                                    | 删除到行开头                                    |
| `Ctrl-k`                                    | 删除到行结尾                                    |
| `Backspace`, `Ctrl-h`                       | 删除前一个 char                                 |
| `Delete`, `Ctrl-d`                          | 删除下一个 char                                 |
| `Ctrl-s`                                    | 载光标下插入一个，可能以后会改成  Ctrl-r Ctrl-w |
| `Ctrl-p`, `Up`                              | 选择上一个历史                                  |
| `Ctrl-n`, `Down`                            | 选择下一个历史                                  |
| `Ctrl-r`                                    | 插入所选寄存器的内容                            |
| `Tab`                                       | 选择下一个补全项                                |
| `BackTab`                                   | 选择上一个补全项                                |
| `Enter`                                     | 打开选定项                                      |

[^Ctrl-r]: 译者注：会弹出寄存器及其内容，输入寄存器的名称即插入其内容。即按 `Ctrl-r"` 将 `"` 寄存器的内容插入到光标下。
