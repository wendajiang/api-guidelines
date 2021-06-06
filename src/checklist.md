# Rust API 指导清单

<!-- Read CONTRIBUTING.md before writing new guidelines -->

- **命名** *(crate 遵照 Rust 命名规范)*
  - 大小写规范 RFC 430 ([C-CASE])
  - 遵循 `as_`, `to_`, `into_` 规范 用以特定类型转换 ([C-CONV])
  - getter 命名规范 ([C-GETTER])
  - 遵循 `iter`, `iter_mut`, `into_iter` 规范 用以生成迭代器 ([C-ITER])
  - 生成迭代器的方法与迭代器类型同名 ([C-ITER-TY])
  - cargo feature 名中不应该有无意义的词 ([C-FEATURE])
  - 词性顺序一致 ([C-WORD-ORDER])
- **互通互用** *(crate 很好地与其他库提供的功能进行交互)*
  - 类型应尽早实现常见的 traits ([C-COMMON-TRAITS])
    - `Copy`, `Clone`, `Eq`, `PartialEq`, `Ord`, `PartialOrd`, `Hash`, `Debug`,
      `Display`, `Default`
  - 使用 `From`, `AsRef`, `AsMut` trait 来转换类型 ([C-CONV-TRAITS])
  - 给集合实现 `FromIterator` 和 `Extend` trait ([C-COLLECT])
  - 给数据结构实现 Serde 的 `Serialize` 和 `Deserialize` trait ([C-SERDE])
  - 类型应尽可能实现 `Send` 和 `Sync` trait ([C-SEND-SYNC])
  - Error 类型 十分直观和有用 ([C-GOOD-ERR])
  - 二进制数类型应提供 `Hex`, `Octal`, `Binary` 的格式化方式 ([C-NUM-FMT])
  - reader/writer 泛型函数使用 `R: Read` 和 `W: Write` 参数传值 ([C-RW-VALUE])
- **Macros** *(crate presents well-behaved macros)*
  - Input syntax is evocative of the output ([C-EVOCATIVE])
  - Macros compose well with attributes ([C-MACRO-ATTR])
  - Item macros work anywhere that items are allowed ([C-ANYWHERE])
  - Item macros support visibility specifiers ([C-MACRO-VIS])
  - Type fragments are flexible ([C-MACRO-TY])
- **Documentation** *(crate is abundantly documented)*
  - Crate level docs are thorough and include examples ([C-CRATE-DOC])
  - All items have a rustdoc example ([C-EXAMPLE])
  - Examples use `?`, not `try!`, not `unwrap` ([C-QUESTION-MARK])
  - Function docs include error, panic, and safety considerations ([C-FAILURE])
  - Prose contains hyperlinks to relevant things ([C-LINK])
  - Cargo.toml includes all common metadata ([C-METADATA])
    - authors, description, license, homepage, documentation, repository,
      readme, keywords, categories
  - Crate sets html_root_url attribute "https://docs.rs/CRATE/X.Y.Z" ([C-HTML-ROOT])
  - Release notes document all significant changes ([C-RELNOTES])
  - Rustdoc does not show unhelpful implementation details ([C-HIDDEN])
- **Predictability** *(crate enables legible code that acts how it looks)*
  - Smart pointers do not add inherent methods ([C-SMART-PTR])
  - Conversions live on the most specific type involved ([C-CONV-SPECIFIC])
  - Functions with a clear receiver are methods ([C-METHOD])
  - Functions do not take out-parameters ([C-NO-OUT])
  - Operator overloads are unsurprising ([C-OVERLOAD])
  - Only smart pointers implement `Deref` and `DerefMut` ([C-DEREF])
  - Constructors are static, inherent methods ([C-CTOR])
- **Flexibility** *(crate supports diverse real-world use cases)*
  - Functions expose intermediate results to avoid duplicate work ([C-INTERMEDIATE])
  - Caller decides where to copy and place data ([C-CALLER-CONTROL])
  - Functions minimize assumptions about parameters by using generics ([C-GENERIC])
  - Traits are object-safe if they may be useful as a trait object ([C-OBJECT])
- **Type safety** *(crate leverages the type system effectively)*
  - Newtypes provide static distinctions ([C-NEWTYPE])
  - Arguments convey meaning through types, not `bool` or `Option` ([C-CUSTOM-TYPE])
  - Types for a set of flags are `bitflags`, not enums ([C-BITFLAG])
  - Builders enable construction of complex values ([C-BUILDER])
