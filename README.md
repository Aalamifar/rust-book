# The Rust Programming Language — Persian Edition

[![Deploy Persian Rust Book](https://github.com/Aalamifar/rust-book/actions/workflows/pages.yml/badge.svg)](https://github.com/Aalamifar/rust-book/actions/workflows/pages.yml)

A Persian translation and interactive edition of **[The Rust Programming Language](https://doc.rust-lang.org/book/)**. This repository builds on the official Rust book and the interactive book project, bringing Persian readers features such as quizzes and Aquascope visualizations.

**Read the book online:** https://aalamifar.github.io/rust-book/

## Highlights

- Persian translation of *The Rust Programming Language*.
- Interactive quizzes powered by [mdbook-quiz](https://github.com/cognitive-engineering-lab/mdbook-quiz).
- Code visualizations powered by [Aquascope](https://github.com/cognitive-engineering-lab/aquascope).
- Custom mdBook preprocessors maintained in this repository.
- Automated deployment to GitHub Pages.

## Building locally

The deployment workflow in [`.github/workflows/pages.yml`](.github/workflows/pages.yml) is the source of truth for the versions used in production. It currently uses Rust 1.90, mdBook 0.5.2, mdbook-quiz 0.5.0, Aquascope 0.4.0, Node.js 20, and pnpm 9, plus a dedicated nightly Rust toolchain for Aquascope.

You will also need the local mdBook preprocessors and JavaScript extensions.

1. Install the tools and versions listed in the deployment workflow.
2. Install the repository's local preprocessors (the package provides the listing, note, heading, and figure binaries):

   ```bash
   cargo install --path packages/mdbook-trpl
   ```

3. Build the JavaScript extensions:

   ```bash
   cd js-extensions
   pnpm init-repo
   cd ..
   ```

4. Build and serve the book:

   ```bash
   mdbook build
   mdbook serve --open
   ```

The generated site is written to the `book/` directory.

## Contributing

Contributions are welcome. The current contribution guide is written in Persian: [CONTRIBUTING.md](CONTRIBUTING.md). Before opening an issue or pull request, please check the existing issues and pull requests.

If you find a problem in the published book, please report it in this repository.

## Sources and attribution

This edition is based on:

- [rust-lang/book](https://github.com/rust-lang/book) — the official Rust book.
- [cognitive-engineering-lab/rust-book](https://github.com/cognitive-engineering-lab/rust-book) — the interactive edition and related tooling.

The printed English edition is published by [No Starch Press](https://nostarch.com/rust-programming-language-2nd-edition).

## License

The repository includes the MIT and Apache-2.0 license texts in [LICENSE-MIT](LICENSE-MIT) and [LICENSE-APACHE](LICENSE-APACHE). Please review the upstream projects' notices and licensing terms when reusing translated or third-party content.

---

## نسخهٔ فارسی

این مخزن شامل ترجمهٔ فارسی و نسخهٔ تعاملی کتاب **The Rust Programming Language** است. در کنار متن فارسی، قابلیت‌هایی مانند آزمون‌های تعاملی (Quiz) و نمایش بصری کد با Aquascope نیز در آن فراهم شده است.

**[مطالعهٔ رایگان کتاب به زبان فارسی](https://aalamifar.github.io/rust-book/)**

### پیش‌نیازها

برای ساخت کتاب به mdBook و پیش‌پردازنده‌های آن نیاز دارید. نسخه‌های دقیق ابزارهای مورد استفاده در فرایند انتشار در [فایل workflow](.github/workflows/pages.yml) مشخص شده‌اند.

این مخزن همچنین از ابزارهای زیر استفاده می‌کند:

- `mdbook-aquascope`
- `mdbook-quiz`
- افزونه‌های محلی mdBook در پوشهٔ `packages`
- `pnpm` برای ساخت افزونه‌های JavaScript

### ساخت کتاب

ابتدا افزونه‌های JavaScript را بسازید:

```bash
cd js-extensions
pnpm init-repo
cd ..
```

سپس کتاب را بسازید:

```bash
mdbook build
```

برای اجرای محلی و مشاهدهٔ تغییرات:

```bash
mdbook serve --open
```

خروجی در پوشهٔ `book` قرار می‌گیرد.

### مشارکت و گزارش مشکل

برای آشنایی با روند مشارکت، راهنمای [CONTRIBUTING.md](CONTRIBUTING.md) را بخوانید. اگر در نسخهٔ آنلاین کتاب مشکلی پیدا کردید، لطفاً آن را در بخش Issues همین مخزن گزارش کنید.

### دربارهٔ ترجمه

متن اصلی این پروژه از [مخزن رسمی کتاب Rust](https://github.com/rust-lang/book) و [نسخهٔ تعاملی کتاب](https://github.com/cognitive-engineering-lab/rust-book) گرفته شده است. این مخزن تلاش می‌کند دسترسی فارسی‌زبانان به یادگیری Rust را با ترجمه و امکانات تعاملی آسان‌تر کند.

### بررسی املایی

برای بررسی املای فایل‌های منبع می‌توانید از اسکریپت `ci/spellcheck.sh` و واژه‌نامهٔ `ci/dictionary.txt` استفاده کنید. اگر اسکریپت واژه‌ای معتبر مانند `BTreeMap` را به‌اشتباه گزارش کرد، آن را به واژه‌نامه اضافه کنید و ترتیب الفبایی را حفظ کنید.
