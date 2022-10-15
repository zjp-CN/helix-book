# 重新映射按键

通过一个简单的 TOML 配置文件暂时支持单向键重新映射。（未来将更强大的解决方案，如通过命令重新绑定）。

要重映射按键，请在你的配置目录（Linux 系统中默认为 `~/.config/helix`）下建一个 `config.toml` 文件，其结构如下：

```toml
# At most one section each of 'keys.normal', 'keys.insert' and 'keys.select'
[keys.normal]
C-s = ":w" # Maps the Ctrl-s to the typable command :w which is an alias for :write (save file)
C-o = ":open ~/.config/helix/config.toml" # Maps the Ctrl-o to opening of the helix config file
a = "move_char_left" # Maps the 'a' key to the move_char_left command
w = "move_line_up" # Maps the 'w' key move_line_up
"C-S-esc" = "extend_line" # Maps Ctrl-Shift-Escape to extend_line
g = { a = "code_action" } # Maps `ga` to show possible code actions
"ret" = ["open_below", "normal_mode"] # Maps the enter key to open_below then re-enter normal mode

[keys.insert]
"A-x" = "normal_mode" # Maps Alt-X to enter normal mode
j = { k = "normal_mode" } # Maps `jk` to exit insert mode
```

> 注意：可输入的命令也可以重新映射，记住保留 `:` 前缀以表示它是可输入的命令。

Ctrl、Shift 和 Alt 修饰符分别使用 `C-`、`S-` 和 `A-` 前缀进行编码。特殊按键的编码如下：

| 按键名称     | 编码        |
|--------------|-------------|
| Backspace    | `backspace` |
| Space        | `space`     |
| Return/Enter | `ret`       |
| \-           | `minus`     |
| Left         | `left`      |
| Right        | `right`     |
| Up           | `up`        |
| Down         | `down`      |
| Home         | `home`      |
| End          | `end`       |
| Page Up      | `pageup`    |
| Page Down    | `pagedown`  |
| Tab          | `tab`       |
| Delete       | `del`       |
| Insert       | `ins`       |
| Null         | `null`      |
| Escape       | `esc`       |

可以通过绑定 `no_op` 命令来禁用按键。

可以在 [按键映射](./keymap.md) 一章中找命令。

> 也可以在 [`helix-term/src/Commands.rs`] 源代码中的 `static_commands!` 宏和 `TypableCommandList` 下，找命令。

[`helix-term/src/commands.rs`]: https://github.com/helix-editor/helix/blob/master/helix-term/src/commands.rs
