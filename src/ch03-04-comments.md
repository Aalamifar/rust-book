## کامنت‌ها

همه‌ی برنامه‌نویس‌ها سعی می‌کنند کدشان را قابل‌فهم بنویسند، اما گاهی توضیح اضافه لازم است. در این‌جور مواقع، برنامه‌نویس‌ها _کامنت_ (comment) در کد سورس خودشان می‌گذارند، که کامپایلر آن‌ها را نادیده می‌گیرد اما ممکن است برای کسانی که کد سورس را می‌خوانند مفید باشد.

این‌جا یک کامنت ساده را می‌بینید:

```rust
// hello, world
```

در Rust، سبک idiomatic برای کامنت این است که با دو اسلش شروع شود، و آن کامنت تا پایان همان خط ادامه دارد. برای کامنت‌هایی که از یک خط بیشتر ادامه دارند، باید `//` را در هر خط بگذارید، این‌طور:

```rust
// So we're doing something complicated here, long enough that we need
// multiple lines of comments to do it! Whew! Hopefully, this comment will
// explain what's going on.
```

یا می‌توانید از سینتکس کامنت چندخطی با `/*` و `*/` استفاده کنید:

```rust
/* So we're doing something complicated here, long enough that we need
   multiple lines of comments to do it! Whew! Hopefully, this comment will
   explain what's going on. */
```

کامنت‌ها می‌توانند در انتهای خط‌هایی که کد دارند هم قرار بگیرند:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-24-comments-end-of-line/src/main.rs}}
```

اما بیشتر آن‌ها را به این شکل می‌بینید که استفاده می‌شوند، با کامنت روی یک خط جدا، بالای کدی که توضیحش می‌دهد:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-25-comments-above-line/src/main.rs}}
```

Rust یک نوع دیگر از کامنت هم دارد، کامنت‌های مستندسازی (documentation comments)، که در بخش [«انتشار یک Crate در Crates.io»][publishing]<!-- ignore --> از فصل ۱۴ درباره‌شان صحبت می‌کنیم.

[publishing]: ch14-02-publishing-to-crates-io.html
