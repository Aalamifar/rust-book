<!-- Old headings. Do not remove or links may break. -->

<a id="installing-binaries-from-cratesio-with-cargo-install"></a>

## نصب برنامه‌های اجرایی با `cargo install`

دستور `cargo install` به شما اجازه می‌دهد Packageهایی را که ابزار اجرایی
ارائه می‌کنند، به‌صورت محلی نصب و استفاده کنید. هدف این دستور جایگزین کردن مدیر بستهٔ سیستم‌عامل
نیست؛ بلکه راهی ساده و راحت برای نصب ابزارهایی است که توسعه‌دهندگان Rust
در [crates.io](https://crates.io/)<!-- ignore --> با دیگران به اشتراک
گذاشته‌اند.

توجه کنید که `cargo install` فقط Packageهایی را نصب می‌کند که دست‌کم یک Binary Target داشته باشند. Binary Target همان برنامهٔ اجرایی‌ای است که از فایل _src/main.rs_ یا فایلی که به‌عنوان یک باینری تعریف شده باشد، ساخته می‌شود. در مقابل، Library Target به‌تنهایی قابل اجرا نیست، اما می‌توان آن را در برنامه‌های دیگر به کار برد. معمولاً در فایل README هر Package توضیح داده می‌شود که شامل یک Library Target، یک Binary Target یا هر دو است.

تمام باینری‌هایی که با `cargo install` نصب می‌شوند، در پوشهٔ `bin`
از مسیر ریشهٔ نصب Rust قرار می‌گیرند. اگر Rust را با استفاده از
_rustup.rs_ نصب کرده‌اید و پیکربندی سفارشی‌ای ندارید، این مسیر معمولاً
`$HOME/.cargo/bin` است. برای اینکه بتوانید برنامه‌های نصب‌شده با
`cargo install` را اجرا کنید، مطمئن شوید این پوشه در متغیر محیطی
`$PATH` قرار دارد.

برای مثال، در فصل ۱۲ اشاره کردیم که پیاده‌سازی‌ای از ابزار `grep`
با زبان Rust وجود دارد که `ripgrep` نام دارد و برای جست‌وجو در فایل‌ها
استفاده می‌شود. برای نصب `ripgrep` می‌توانیم دستور زیر را اجرا کنیم:

<!-- manual-regeneration
cargo install something you don't have, copy relevant output below
-->

```console
$ cargo install ripgrep
    Updating crates.io index
  Downloaded ripgrep v14.1.1
  Downloaded 1 crate (213.6 KB) in 0.40s
  Installing ripgrep v14.1.1
--snip--
   Compiling grep v0.3.2
    Finished `release` profile [optimized] target(s) in 6.73s
  Installing ~/.cargo/bin/rg
   Installed package `ripgrep v14.1.1` (executable `rg`)
```

خط یکی‌مانده‌به‌آخر خروجی، مسیر و نام باینری نصب‌شده را نشان می‌دهد.
در این مثال نام فایل اجرایی `rg` است، نه `ripgrep`. اگر مسیر نصب
در متغیر `$PATH` قرار داشته باشد، همان‌طور که پیش‌تر گفتیم، می‌توانید
دستور `rg --help` را اجرا کنید و از یک ابزار جست‌وجوی سریع‌تر که با
Rust نوشته شده است استفاده کنید!

