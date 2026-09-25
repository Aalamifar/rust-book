## کتاب برنامه‌نویسی Rust

[![Deploy Persian Rust Book](https://github.com/Aalamifar/rust-book/actions/workflows/pages.yml/badge.svg)](https://github.com/Aalamifar/rust-book/actions/workflows/pages.yml)

این مخزن شامل منبع کتاب **«The Rust Programming Language»** است؛ به‌ویژه یک شاخهٔ آزمایشی که ویژگی‌های تعاملی مانند آزمون‌ها (quizzes) را پشتیبانی می‌کند.

**اگر مشکلی را در <https://aalamifar.github.io/rust-book/> کشف کردید، لطفاً آن را در این مخزن گزارش کنید، نه در جای دیگری.**

[این کتاب به‌صورت چاپی توسط No Starch Press به انگلیسی موجود است][nostarch].

[nostarch]: https://nostarch.com/rust-programming-language-2nd-edition

همچنین می‌توانید [کتاب تعاملی][book_fa] را به‌صورت رایگان آنلاین به فارسی بخوانید. همینطور می‌توانید کتاب را مطابق آخرین نسخه‌های Rustا [stable]، [beta] یا [nightly] استفاده کنید. توجه داشته باشید که ممکن است مشکلاتی که در این نسخه‌ها وجود دارد، در این مخزن اصلاح شده باشد، زیرا این انتشار‌ها به‌صورت کمتری به‌روز می‌شوند.

[book_fa]:https://aalamifar.github.io/rust-book/
[stable]: https://doc.rust-lang.org/stable/book/
[beta]: https://doc.rust-lang.org/beta/book/
[nightly]: https://doc.rust-lang.org/nightly/book/

برای دریافت کدهای تمام مثال‌های کتاب، به [releases] مراجعه کنید.

[releases]: https://github.com/rust-lang/book/releases

## پیش‌نیازها

برای ساخت کتاب به [mdBook](https://github.com/rust-lang/mdBook) نیاز دارید؛ ترجیحاً همان نسخه‌ای که در این کتاب استفاده شده در [این فایل](https://github.com/Aalamifar/rust-book/blob/main/book.toml) استفاده می‌کند استفاده کنید. برای دریافت آن:

```bash
$ cargo install mdbook --locked --version <شماره_نسخه>
```

این فورک همچنین به چند پیش‌پردازندهٔ mdBook برای پشتیبانی از افزونه‌های آزمایشی ما نیاز دارد. دستورالعمل نصب هر کدام در لینک‌های زیر آمده است:

* `mdbook-aquascope`: <https://github.com/cognitive-engineering-lab/aquascope#installation>
* `mdbook-quiz`: <https://github.com/cognitive-engineering-lab/mdbook-quiz#installation>

نسخهٔ دقیق هر پیش‌پردازنده را می‌توانید در [CD این مخزن](https://github.com/Aalamifar/rust-book/blob/main/.github/workflows/pages.yml) ببینید.

و در نهایت به [pnpm](https://pnpm.io/installation) نیاز دارید.

کتاب دو افزونهٔ mdbook دارد که در این مخزن قرار دارند. اگر آن‌ها را نصب نکنید، هشدارهایی هنگام ساخت می‌بینید و خروجی به‌درستی نمایش داده نمی‌شود، اما همچنان می‌توانید کتاب را بسازید. برای استفاده از این افزونه‌ها اجرا کنید:

```bash
$ cargo install --locked --path packages/mdbook-trpl-listing
$ cargo install --locked --path packages/mdbook-trpl-note
```

## ساخت کتاب

### با `cargo-make`

اگر [`cargo-make`] نصب شده باشد، کافی است اجرا کنید:

```bash
$ cargo make build
```

### بدون `cargo-make`

ابتدا افزونه‌های جاوااسکریپت را بسازید:

```bash
$ cd js-extensions
$ pnpm init-repo
$ cd ..
```

سپس کتاب را می‌توانید با دستور زیر بسازید:

```bash
$ mdbook build
```

### خروجی

خروجی در پوشهٔ `book` قرار می‌گیرد. برای مشاهده آن در مرورگر وب، فایل `index.html` را باز کنید.

_Firefox:_

```bash
$ firefox book/index.html                       # Linux
$ open -a "Firefox" book/index.html             # macOS
$ Start-Process "firefox.exe" .\book\index.html # Windows (PowerShell)
$ start firefox.exe .\book\index.html           # Windows (Cmd)
```

_Chrome:_

```bash
$ google-chrome book/index.html                 # Linux
$ open -a "Google Chrome" book/index.html       # macOS
$ Start-Process "chrome.exe" .\book\index.html  # Windows (PowerShell)
$ start chrome.exe .\book\index.html            # Windows (Cmd)
```

برای اجرای تست‌ها:

```bash
$ cd packages/trpl
$ mdbook test --library-path packages/trpl/target/debug/deps
```

### اجرای سریع تر کتاب

برای اجرای سریع تر کتاب می توانید بعد از ساخت کتاب با استفاده از دستور زیر در دایرکتوری کتاب آن را اجرا کنید:

```bash
$ mdbook serve --open
```

## مشارکت

ما مشتاق کمک‌های شما هستیم! لطفاً برای آشنایی با انواع مشارکت‌های موردنیاز، به [CONTRIBUTING.md][contrib] و [CONTRIBUTING_fa.md][contrib_fa] مراجعه کنید.

[contrib]: https://github.com/Aalamifar/rust-book/blob/main/CONTRIBUTING.md
[contrib_fa]: https://github.com/Aalamifar/rust-book/blob/main/CONTRIBUTING_fa.md

### درباره ترجمه‌

این مخزن ترجمهٔ فارسی کتاب Rust است و شامل ویژگی‌های تعاملی نسخهٔ آزمایشی (آزمون‌ها، Aquascope و …) می‌شود. متن اصلی به انگلیسی از [rust-lang/book](https://github.com/rust-lang/book) و  
ا[cognitive-engineering-lab/rust-book](https://github.com/cognitive-engineering-lab/rust-book) گرفته شده است و برای ترجمه و کار در هر قسمت از کتاب یک issues در repositorie باز میشود و در آخر به شاخه اصلی ادغام می‌شود

## بررسی املایی

برای اسکن فایل‌های منبع به‌منظور یافتن خطاهای املایی، می‌توانید اسکریپت `spellcheck.sh` موجود در پوشهٔ `ci` را استفاده کنید. این اسکریپت به یک واژه‌نامهٔ معتبر نیاز دارد که در `ci/dictionary.txt` فراهم شده است. اگر اسکریپت یک **false positive** تولید کرد (مثلاً واژه‌ای مثل `BTreeMap` که در واژه‌نامه وجود ندارد)، کافی است آن واژه را به `ci/dictionary.txt` اضافه کنید (به ترتیب حروف الفبا برای حفظ سازگاری).

[`cargo-make`]: https://github.com/sagiegurari/cargo-make