# 命名规范


<a id="c-case"></a>
## 大小写规范 RFC 430 

> Casing conforms to RFC 430 (C-CASE)

基础的命名规范在 [RFC 430] 中有描述。

通常，Rust 倾向于在“类型级别”（类型和 trait ）的结构中使用 `UpperCamelCase` ，
在“值级别”的结构中使用 `snake_case` 。确切说：

| Item | 规范 |
| ---- | ---------- |
| Crates | [有争议](https://github.com/rust-lang/api-guidelines/issues/29) [^crate-name] |
| Modules | `snake_case` |
| Types | `UpperCamelCase` |
| Traits | `UpperCamelCase` |
| Enum variants | `UpperCamelCase` |
| Functions | `snake_case` |
| Methods | `snake_case` |
| General constructors | `new` 或者 `with_more_details` |
| Conversion constructors | `from_some_other_type` |
| Macros | `snake_case!` |
| Local variables | `snake_case` |
| Statics | `SCREAMING_SNAKE_CASE` |
| Constants | `SCREAMING_SNAKE_CASE` |
| Type parameters | 简明的 `UpperCamelCase` ，通常使用单个大写字母： `T` |
| Lifetimes | 简短的 `lowercase`，通常使用单个小写字母 `'a`, `'de`, `'src` |
| Features | [有争议](https://github.com/rust-lang/api-guidelines/issues/101) ，但是一般遵照 [C-FEATURE] |

在 `UpperCamelCase` 情况下，复合词的首字母缩略词和缩略词算作一个单词：
使用 `Uuid` 而不使用 `UUID`，使用 `Usize` 而不使用 `USize`，
使用 `Stdin` 而不使用 `StdIn`。
在 `snake_case` 情况下，首字母缩略词和缩略词都是小写： `is_xid_start` 。

在 `snake_case` 或者 `SCREAMING_SNAKE_CASE` 情况下，
一个“单词”不应该由单个字母组成——除非这个字母时最后一个“词”：
使用 `btree_map` 而不使用 `b_tree_map`，使用 `PI_2` 而不使用 `PI2` 。

Crate 的名称不应该使用 `-rs` 或者 `-rust` 作为后缀或者前缀。
因为每个 crate 都是 Rust 编写的！
没必要一直提醒使用者这一点。

[^crate-name]:译者注：一般是 `snake_case` 或 `kebab-case` ，但只能存在一种形式，
且实际在 Rust 代码中使用时，后者需使用前者形式。
如 [`use actix_web`](https://github.com/actix/actix-web) 


[RFC 430]: https://github.com/rust-lang/rfcs/blob/master/text/0430-finalizing-naming-conventions.md
[C-FEATURE]: #c-feature

来自标准库的例子：

整个标准库都是这样做的。
这条原则不难。


<a id="c-conv"></a>
## 遵循 `as_`, `to_`, `into_` 规范 用以特定类型转换

> Ad-hoc conversions follow `as_`, `to_`, `into_` conventions (C-CONV)

应该使用带有以下前缀名称方法来进行特定类型转换：

| 名称前缀 | 内存代价 | 所有权 |
| ------ | ---- | --------- |
| `as_` | 无代价 | borrowed -\> borrowed |
| `to_` | 代价昂贵 | borrowed -\> borrowed<br>borrowed -\> owned (非 Copy 类型)<br>owned -\> owned (Copy 类型) |
| `into_` | 变量名 | owned -\> owned (非 Copy 类型) |

例如：

- `as_`
    - [`str::as_bytes()`] 
      用于查看 UTF-8 字节的 `str` 切片，
      这是无内存代价的（不会产生内存分配）。
      传入值是 `&str` 类型，输出值是 `&[u8]` 类型。
- `to_`
    - [`Path::to_str`] 
      对操作系统路径进行 UTF-8 字节检查，需要很大花销。
      虽然输入和输出都是借用，但是这个方法对运行时产生不容忽视的代价，
      所以不应使用 `as_str` 名称。
    - [`str::to_lowercase()`] 
      生成正确的 Unicode 小写字符，
      涉及遍历字符串的字符，可能需要分配内存。
      输入值是 `&str` 类型，输出值是 `String` 类型。
    - [`f64::to_radians()`] 
      把浮点数的角度制转换成弧度制。
      输入和输出都是 `f64` 。没必要传入 `&f64` ，因为复制 `f64` 花销很小。
      但是使用 `into_radians` 名称就会具有误导性，因为输入数据没有被消耗。
- `into_`
    - [`String::into_bytes()`]
      从 `String` 提取出背后的 `Vec<u8>` 数据，这是无代价的。
      它转移了 `String` 的所有权，然后返回具有所有权的 `Vec<u8>` 。
    - [`BufReader::into_inner()`] 
      转移了 buffered reader 的所有权，取出其背后的 reader ，这是无代价的。
      存于缓冲区的数据被丢弃了。
    - [`BufWriter::into_inner()`] 
      转移了 buffered writer 的所有权，取出其背后的 writer ，这可能以很大的代价刷新所有缓存数据。

[`str::as_bytes()`]: https://doc.rust-lang.org/std/primitive.str.html#method.as_bytes
[`Path::to_str`]: https://doc.rust-lang.org/std/path/struct.Path.html#method.to_str
[`str::to_lowercase()`]: https://doc.rust-lang.org/std/primitive.str.html#method.to_lowercase
[`f64::to_radians()`]: https://doc.rust-lang.org/std/primitive.f64.html#method.to_radians
[`String::into_bytes()`]: https://doc.rust-lang.org/std/string/struct.String.html#method.into_bytes
[`BufReader::into_inner()`]: https://doc.rust-lang.org/std/io/struct.BufReader.html#method.into_inner
[`BufWriter::into_inner()`]: https://doc.rust-lang.org/std/io/struct.BufWriter.html#method.into_inner

以 `as_` 和 `into_` 作为前缀的类型转换通常 *减少了抽象* ，
要么是查看背后的数据 ( `as` ) ，要么是分解 (deconstructe) 背后的数据 ( `into` ) 。
相对地，以 `to_` 作为前缀的类型转换处于同一个抽象层次，
但是做了更多的工作。

当一个类型用更高级别的语义 (higher-level semantics) 封装 (wraps) 一个与之有关的值时，
应该使用 `into_inner()` 方法名来取出被封装的值。
这适用于以下封装器：
读取缓存 ([`BufReader`]) 、编码或解码 ([`GzDecoder`]) 、取出原子 ([`AtomicBool`]) 、
或者任何相似的语义 ([`BufWriter`])。

[`BufReader`]: https://doc.rust-lang.org/std/io/struct.BufReader.html#method.into_inner
[`GzDecoder`]: https://docs.rs/flate2/1.0.20/flate2/read/struct.GzDecoder.html#method.into_inner
[`AtomicBool`]: https://doc.rust-lang.org/std/sync/atomic/struct.AtomicBool.html#method.into_inner
[`BufWriter`]: https://doc.rust-lang.org/std/io/struct.BufWriter.html#method.into_inner

如果类型转换方法返回的类型具有 `mut` 标识符，
那么这个方法的名称应如同返回类型组成部分的顺序那样，带有 `mut` 名。
比如 [`Vec::as_mut_slice`] 返回 `mut slice` 类型，这个方法的功能正如其名称所述，
所以这个名称优于 `as_slice_mut` 。

[`Vec::as_mut_slice`]: https://doc.rust-lang.org/std/vec/struct.Vec.html#method.as_mut_slice

```rust.ignored
// Return type is a mut slice.
fn as_mut_slice(&mut self) -> &mut [T];
```

更多来自标准库的例子：

- [`Result::as_ref`](https://doc.rust-lang.org/std/result/enum.Result.html#method.as_ref)
- [`RefCell::as_ptr`](https://doc.rust-lang.org/std/cell/struct.RefCell.html#method.as_ptr)
- [`slice::to_vec`](https://doc.rust-lang.org/std/primitive.slice.html#method.to_vec)
- [`Option::into_iter`](https://doc.rust-lang.org/std/option/enum.Option.html#method.into_iter)


<a id="c-getter"></a>
## getter 命名规范

> Getter names follow Rust convention (C-GETTER)

在 Rust 代码中，getter 方法（用来访问或者获取数据的方法）
并不使用 `get_` 前缀，但是也有一些例外。

```rust,ignored
pub struct S {
    first: First,
    second: Second,
}

impl S {
    // Not `get_first`.
    pub fn first(&self) -> &First {
        &self.first
    }

    // Not `get_first_mut`, `get_mut_first`, or `mut_first`.
    pub fn first_mut(&mut self) -> &mut First {
        &mut self.first
    }
}
```

仅在访问或获得 一个事物 这种显而易见的情况时，使用 `get` 来命名才比较合理。
比如 [`Cell::get`] 访问其 `Cell` 内容。

[`Cell::get`]: https://doc.rust-lang.org/std/cell/struct.Cell.html#method.get

对于在运行时进行验证（比如边界检查）的 getters ，
考虑给 unsafe 的方法名称增加 `_unchecked` 。
通常会有以下一些签名：

```rust,ignored
fn get(&self, index: K) -> Option<&V>;
fn get_mut(&mut self, index: K) -> Option<&mut V>;
unsafe fn get_unchecked(&self, index: K) -> &V;
unsafe fn get_unchecked_mut(&mut self, index: K) -> &mut V;
```

getter 和类型转换 (conversion | [C-CONV](#c-conv)) 之间的区别很小，
大部分时候不那么清晰可辨。
比如 [`TempDir::path`] 可以被理解为临时目录的文件系统路径的 getter ，
而 [`TempDir::into_path`] 负责把删除临时目录时转换的数据传给调用者。
因为 `path` 方法是一个 getter ，如果用 `get_path` 或者 `as_path` 就不对了。

[`TempDir::path`]: https://docs.rs/tempdir/0.3.7/tempdir/struct.TempDir.html#method.path
[`TempDir::into_path`]: https://docs.rs/tempdir/0.3.7/tempdir/struct.TempDir.html#method.into_path

来自标准库的例子：

- [`std::io::Cursor::get_mut`](https://doc.rust-lang.org/std/io/struct.Cursor.html#method.get_mut)
- [`std::ptr::Unique::get_mut`](https://doc.rust-lang.org/std/ptr/struct.Unique.html#method.get_mut)
- [`std::sync::PoisonError::get_mut`](https://doc.rust-lang.org/std/sync/struct.PoisonError.html#method.get_mut)
- [`std::sync::atomic::AtomicBool::get_mut`](https://doc.rust-lang.org/std/sync/atomic/struct.AtomicBool.html#method.get_mut)
- [`std::collections::hash_map::OccupiedEntry::get_mut`](https://doc.rust-lang.org/std/collections/hash_map/struct.OccupiedEntry.html#method.get_mut)
- [`<[T]>::get_unchecked`](https://doc.rust-lang.org/std/primitive.slice.html#method.get_unchecked)


<a id="c-iter"></a>
## 遵循 `iter`, `iter_mut`, `into_iter` 规范 用以生成迭代器 

> Methods on collections that produce iterators follow `iter`, `iter_mut`, `into_iter` (C-ITER)

参考 [RFC 199] 。


For a container with elements of type `U`, iterator methods should be named:

```rust,ignored
fn iter(&self) -> Iter             // Iter implements Iterator<Item = &U>
fn iter_mut(&mut self) -> IterMut  // IterMut implements Iterator<Item = &mut U>
fn into_iter(self) -> IntoIter     // IntoIter implements Iterator<Item = U>
```

This guideline applies to data structures that are conceptually homogeneous
collections. As a counterexample, the `str` type is slice of bytes that are
guaranteed to be valid UTF-8. This is conceptually more nuanced than a
homogeneous collection so rather than providing the
`iter`/`iter_mut`/`into_iter` group of iterator methods, it provides
[`str::bytes`] to iterate as bytes and [`str::chars`] to iterate as chars.

[`str::bytes`]: https://doc.rust-lang.org/std/primitive.str.html#method.bytes
[`str::chars`]: https://doc.rust-lang.org/std/primitive.str.html#method.chars

This guideline applies to methods only, not functions. For example
[`percent_encode`] from the `url` crate returns an iterator over percent-encoded
string fragments. There would be no clarity to be had by using an
`iter`/`iter_mut`/`into_iter` convention.

[`percent_encode`]: https://docs.rs/url/1.4.0/url/percent_encoding/fn.percent_encode.html
[RFC 199]: https://github.com/rust-lang/rfcs/blob/master/text/0199-ownership-variants.md

### Examples from the standard library

- [`Vec::iter`](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.iter)
- [`Vec::iter_mut`](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.iter_mut)
- [`Vec::into_iter`](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.into_iter)
- [`BTreeMap::iter`](https://doc.rust-lang.org/std/collections/struct.BTreeMap.html#method.iter)
- [`BTreeMap::iter_mut`](https://doc.rust-lang.org/std/collections/struct.BTreeMap.html#method.iter_mut)


<a id="c-iter-ty"></a>
## Iterator type names match the methods that produce them (C-ITER-TY)

A method called `into_iter()` should return a type called `IntoIter` and
similarly for all other methods that return iterators.

This guideline applies chiefly to methods, but often makes sense for functions
as well. For example the [`percent_encode`] function from the `url` crate
returns an iterator type called [`PercentEncode`][PercentEncode-type].

[PercentEncode-type]: https://docs.rs/url/1.4.0/url/percent_encoding/struct.PercentEncode.html

These type names make the most sense when prefixed with their owning module, for
example [`vec::IntoIter`].

[`vec::IntoIter`]: https://doc.rust-lang.org/std/vec/struct.IntoIter.html

### Examples from the standard library

* [`Vec::iter`] returns [`Iter`][slice::Iter]
* [`Vec::iter_mut`] returns [`IterMut`][slice::IterMut]
* [`Vec::into_iter`] returns [`IntoIter`][vec::IntoIter]
* [`BTreeMap::keys`] returns [`Keys`][btree_map::Keys]
* [`BTreeMap::values`] returns [`Values`][btree_map::Values]

[`Vec::iter`]: https://doc.rust-lang.org/std/vec/struct.Vec.html#method.iter
[slice::Iter]: https://doc.rust-lang.org/std/slice/struct.Iter.html
[`Vec::iter_mut`]: https://doc.rust-lang.org/std/vec/struct.Vec.html#method.iter_mut
[slice::IterMut]: https://doc.rust-lang.org/std/slice/struct.IterMut.html
[`Vec::into_iter`]: https://doc.rust-lang.org/std/vec/struct.Vec.html#method.into_iter
[vec::IntoIter]: https://doc.rust-lang.org/std/vec/struct.IntoIter.html
[`BTreeMap::keys`]: https://doc.rust-lang.org/std/collections/struct.BTreeMap.html#method.keys
[btree_map::Keys]: https://doc.rust-lang.org/std/collections/btree_map/struct.Keys.html
[`BTreeMap::values`]: https://doc.rust-lang.org/std/collections/struct.BTreeMap.html#method.values
[btree_map::Values]: https://doc.rust-lang.org/std/collections/btree_map/struct.Values.html


<a id="c-feature"></a>
## Feature names are free of placeholder words (C-FEATURE)

Do not include words in the name of a [Cargo feature] that convey zero meaning,
as in `use-abc` or `with-abc`. Name the feature `abc` directly.

[Cargo feature]: http://doc.crates.io/manifest.html#the-features-section

This arises most commonly for crates that have an optional dependency on the
Rust standard library. The canonical way to do this correctly is:

```toml
# In Cargo.toml

[features]
default = ["std"]
std = []
```

```rust
// In lib.rs

#![cfg_attr(not(feature = "std"), no_std)]
```

Do not call the feature `use-std` or `with-std` or any creative name that is not
`std`. This naming convention aligns with the naming of implicit features
inferred by Cargo for optional dependencies. Consider crate `x` with optional
dependencies on Serde and on the Rust standard library:

```toml
[package]
name = "x"
version = "0.1.0"

[features]
std = ["serde/std"]

[dependencies]
serde = { version = "1.0", optional = true }
```

When we depend on `x`, we can enable the optional Serde dependency with
`features = ["serde"]`. Similarly we can enable the optional standard library
dependency with `features = ["std"]`. The implicit feature inferred by Cargo for
the optional dependency is called `serde`, not `use-serde` or `with-serde`, so
we like for explicit features to behave the same way.

As a related note, Cargo requires that features are additive so a feature named
negatively like `no-abc` is practically never correct.


<a id="c-word-order"></a>
## Names use a consistent word order (C-WORD-ORDER)

Here are some error types from the standard library:

- [`JoinPathsError`](https://doc.rust-lang.org/std/env/struct.JoinPathsError.html)
- [`ParseBoolError`](https://doc.rust-lang.org/std/str/struct.ParseBoolError.html)
- [`ParseCharError`](https://doc.rust-lang.org/std/char/struct.ParseCharError.html)
- [`ParseFloatError`](https://doc.rust-lang.org/std/num/struct.ParseFloatError.html)
- [`ParseIntError`](https://doc.rust-lang.org/std/num/struct.ParseIntError.html)
- [`RecvTimeoutError`](https://doc.rust-lang.org/std/sync/mpsc/enum.RecvTimeoutError.html)
- [`StripPrefixError`](https://doc.rust-lang.org/std/path/struct.StripPrefixError.html)

All of these use verb-object-error word order. If we were adding an error to
represent an address failing to parse, for consistency we would want to name it
in verb-object-error order like `ParseAddrError` rather than `AddrParseError`.

The particular choice of word order is not important, but pay attention to
consistency within the crate and consistency with similar functionality in the
standard library.
