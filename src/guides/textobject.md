# 添加文本对象查询

文本对象 ([textobjects]) 是特定于语言的，比如函数、类等。它需要 tree-sitter 语法和 `textobjects.scm`
查询文件才能正常工作。 tree-sitter 让我们查询源代码语法树，并捕获特定的部分。

查询 (query) 是以 lisp 方言的方式写的。更多关于如何编写查询的信息可参考官方 tree-sitter [文档][tree-sitter-queries]。

贡献时，查询文件应放置于 `runtime/queries/{language}/textobjects.scm`。

注意，你应该将它们放置在你本地的运行时目录 （Linux 下为 `~/.config/helix/runtime`） 下来测试查询文件。

会识别以下捕获 ([captures][tree-sitter-captures])：

| 捕获名称           |
|--------------------|
| `function.inside`  |
| `function.around`  |
| `class.inside`     |
| `class.around`     |
| `test.inside`      |
| `test.around`      |
| `parameter.inside` |
| `comment.inside`   |
| `comment.around`   |

Helix 的 github 仓库下有[示例][textobject-examples]。

## 基于导航的文本对象查询

[基于 tree-sitter 的导航][textobjects-nav] 使用以下捕获顺序做到：

- `object.movement`
- `object.around`
- `object.inside`

比如如果在某个语言的 `textobjects.scm` 文件中定义了 `function.around` 捕获，那么函数导航应该自动工作。只在
`function.around` 所捕获的节点不再导航上下文中起作用时，才应该定义 `function.movement`。

[textobjects]: ../usage.md#textobjects
[textobjects-nav]: ../usage.md#tree-sitter-textobject-based-navigation
[tree-sitter-queries]: https://tree-sitter.github.io/tree-sitter/using-parsers#query-syntax
[tree-sitter-captures]: https://tree-sitter.github.io/tree-sitter/using-parsers#capturing-nodes
[textobject-examples]: https://github.com/search?q=repo%3Ahelix-editor%2Fhelix+filename%3Atextobjects.scm&type=Code&ref=advsearch&l=&l=
