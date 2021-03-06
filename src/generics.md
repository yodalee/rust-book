# 泛型

有時我們希望函式或資料型別能接受不同型別的參數，在 Rust 中提供了泛型來滿足這項需求。
在型別論中，泛型又被稱為「參數式多型」，係指型別或函式面對給定的參數（parametric）時具有多種型態（「poly」為多，「morph」指型態）。

總之不管理論，先來看一些泛型程式，Rust 標準函式庫提供了型別 `Option<T>` 即是泛型：

```rust
enum Option<T> {
    Some(T),
    None,
}
```

其中我們曾見過數次的 `<T>` 即表示泛型資料型別，在 `enum` 的宣告中只要遇到 `T` 即會被替換成泛型中使用的型別。
以下是使用 `Option<T>` 並附加額外型別註釋的範例：

```rust
let x: Option<i32> = Some(5);
```

在型別宣告中，我們使用與 `Option<T>` 相似的 `Option<i32>`。
因此在這個 `Option` 中，`T` 即是 `i32`。
等式右邊我們加上 `T` 為 `5` 的 `Some(T)`。
因為 `5` 屬於型別 `i32`，所以兩邊相符，可以通過編譯。
若兩邊不符則會發生錯誤：

```rust,ignore
let x: Option<f64> = Some(5);
// error: mismatched types: expected `core::option::Option<f64>`,
// found `core::option::Option<_>` (expected f64 but found integral variable)
```

這並不表示我們不能讓 `Option<T>` 對應為 `f64`，只是等式兩邊必須相符：

```rust
let x: Option<i32> = Some(5);
let y: Option<f64> = Some(5.0f64);
```

這樣就行了。
一個定義，多種使用方式。

泛型並非只能泛型一個型別。
讓我們看看 Rust 標準函式庫的另一個型別 `Result<T, E>`：

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

這是個針對 `T` 和 `E` _兩個_ 型別泛型的型別。
型別的大寫字可以是任何字。
想要的話，我們也可以把 `Result<T, E>` 定義成：

```rust
enum Result<A, Z> {
    Ok(A),
    Err(Z),
}
```

慣例上第一個泛型參數會是 `T` 表示「type」，然後我們用 `E` 表示「error」。
然而 Rust 實際上並不管這些東西。

型別 `Result<T, E>` 用在回傳運算結果，也能夠在出錯時回傳錯誤。

## 泛型函式

我們可以用類似的語法宣告接受泛型的函式：

```rust
fn takes_anything<T>(x: T) {
    // do something with x
}
```

語法包含兩個部分：`<T>` 表示 "這個函式對型別 `T` 泛型"，而 `x: T` 表示 "x 的型別為 `T`"

一個泛型可以用在多個參數上：

```rust
fn takes_two_of_the_same_things<T>(x: T, y: T) {
    // ...
}
```

我們也可以宣告接受多個泛型的版本：

```rust
fn takes_two_things<T, U>(x: T, y: U) {
    // ...
}
```

## 泛型結構體

泛型也可以使用在 `struct` 中：

```rust
struct Point<T> {
    x: T,
    y: T,
}

let int_origin = Point { x: 0, y: 0 };
let float_origin = Point { x: 0.0, y: 0.0 };
```

與函式很類似，`<T>` 宣告泛型參數，使用 `x: T` 宣告型別。

當要加上泛型結構體的實作時，在 `impl` 之後宣告型別參數：

```rust
# struct Point<T> {
#     x: T,
#     y: T,
# }
#
impl<T> Point<T> {
    fn swap(&mut self) {
        std::mem::swap(&mut self.x, &mut self.y);
    }
}
```

目前為止，我們已經展示泛型可以用於任意型別。
這在許多狀況下非常有用，例如先前看到的 `Option<T>`，以及接下來會看到通用容器型別，如 [Vec&lt;T&gt;][Vec]。
另一方面，常常我們想要用這樣的彈性換取表現能力，
參考 [trait bounds][traits] 了解如何做到。

[traits]: traits.html
[Vec]: http://doc.rust-lang.org/std/vec/struct.Vec.html


> *commit 6ba9520*
