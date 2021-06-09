# 如何贡献 API 编写指南

Rust API 编写指南 项目欢迎任何人
以提建议、提交 bug 报告、提 PR 、提出反馈的方式，来贡献自己的力量。
如果你正在考虑帮助我们，这篇文档给你一些指导。

如果可以的话，请到 Github [issue] 或 [Gitter] 频道 联系我们。

[issue]: https://github.com/rust-lang/api-guidelines/issues
[Gitter]: https://gitter.im/rust-impl-period/WG-libs-guidelines

## 提交新指南

我们一直寻找来自高质量的 Rust 库的编写经验。
如果你对 crate API 有洞见，想让其他 crates 从中受益，
请开启一个 [讨论][open a discussion] 让我们知道。

如果你想对现有的指南进行具体的修正，
也可以提交一个 [issue][open a issue] 。

[open a discussion]: https://github.com/rust-lang/api-guidelines/discussions/new
[open an issue]: https://github.com/rust-lang/api-guidelines/issues/new

## 编写指南内容

指南以一系列 Markdown 文件的形式放置在 [`src`] 目录。
当做出某些修改的时候，你可以使用 [mdBook] 来预览渲染的内容。

[`src`]: https://github.com/rust-lang/api-guidelines/tree/master/src
[mdBook]: https://github.com/azerupi/mdBook

```sh
cargo install mdbook

# 在 api-guidelines 目录
mdbook serve
```

使用 `mdbook serve` 命令可以让渲染的 API 编写指南一直在
http://localhost:3000/ 页面可浏览。

## 对编写指南的建议

我们遵循一些语法方面的规则来确保指南的一览表风格保持一致和内容清晰可读。

一条指南是对假想 crate 的 **陈述说明** 。

```diff
  不要用祈使语气：
- "Implement Hex, Octal, Binary for binary number types"
  使用陈述语气：
+ "Binary number types provide Hex, Octal, Binary formatting"

  不要使用命令：
- "Macros should compose well with attributes"
  请使用陈述：
+ "Macros compose well with attributes"
```

指南具有明显的 **主语** 和 **动词** 。

```diff
  不要无主语：
- "Includes all common Cargo.toml metadata"
  请添加主语：
+ "Cargo.toml includes all common metadata"

  不要无动词：
- "Thoroughly documented with examples"
  请添加动词：
+ "Crate level docs are thorough and include examples"

  不要用模糊的句式：
- "There are no out-parameters"
  请使用具体的句式：
+ "Functions do not take out-parameters"
```

指南使用 **主动语态** 。

```diff
  不要用被动语态：
- "Function arguments are validated"
  请使用主动语态：
+ "Functions validate their arguments"
```

## 规范

在所有与 Rust 有关的领域，我们都遵循 [Rust 代码准则][Rust Code of Conduct] 。
与这方面有关的问题，请联系 Rust moderation 团队： rust-mods@rust-lang.org 。

[Rust Code of Conduct]: https://www.rust-lang.org/policies/code-of-conduct
