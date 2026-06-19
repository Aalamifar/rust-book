# زبان برنامه‌نویسی Rust — ترجمهٔ فارسی

[![Deploy Persian Rust Book](https://github.com/Aalamifar/rust-book/actions/workflows/pages.yml/badge.svg)](https://github.com/Aalamifar/rust-book/actions/workflows/pages.yml)

نسخهٔ فارسی [کتاب آزمایشی Rust Book](https://rust-book.cs.brown.edu/) از Cognitive Engineering Lab،
بر پایهٔ [The Rust Programming Language](https://github.com/rust-lang/book).

**خواندن آنلاین:** [https://aalamifar.github.io/rust-book/](https://aalamifar.github.io/rust-book/)

## دربارهٔ این پروژه

این مخزن ترجمهٔ فارسی کتاب Rust است و شامل ویژگی‌های تعاملی نسخهٔ آزمایشی (آزمون‌ها، Aquascope و …) می‌شود.
متن اصلی به انگلیسی از [rust-lang/book](https://github.com/rust-lang/book) و
[cognitive-engineering-lab/rust-book](https://github.com/cognitive-engineering-lab/rust-book) گرفته شده است.

وضعیت ترجمه در [TRANSLATION.md](TRANSLATION.md) ثبت می‌شود.

## پیش‌نیازها

- [Rust](https://www.rust-lang.org/tools/install) (نسخهٔ 1.90 یا جدیدتر)
- [mdBook](https://github.com/rust-lang/mdBook) نسخهٔ `0.5.2`
- [mdbook-quiz](https://github.com/cognitive-engineering-lab/mdbook-quiz) نسخهٔ `0.5.0`
- [mdbook-aquascope](https://github.com/cognitive-engineering-lab/aquascope) نسخهٔ `0.4.0`
- [pnpm](https://pnpm.io/installation)
- پیش‌پردازنده‌های محلی این مخزن: `packages/mdbook-trpl`

## ساخت محلی

### با cargo-make

```bash
cargo make build
```

### بدون cargo-make

```bash
cd js-extensions
pnpm init-repo
cd ..

cargo install --path packages/mdbook-trpl
mdbook build
```

خروجی در پوشهٔ `book/` ساخته می‌شود. برای پیش‌نمایش:

```bash
mdbook serve --open
```

## انتشار (GitHub Pages)

با push به شاخهٔ `main`، workflow در [`.github/workflows/pages.yml`](.github/workflows/pages.yml)
کتاب را می‌سازد و روی GitHub Pages منتشر می‌کند.

**تنظیمات لازم در GitHub (یک‌بار):**

1. **Settings → Pages → Build and deployment:** منبع را **GitHub Actions** انتخاب کنید.
2. پس از اولین deploy موفق، آدرس سایت در همان صفحه نمایش داده می‌شود.

## مشارکت

اگر در ترجمه یا محتوا اشتباهی دیدید، issue یا pull request بفرستید.
جزئیات بیشتر در [CONTRIBUTING.md](CONTRIBUTING.md).

## مجوز

متن اصلی کتاب تحت مجوزهای [Apache 2.0](LICENSE-APACHE) و [MIT](LICENSE-MIT) منتشر شده است.
ترجمهٔ فارسی نیز تحت همان مجوزها در دسترس است.

## منابع

- [کتاب رسمی Rust (انگلیسی)](https://doc.rust-lang.org/book/)
- [نسخهٔ آزمایشی (انگلیسی)](https://rust-book.cs.brown.edu/)
- [No Starch Press](https://nostarch.com/rust-programming-language-3rd-edition)
