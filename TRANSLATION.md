# برنامهٔ ترجمهٔ فارسی

این مخزن نسخهٔ فارسی کتاب تجربی Rust Book از Brown است. منبع اصلی:

- [cognitive-engineering-lab/rust-book](https://github.com/cognitive-engineering-lab/rust-book)
- [rust-lang/book](https://github.com/rust-lang/book)

## وضعیت

- [x] راه‌اندازی پروژهٔ `book-fa`
- [x] تنظیم عنوان، زبان و جهت راست‌به‌چپ
- [x] ترجمهٔ فهرست اصلی تا فصل ۶ و پیوست‌ها
- [x] ترجمهٔ صفحهٔ عنوان
- [x] ترجمهٔ «این کتاب چه تفاوتی دارد؟»
- [x] ترجمهٔ پیش‌گفتار
- [x] ترجمهٔ مقدمه
- [x] ترجمهٔ README، style-guide و TODO
- [x] آماده‌سازی GitHub Actions برای انتشار روی GitHub Pages
- [ ] ترجمهٔ فصل‌های ۱ تا ۲۱
- [ ] ترجمهٔ پیوست‌ها

## واژه‌نامهٔ پیشنهادی

| English          | فارسی       |
| ---------------- | ----------- |
| ownership        | مالکیت      |
| borrowing        | امانت‌گیری  |
| reference        | ارجاع       |
| mutable          | تغییرپذیر   |
| immutable        | تغییرناپذیر |
| lifetime         | طول‌عمر     |
| trait            | Trait       |
| crate            | Crate       |
| package          | Package     |
| struct             | struct        |
| enum             | enum        |
| pattern matching | تطبیق الگو  |

## قاعده‌های ترجمه

- نام فایل‌ها، لینک‌ها، کدها، خروجی‌های ترمینال و shortcodeهای mdBook را تغییر ندهید.
- اصطلاح‌های Rust مانند `Cargo.toml`، `match`، `trait` و `crate` در متن فارسی با همان شکل فنی حفظ شوند، مگر در جایی که ترجمه باعث وضوح بیشتر شود.
- هر فصل را جداگانه ترجمه و سپس با `mdbook build` بررسی کنید.
