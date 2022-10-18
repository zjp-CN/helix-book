# 安装

GitHub 发布页面上[提供了](https://github.com/helix-editor/helix/releases)预构建的二进制文件。

[![Packaging status](https://repology.org/badge/vertical-allrepos/helix.svg)](https://repology.org/project/helix/versions)

## OSX

Helix 在 homebrew-core 中可用：

```bash
brew install helix
```

## Linux

### NixOS

[flake]: https://nixos.wiki/wiki/Flakes
[Cachix]: https://www.cachix.org/
[Cachix cli]: https://docs.cachix.org/installation

在项目根目录中提供了包含 Helix 包的 [flake]。它还重现开发 shell 环境，用来与 `nix develop` 一起在 Helix 上工作。

每次使用 [Cachix] 推送到 master 时，flake 的输出都被缓存。如果使用者在第一次使用时接受新设置，则 flake 被配置为自动使用该缓存。

如果你使用的 Nix 版本没有启用 flake，则可以安装 [Cachix cli]；`cachix use helix` 会将 Nix 配置为尽可能使用缓存输出。

### Arch Linux

`community` 仓库中可用。构建 master 的 AUR 上也提供了 [helix-git] 包。

[helix-git]: https://aur.archlinux.org/packages/helix-git

### Fedora Linux

通过以下方式安装 Helix 的 COPR 包

```bash
sudo dnf copr enable varlad/helix
sudo dnf install helix
```

### Void Linux

```bash
sudo xbps-install helix
```

## 从源代码构建

```bash
git clone https://github.com/helix-editor/helix
cd helix
cargo install --path helix-term
# 译者注：除非你开发 tree-sitter，否则建议源码安装时使用环境变量 `HELIX_DISABLE_AUTO_GRAMMAR_BUILD=1` 来避免构建 tree-sitter
# HELIX_DISABLE_AUTO_GRAMMAR_BUILD=1 cargo install --path helix-term
```

这会将 `hx` 二进制文件安装到 `$HOME/.cargo/bin`。

Helix 还需要它的运行时文件，因此请确保将 `runtime/` 目录复制或者 symlink 到配置目录。例如使用如下命令：

| OS                  | 命令                                             |
| ------------------- | ------------------------------------------------ |
| windows(cmd.exe)    | `xcopy /e /i runtime %AppData%/helix/runtime`    |
| windows(powershell) | `xcopy /e /i runtime $Env:AppData\helix\runtime` |
| linux/macos         | `ln -s $PWD/runtime ~/.config/helix/runtime`     |

还可以通过 `HELIX_RUNTIME` 环境变量覆盖此位置。

要在支持 [XDG 桌面菜单][XDG desktop menu]的桌面环境（包括 Gnome 和 KDE）中使用 Helix，请将提供的 `.desktop` 文件复制到正确的文件夹中：

[XDG desktop menu]: https://specifications.freedesktop.org/menu-spec/menu-spec-latest.html

```bash
cp contrib/Helix.desktop ~/.local/share/applications
```

要使用默认终端之外的其他终端，你需要修改 `.desktop` 文件。例如，使用 `kitty`：

```bash
sed -i "s|Exec=hx %F|Exec=kitty hx %F|g" ~/.local/share/applications/Helix.desktop
sed -i "s|Terminal=true|Terminal=false|g" ~/.local/share/applications/Helix.desktop
```

请注意：Helix 还没有图标，所以将使用系统默认设置。

## 完成安装

为了确保一切都按预期进行，最后你应该运行检查命令：

```bash
hx --health
```

关于检查结果中显示的信息的详细信息，见 [状况检查][healthcheck]。

[healthcheck]: https://github.com/helix-editor/helix/wiki/Healthcheck

### 构建 tree-sitter 语法

如果没有预先打包 tree-sitter 语法工具，则必须获取和和编译它。[^tree-sitter]

使用 `hx --grammar fetch`（需要 `git`）获取语法，并使用 `hx --grammar build`（需要 C++ 编译器）编译它们。

[^tree-sitter]: 译者注：`runtime/grammars`（里面是静态库等文件，全部语言加起来占几百M） 和
`runtime/queries` （里面是 scm 文件） 用于语法高亮、textobject、自动缩进等功能，没有它们，你的代码文件只有
LSP 功能，而且看起来是纯文本，见[此处讨论](https://github.com/helix-editor/helix/discussions/4345)。

### 安装语言服务器

如果你需要自动补全、代码诊断等功能，则安装语言服务器。按照 Hexlix Wiki 页面上的说明[添加语言服务器][language-servers]。

[language-servers]: https://github.com/helix-editor/helix/wiki/How-to-install-the-default-language-servers