- **Dependability** *(crate is unlikely to do the wrong thing)*
  - Functions validate their arguments ([C-VALIDATE])
  - Destructors never fail ([C-DTOR-FAIL])
  - Destructors that may block have alternatives ([C-DTOR-BLOCK])
- **Debuggability** *(crate is conducive to easy debugging)*
  - All public types implement `Debug` ([C-DEBUG])
  - `Debug` representation is never empty ([C-DEBUG-NONEMPTY])
- **Future proofing** *(crate is free to improve without breaking users' code)*
  - Sealed traits protect against downstream implementations ([C-SEALED])
  - Structs have private fields ([C-STRUCT-PRIVATE])
  - Newtypes encapsulate implementation details ([C-NEWTYPE-HIDE])
  - Data structures do not duplicate derived trait bounds ([C-STRUCT-BOUNDS])
- **Necessities** *(to whom they matter, they really matter)*
  - Public dependencies of a stable crate are stable ([C-STABLE])
  - Crate and its dependencies have a permissive license ([C-PERMISSIVE])


[C-CASE]: naming.html#c-case
[C-CONV]: naming.html#c-conv
[C-GETTER]: naming.html#c-getter
[C-ITER]: naming.html#c-iter
[C-ITER-TY]: naming.html#c-iter-ty
[C-FEATURE]: naming.html#c-feature
[C-WORD-ORDER]: naming.html#c-word-order

[C-COMMON-TRAITS]: interoperability.html#c-common-traits
[C-CONV-TRAITS]: interoperability.html#c-conv-traits
[C-COLLECT]: interoperability.html#c-collect
[C-SERDE]: interoperability.html#c-serde
[C-SEND-SYNC]: interoperability.html#c-send-sync
[C-GOOD-ERR]: interoperability.html#c-good-err
[C-NUM-FMT]: interoperability.html#c-num-fmt
[C-RW-VALUE]: interoperability.html#c-rw-value

[C-EVOCATIVE]: macros.html#c-evocative
[C-MACRO-ATTR]: macros.html#c-macro-attr
[C-ANYWHERE]: macros.html#c-anywhere
[C-MACRO-VIS]: macros.html#c-macro-vis
[C-MACRO-TY]: macros.html#c-macro-ty

[C-CRATE-DOC]: documentation.html#c-crate-doc
[C-EXAMPLE]: documentation.html#c-example
[C-QUESTION-MARK]: documentation.html#c-question-mark
[C-FAILURE]: documentation.html#c-failure
[C-LINK]: documentation.html#c-link
[C-METADATA]: documentation.html#c-metadata
[C-HTML-ROOT]: documentation.html#c-html-root
[C-RELNOTES]: documentation.html#c-relnotes
[C-HIDDEN]: documentation.html#c-hidden

[C-SMART-PTR]: predictability.html#c-smart-ptr
[C-CONV-SPECIFIC]: predictability.html#c-conv-specific
[C-METHOD]: predictability.html#c-method
[C-NO-OUT]: predictability.html#c-no-out
[C-OVERLOAD]: predictability.html#c-overload
[C-DEREF]: predictability.html#c-deref
[C-CTOR]: predictability.html#c-ctor

[C-INTERMEDIATE]: flexibility.html#c-intermediate
[C-CALLER-CONTROL]: flexibility.html#c-caller-control
[C-GENERIC]: flexibility.html#c-generic
[C-OBJECT]: flexibility.html#c-object

[C-NEWTYPE]: type-safety.html#c-newtype
[C-CUSTOM-TYPE]: type-safety.html#c-custom-type
[C-BITFLAG]: type-safety.html#c-bitflag
[C-BUILDER]: type-safety.html#c-builder

[C-VALIDATE]: dependability.html#c-validate
[C-DTOR-FAIL]: dependability.html#c-dtor-fail
[C-DTOR-BLOCK]: dependability.html#c-dtor-block

[C-DEBUG]: debuggability.html#c-debug
[C-DEBUG-NONEMPTY]: debuggability.html#c-debug-nonempty

[C-SEALED]: future-proofing.html#c-sealed
[C-STRUCT-PRIVATE]: future-proofing.html#c-struct-private
[C-NEWTYPE-HIDE]: future-proofing.html#c-newtype-hide
[C-STRUCT-BOUNDS]: future-proofing.html#c-struct-bounds

[C-STABLE]: necessities.html#c-stable
[C-PERMISSIVE]: necessities.html#c-permissive
