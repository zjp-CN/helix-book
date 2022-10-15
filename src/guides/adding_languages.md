# 添加语言服务

## 语言配置

要添加一种新语言，需要在 `languages.toml` 中添加一项 `[[language]]`，见 [语言配置][language configuration section]。

为现有语言添加新语言或语言服务器配置时，请在创建 PR 之前，运行 `Cargo xtask docgen`，来将新配置添加到 [语言支持][lang-support] 文档。

添加语言服务器配置时，请确保更新 [语言服务器 Wiki][install-lsp-wiki]。

## 语法配置

如果有适用于该语言的 tree-sitter 语法，请在 `languages.toml` 中添加一个新的 `[[grammar]]` 项。

你可以使用 `source.path` 键而不是指向本地语法仓库绝对路径的 `source.git` 来进行测试，但在提交 PR 之前，请切换到 `source.git`。

## 查询

你必须对具有语法高亮和缩进等功能的语言添加查询。为此，添加一个目录，路径为 `runtime/queries/<name>/`。

tree-sitter 网站提供了更多关于 [如何编写查询](https://tree-sitter.github.io/tree-sitter/syntax-highlighting#queries) 的信息。

> 注意：在计算查询时，第一个匹配的查询优先，这与 Neovim 等其他编辑器不同，在 Neovim
> 中，最后一个匹配的查询优先于它之前的查询。例子见 [该 issue][neovim-query-precedence]。

## 常见问题

* 如果在切换分支后运行时出错，则可能需要更新 tree-sitter 语法。运行 `hx --grammar fetch` 来获取语法，运行
  `hx --grammar build` 来构建任何过时的语法。
* 如果解析器出现分段错误(segfaulting)，或者你想删除解析器，请确保在 `runtime/grammar/<name>.so` 中删除已编译的解析器。

[language configuration section]: ../languages.md
[neovim-query-precedence]: https://github.com/helix-editor/helix/pull/1170#issuecomment-997294090
[install-lsp-wiki]: https://github.com/helix-editor/helix/wiki/How-to-install-the-default-language-servers
[lang-support]: ../lang-support.md
