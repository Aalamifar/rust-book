## انتشار یک Crate در Crates.io

تا اینجا از بسته‌های موجود در [crates.io](https://crates.io/)<!-- ignore -->
به‌عنوان وابستگی پروژه‌هایمان استفاده کرده‌ایم؛ اما شما هم می‌توانید
با انتشار Crateهای خود، کدتان را با دیگران به اشتراک بگذارید.
Registry مربوط به Crateها در [crates.io](https://crates.io/)<!-- ignore -->
کد منبع بسته‌ها را توزیع می‌کند و به همین دلیل بیشتر میزبان کدهای
Open Source است.

Rust و Cargo قابلیت‌هایی دارند که پیدا کردن و استفاده از بستهٔ منتشرشدهٔ
شما را برای دیگران آسان‌تر می‌کنند. ابتدا با تعدادی از این قابلیت‌ها
آشنا می‌شویم و سپس فرایند انتشار یک Crate را توضیح می‌دهیم.

### نوشتن Doc Commentها مفید

مستندسازی دقیق بسته‌ها به کاربران کمک می‌کند بدانند چه زمانی و چگونه
باید از آن‌ها استفاده کنند؛ بنابراین ارزش دارد برای نوشتن مستندات وقت
بگذارید. در فصل ۳ دربارهٔ نوشتن Comment در کد Rust با استفاده
از دو اسلش، یعنی `//`، صحبت کردیم. Rust نوع خاصی از توضیح را نیز برای
مستندسازی در اختیارمان قرار می‌دهد که به آن _Doc Comment_
(Documentation Comment) می‌گوییم. این توضیحات امکان تولید مستندات
HTML را فراهم می‌کنند.

مستندات HTML محتوای Doc Commentها مربوط به اعضای API عمومی را نمایش
می‌دهند. این مستندات برای برنامه‌نویسانی نوشته شده‌اند که می‌خواهند
بدانند _چگونه از_ Crate شما استفاده کنند، نه اینکه پیاده‌سازی داخلی
آن _چگونه انجام شده است_.

Doc Commentها به‌جای دو اسلش، از سه اسلش یعنی `///` استفاده
می‌کنند و از نشانه‌گذاری Markdown برای قالب‌بندی متن پشتیبانی می‌کنند.
این توضیحات را درست پیش از عضوی قرار دهید که قرار است مستندش کنند.
در Listing 14-1 Doc Commentها تابع `add_one` را در Crateای به نام
`my_crate` مشاهده می‌کنید.

<Listing number="14-1" file-name="src/lib.rs" caption="Doc Comment برای یک تابع">

```rust,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-01/src/lib.rs}}
```

</Listing>

در این مثال، ابتدا توضیحی دربارهٔ کاری که تابع `add_one` انجام می‌دهد
می‌نویسیم، سپس بخشی با عنوان `Examples` ایجاد می‌کنیم و در ادامه کدی
قرار می‌دهیم که نحوهٔ استفاده از تابع `add_one` را نشان می‌دهد.
برای تولید مستندات HTML از این Doc Comment، دستور `cargo doc`
را اجرا می‌کنیم. این دستور ابزار `rustdoc` را که همراه Rust توزیع
می‌شود اجرا می‌کند و مستندات HTML تولیدشده را در پوشهٔ _target/doc_
قرار می‌دهد.

برای راحتی بیشتر، اجرای دستور `cargo doc --open` مستندات HTML مربوط
به Crate فعلی شما و همچنین مستندات تمام وابستگی‌های آن را می‌سازد و
نتیجه را در مرورگر وب باز می‌کند. به تابع `add_one` بروید تا ببینید
متن Doc Commentها چگونه نمایش داده می‌شود؛ درست مانند شکل ۱۴-۱.

<img alt="Rendered HTML documentation for the `add_one` function of `my_crate`" src="img/trpl14-01.png" class="center" />

<span class="caption">شکل ۱۴-۱: مستندات HTML مربوط به تابع `add_one`</span>

#### بخش‌های رایج در مستندات

در Listing 14-1 از عنوان Markdown یعنی `# Examples` استفاده کردیم
تا بخشی با عنوان «Examples» در مستندات HTML ایجاد کنیم. نویسندگان
Crateها معمولاً از بخش‌های دیگری نیز در مستندات خود استفاده می‌کنند:

- **Panics (Panic):** این بخش موقعیت‌هایی را توضیح
  می‌دهد که ممکن است تابع مستندشده در آن‌ها دچار Panic شود. کسانی که
  از تابع استفاده می‌کنند و نمی‌خواهند برنامه‌شان متوقف شود، باید
  مطمئن شوند تابع را در چنین شرایطی فراخوانی نمی‌کنند.
- **Errors (خطاها):** اگر تابع مقداری از نوع `Result` برمی‌گرداند،
  توضیح انواع خطاهای احتمالی و شرایطی که باعث بازگردانده شدن آن‌ها
  می‌شوند، به فراخواننده کمک می‌کند برای هر نوع خطا رفتار مناسبی
  در کد خود در نظر بگیرد.
- **Safety (ایمنی):** اگر فراخوانی تابع نیازمند `unsafe` باشد
  (در فصل ۲۰ دربارهٔ ناایمنی صحبت می‌کنیم)، باید بخشی توضیح دهد چرا
  این تابع ناایمن است و فراخواننده موظف است چه Invariantهایی را رعایت کند.

بیشتر Doc Commentها به همهٔ این بخش‌ها نیاز ندارند؛ اما این فهرست
یادآوری خوبی است تا جنبه‌هایی از کد را که کاربران علاقه‌مندند دربارهٔ
آن‌ها بدانند، از قلم نیندازید.

#### Doc Commentها به‌عنوان آزمون

اضافه کردن بلوک‌های کد نمونه به Doc Commentها، علاوه بر اینکه نحوهٔ
استفاده از کتابخانه را نشان می‌دهد، مزیت دیگری هم دارد: اجرای دستور
`cargo test`، مثال‌های کد موجود در مستندات شما را نیز به‌عنوان آزمون
اجرا می‌کند!

هیچ‌چیز بهتر از مستنداتی نیست که مثال‌های کاربردی دارند؛ اما در مقابل،
هیچ‌چیز بدتر از مثال‌هایی نیست که به دلیل تغییر کد، دیگر کار نمی‌کنند.
اگر `cargo test` را با مستندات تابع `add_one` در Listing 14-1 اجرا
کنیم، بخشی شبیه خروجی زیر را در نتایج آزمون‌ها می‌بینیم:

<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-01/
cargo test
copy just the doc-tests section below
-->

```text
   Doc-tests my_crate

running 1 test
test src/lib.rs - add_one (line 5) ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.27s
```

حالا اگر تابع یا مثال را طوری تغییر دهیم که دستور `assert_eq!` در
مثال باعث Panic شود و دوباره `cargo test` را اجرا کنیم، آزمون‌های
مستندات متوجه می‌شوند که مثال و کد واقعی دیگر با یکدیگر هماهنگ نیستند!

<!-- Old headings. Do not remove or links may break. -->

<a id="commenting-contained-items"></a>

#### Doc Comment برای عضو دربرگیرنده

سبک دیگری از Doc Commentها با نماد `//!` وجود دارد. این نوع توضیح،
به‌جای اعضایی که _بعد از_ آن قرار می‌گیرند، به عضوی مستندات اضافه
می‌کند که خودِ توضیح را _در بر گرفته است_.

معمولاً از این نوع توضیحات در فایل ریشهٔ Crate (که طبق قرارداد
_src/lib.rs_ است) یا درون یک ماژول استفاده می‌کنیم تا کل Crate یا
ماژول را به‌صورت یکجا مستند کنیم.

برای مثال، اگر بخواهیم هدف Crateای به نام `my_crate` را که تابع
`add_one` را در خود دارد توضیح دهیم، Doc Commentها را با نماد
`//!` در ابتدای فایل _src/lib.rs_ قرار می‌دهیم؛ همان‌طور که در
Listing 14-2 نشان داده شده است.

<Listing number="14-2" file-name="src/lib.rs" caption="مستندسازی کل Crate با نام `my_crate`">

```rust,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-02/src/lib.rs:here}}
```

</Listing>

دقت کنید پس از آخرین خطی که با `//!` شروع می‌شود، هیچ کدی قرار
نداده‌ایم. چون به‌جای `///` از `//!` استفاده کرده‌ایم، عضوی را
مستند می‌کنیم که این توضیح را در خود جای داده است، نه عضوی را که
پس از توضیح می‌آید. در این مثال، آن عضو فایل _src/lib.rs_ است که
ریشهٔ Crate محسوب می‌شود. بنابراین این توضیحات کل Crate را توصیف
می‌کنند.

وقتی دستور `cargo doc --open` را اجرا کنیم، این توضیحات در صفحهٔ
اصلی مستندات `my_crate` و بالای فهرست اعضای عمومی Crate نمایش داده
می‌شوند؛ همان‌طور که در شکل ۱۴-۲ می‌بینید.

Doc Commentها درون اعضا، به‌ویژه برای توضیح Crateها و ماژول‌ها
مفیدند. از آن‌ها برای شرح هدف کلی بخشی که توضیح را در بر گرفته است
استفاده کنید تا کاربران بهتر متوجه سازمان‌دهی Crate شوند.

<img alt="Rendered HTML documentation with a comment for the crate as a whole" src="img/trpl14-02.png" class="center" />

<span class="caption">شکل ۱۴-۲: مستندات تولیدشده برای کل Crate `my_crate`، همراه با توضیحی دربارهٔ هدف آن</span>

<!-- Old headings. Do not remove or links may break. -->

<a id="exporting-a-convenient-public-api-with-pub-use"></a>

### ارائهٔ یک API عمومیِ خوش‌دست

ساختار API عمومی هنگام انتشار یک Crate اهمیت زیادی دارد. افرادی که از
Crate شما استفاده می‌کنند، به‌اندازهٔ شما با ساختار داخلی آن آشنا
نیستند. اگر Crate سلسله‌مراتب ماژول‌های بزرگی داشته باشد، ممکن است
پیدا کردن بخش‌هایی که می‌خواهند از آن‌ها استفاده کنند برایشان دشوار
شود.

در فصل ۷ یاد گرفتیم با کلیدواژهٔ `pub` اعضا را عمومی کنیم و با
کلیدواژهٔ `use` آن‌ها را به Scope بیاوریم. با این
حال، ساختاری که هنگام توسعهٔ Crate برای شما منطقی به نظر می‌رسد،
ممکن است برای کاربران آن چندان راحت نباشد.

شاید بخواهید Structهای خود را در سلسله‌مراتبی چندسطحی سازمان‌دهی کنید؛
اما در این صورت، افرادی که می‌خواهند از نوعی استفاده کنند که در عمق
این سلسله‌مراتب تعریف شده است، ممکن است حتی متوجه وجود آن نوع نشوند.
همچنین شاید مجبور بودن به نوشتن مسیری مانند
`use my_crate::some_module::another_module::UsefulType;`
به‌جای `use my_crate::UsefulType;` برایشان آزاردهنده باشد.

خبر خوب این است که اگر ساختار داخلی کتابخانه برای استفادهٔ دیگران
مناسب نیست، لازم نیست سازمان‌دهی داخلی خود را تغییر دهید. به‌جای آن
می‌توانید با استفاده از `pub use` اعضا را دوباره صادر کنید
(Re-export) تا ساختار عمومی متفاوتی از ساختار خصوصی داخلی داشته باشید.

_صادر کردن دوباره_ یعنی یک عضو عمومی را در یک محل دیگر نیز عمومی
کنیم؛ به‌گونه‌ای که انگار در همان محل دوم تعریف شده است.

برای مثال، فرض کنید کتابخانه‌ای به نام `art` برای مدل‌سازی مفاهیم
هنری ساخته‌ایم. این کتابخانه دو ماژول دارد: ماژول `kinds` که شامل
دو Enum با نام‌های `PrimaryColor` و `SecondaryColor` است و ماژول
`utils` که تابعی به نام `mix` را در خود دارد. این ساختار در
Listing 14-3 نشان داده شده است.

<Listing number="14-3" file-name="src/lib.rs" caption="کتابخانهٔ art با اعضایی که در ماژول‌های kinds و utils سازمان‌دهی شده‌اند">

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-03/src/lib.rs:here}}
```

</Listing>

شکل ۱۴-۳ نشان می‌دهد صفحهٔ اصلی مستندات این Crate که با دستور
`cargo doc` تولید شده است، چه شکلی خواهد داشت.

<img alt="Rendered documentation for the `art` crate that lists the `kinds` and `utils` modules" src="img/trpl14-03.png" class="center" />

<span class="caption">شکل ۱۴-۳: صفحهٔ اصلی مستندات Crate مربوط به `art` که ماژول‌های `kinds` و `utils` را فهرست می‌کند</span>

توجه کنید که نوع‌های `PrimaryColor` و `SecondaryColor` و همچنین
تابع `mix` در صفحهٔ اصلی فهرست نشده‌اند. برای دیدن آن‌ها باید روی
ماژول‌های `kinds` و `utils` کلیک کنیم.

Crate دیگری که به این کتابخانه وابسته باشد، برای وارد کردن اعضای
`art` به محدودهٔ دسترسی خود باید از دستورهای `use` استفاده کند
و ساختار فعلی ماژول‌ها را نیز در مسیرها مشخص کند. در Listing 14-4
نمونه‌ای از Crateای را می‌بینید که از اعضای `PrimaryColor` و
`mix` در Crate مربوط به `art` استفاده می‌کند.

<Listing number="14-4" file-name="src/main.rs" caption="Crateای که با استفاده از ساختار داخلی صادرشده، از اعضای کتابخانهٔ art استفاده می‌کند">

```rust,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-04/src/main.rs}}
```

</Listing>

نویسندهٔ کد Listing 14-4 که از Crate مربوط به `art` استفاده می‌کند،
باید می‌دانست `PrimaryColor` در ماژول `kinds` و تابع `mix` در
ماژول `utils` قرار دارد. ساختار ماژول‌های Crate مربوط به `art`
برای توسعه‌دهندگانی که روی خود این Crate کار می‌کنند اهمیت بیشتری
دارد تا کسانی که فقط از آن استفاده می‌کنند.

ساختار داخلی اطلاعات مفیدی دربارهٔ نحوهٔ استفاده از Crate در اختیار
کاربر قرار نمی‌دهد؛ بلکه ممکن است باعث سردرگمی شود، چون توسعه‌دهندگان
باید محل تعریف اعضا را پیدا کنند و نام ماژول‌ها را در دستورهای
`use` بنویسند.

برای اینکه ساختار داخلی را از API عمومی جدا کنیم، می‌توانیم کد
Crate مربوط به `art` در Listing 14-3 را تغییر دهیم و با اضافه کردن
دستورهای `pub use`، اعضا را در سطح بالاتری دوباره صادر کنیم؛
همان‌طور که در Listing 14-5 می‌بینید.

<Listing number="14-5" file-name="src/lib.rs" caption="اضافه کردن دستورهای pub use برای صادر کردن دوبارهٔ اعضا">

```rust,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-05/src/lib.rs:here}}
```

</Listing>

اکنون مستندات API که Cargo برای این Crate تولید می‌کند، اعضای
صادرشدهٔ دوباره را در صفحهٔ اصلی فهرست می‌کند و به آن‌ها پیوند
می‌دهد؛ همان‌طور که در شکل ۱۴-۴ می‌بینید. در نتیجه پیدا کردن
نوع‌های `PrimaryColor` و `SecondaryColor` و تابع `mix`
آسان‌تر می‌شود.

<img alt="Rendered documentation for the `art` crate with the re-exports on the front page" src="img/trpl14-04.png" class="center" />

<span class="caption">شکل ۱۴-۴: صفحهٔ اصلی مستندات Crate مربوط به `art` که اعضای صادرشدهٔ دوباره را نمایش می‌دهد</span>

کاربران Crate مربوط به `art` همچنان می‌توانند ساختار داخلی Listing
14-3 را ببینند و مانند Listing 14-4 از آن استفاده کنند؛ یا می‌توانند
ساختار ساده‌تر Listing 14-5 را به کار ببرند، همان‌طور که در Listing
14-6 نشان داده شده است.

<Listing number="14-6" file-name="src/main.rs" caption="برنامه‌ای که از اعضای صادرشدهٔ دوبارهٔ Crate مربوط به art استفاده می‌کند">

```rust,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-06/src/main.rs:here}}
```

</Listing>

وقتی ماژول‌های تو‌در‌توی زیادی دارید، صادر کردن دوبارهٔ نوع‌ها با
`pub use` در سطح بالایی می‌تواند تجربهٔ استفاده از Crate را برای
کاربران به‌طور چشمگیری بهتر کند. کاربرد رایج دیگر `pub use` این
است که تعریف‌های یک وابستگی را از Crate فعلی دوباره صادر کنید تا
آن تعریف‌ها بخشی از API عمومی Crate شما شوند.

ساختن یک API عمومیِ خوش‌ساخت بیشتر هنر است تا علم؛ می‌توانید بارها
آن را اصلاح کنید تا به ساختاری برسید که برای کاربران مناسب‌تر باشد.
استفاده از `pub use` به شما انعطاف می‌دهد تا ساختار داخلی Crate
را مستقل از ساختاری که به کاربران نمایش می‌دهید، طراحی کنید.
کد بعضی از Crateهایی را که نصب کرده‌اید بررسی کنید و ببینید آیا
ساختار داخلی آن‌ها با API عمومی‌شان تفاوت دارد یا نه.


### ساخت حساب کاربری در Crates.io

پیش از انتشار هر Crate، باید در
[crates.io](https://crates.io/)<!-- ignore --> حساب کاربری بسازید
و یک توکن API دریافت کنید. برای این کار به صفحهٔ اصلی
[crates.io](https://crates.io/)<!-- ignore --> بروید و با حساب
GitHub خود وارد شوید. (در حال حاضر داشتن حساب GitHub الزامی است،
اما ممکن است این وب‌سایت در آینده روش‌های دیگری برای ساخت حساب
ارائه کند.)

پس از ورود، به تنظیمات حساب خود در نشانی
[https://crates.io/me/](https://crates.io/me/)<!-- ignore -->
بروید و کلید API خود را دریافت کنید. سپس دستور `cargo login`
را اجرا کنید و در زمان درخواست، توکن API را وارد کنید؛ برای مثال:

```console
$ cargo login
abcdefghijklmnopqrstuvwxyz012345
```

این دستور توکن API شما را در اختیار Cargo قرار می‌دهد و آن را به‌صورت
محلی در فایل _~/.cargo/credentials.toml_ ذخیره می‌کند. توجه داشته
باشید که این توکن یک راز (Secret) است و نباید آن را با هیچ‌کس به
اشتراک بگذارید. اگر به هر دلیلی آن را در اختیار شخص دیگری قرار دادید،
باید توکن قبلی را لغو کنید و توکن تازه‌ای در
[crates.io](https://crates.io/)<!-- ignore --> بسازید.

### افزودن Metadata به Package

فرض کنید Crateای دارید که می‌خواهید آن را منتشر کنید. پیش از انتشار،
باید تعدادی Metadata (Metadata) به بخش `[package]` در فایل
_Cargo.toml_ آن اضافه کنید.

Crate شما باید نامی یکتا داشته باشد. هنگام کار روی یک Crate در
سیستم خودتان، می‌توانید هر نامی که دوست دارید برای آن انتخاب کنید؛
اما نام‌ها در [crates.io](https://crates.io/)<!-- ignore -->
بر اساس اصل «هرکس زودتر ثبت کند» اختصاص داده می‌شوند. وقتی نامی
قبلاً انتخاب شده باشد، شخص دیگری نمی‌تواند Crate دیگری را با همان
نام منتشر کند.

پیش از انتشار، نام موردنظرتان را جست‌وجو کنید. اگر آن نام قبلاً
استفاده شده است، باید نام دیگری انتخاب کنید و فیلد `name` را
در بخش `[package]` فایل _Cargo.toml_ تغییر دهید. برای مثال:

<span class="filename">نام فایل: Cargo.toml</span>

```toml
[package]
name = "guessing_game"
```

حتی اگر نام یکتایی انتخاب کرده باشید، اگر در این مرحله دستور
`cargo publish` را اجرا کنید، ابتدا یک هشدار و سپس یک خطا دریافت
خواهید کرد:

<!-- manual-regeneration
Create a new package with an unregistered name, making no further modifications
  to the generated package, so it is missing the description and license fields.
cargo publish
copy just the relevant lines below
-->

```console
$ cargo publish
    Updating crates.io index
warning: manifest has no description, license, license-file, documentation, homepage or repository.
See https://doc.rust-lang.org/cargo/reference/manifest.html#package-metadata for more info.
--snip--
error: failed to publish to registry at https://crates.io

Caused by:
  the remote server responded with an error (status 400 Bad Request): missing or empty metadata fields: description, license. Please see https://doc.rust-lang.org/cargo/reference/manifest.html for more information on configuring these fields
```

این خطا به این دلیل رخ می‌دهد که اطلاعات مهمی را وارد نکرده‌اید:
توضیح و مجوز (License). این اطلاعات لازم‌اند تا دیگران بدانند
Crate شما چه کاری انجام می‌دهد و تحت چه شرایطی اجازه دارند از آن
استفاده کنند.

در فایل _Cargo.toml_، توضیحی یک یا دو جمله‌ای برای Crate بنویسید؛
چون این توضیح در نتایج جست‌وجو کنار نام Crate نمایش داده می‌شود.
برای فیلد `license` نیز باید یک _شناسهٔ مجوز_ (License Identifier)
وارد کنید. فهرست شناسه‌های قابل‌استفاده در پایگاه دادهٔ
[Software Package Data Exchange (SPDX) متعلق به بنیاد لینوکس][spdx]
قرار دارد.

برای مثال، اگر مجوز MIT را برای Crate خود انتخاب کرده‌اید، شناسهٔ
`MIT` را اضافه کنید:

<span class="filename">نام فایل: Cargo.toml</span>

```toml
[package]
name = "guessing_game"
license = "MIT"
```

اگر می‌خواهید از مجوزی استفاده کنید که در فهرست SPDX وجود ندارد،
باید متن آن مجوز را در فایلی قرار دهید و آن فایل را به پروژه اضافه
کنید. سپس به‌جای کلید `license` از کلید `license-file` استفاده
کنید و نام فایل مجوز را در آن مشخص کنید.

راهنمای انتخاب مجوز مناسب برای پروژه، خارج از محدودهٔ این کتاب است.
بسیاری از افراد در جامعهٔ Rust، مانند خود زبان Rust، از مجوز دوگانهٔ
`MIT OR Apache-2.0` استفاده می‌کنند. این روش نشان می‌دهد که می‌توانید
چند شناسهٔ مجوز را نیز با کلمهٔ `OR` از هم جدا کنید تا پروژه‌تان
تحت چند مجوز قابل استفاده باشد.

وقتی نام یکتا، نسخه، توضیح و مجوز را اضافه کردید، فایل _Cargo.toml_
پروژه‌ای که آمادهٔ انتشار است می‌تواند شبیه نمونهٔ زیر باشد:

<span class="filename">نام فایل: Cargo.toml</span>

```toml
[package]
name = "guessing_game"
version = "0.1.0"
edition = "2024"
description = "A fun game where you guess what number the computer has chosen."
license = "MIT OR Apache-2.0"

[dependencies]
```

[مستندات Cargo](https://doc.rust-lang.org/cargo/) Metadata
دیگری را نیز توضیح می‌دهد که با مشخص کردن آن‌ها، دیگران راحت‌تر
می‌توانند Crate شما را پیدا کنند و از آن استفاده کنند.

### انتشار در Crates.io

اکنون که حساب کاربری ساخته‌اید، توکن API خود را ذخیره کرده‌اید،
نامی برای Package انتخاب کرده‌اید و Metadata ضروری را مشخص کرده‌اید،
آمادهٔ انتشار هستید!

انتشار یک Crate، نسخهٔ مشخصی از آن را در
[crates.io](https://crates.io/)<!-- ignore --> بارگذاری می‌کند
تا دیگران بتوانند از آن استفاده کنند.

مراقب باشید؛ انتشار یک نسخه _دائمی_ است. نسخهٔ منتشرشده را نمی‌توان
بازنویسی کرد و کد آن را نیز جز در شرایط خاص نمی‌توان حذف کرد.
یکی از اهداف اصلی Crates.io این است که آرشیوی دائمی از کدها باشد
تا فرایند ساخت تمام پروژه‌هایی که به Crateهای موجود در
[crates.io](https://crates.io/)<!-- ignore --> وابسته‌اند،
همچنان در آینده کار کند. اگر حذف نسخه‌ها آزاد بود، رسیدن به این
هدف غیرممکن می‌شد. البته برای تعداد نسخه‌هایی که می‌توانید از
یک Crate منتشر کنید محدودیتی وجود ندارد.

دوباره دستور `cargo publish` را اجرا کنید. این بار باید با موفقیت
انجام شود:

<!-- manual-regeneration
go to some valid crate, publish a new version
cargo publish
copy just the relevant lines below
-->

```console
$ cargo publish
    Updating crates.io index
   Packaging guessing_game v0.1.0 (file:///projects/guessing_game)
    Packaged 6 files, 1.2KiB (895.0B compressed)
   Verifying guessing_game v0.1.0 (file:///projects/guessing_game)
   Compiling guessing_game v0.1.0
(file:///projects/guessing_game/target/package/guessing_game-0.1.0)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.19s
   Uploading guessing_game v0.1.0 (file:///projects/guessing_game)
    Uploaded guessing_game v0.1.0 to registry `crates-io`
note: waiting for `guessing_game v0.1.0` to be available at registry
`crates-io`.
You may press ctrl-c to skip waiting; the crate should be available shortly.
   Published guessing_game v0.1.0 at registry `crates-io`
```

تبریک! حالا کدتان را با جامعهٔ Rust به اشتراک گذاشته‌اید و هرکسی
می‌تواند به‌سادگی Crate شما را به‌عنوان وابستگی پروژهٔ خودش اضافه کند.

### انتشار نسخهٔ جدید از یک Crate موجود

وقتی تغییراتی در Crate خود ایجاد کرده‌اید و آمادهٔ انتشار نسخهٔ
جدیدی هستید، مقدار `version` را در فایل _Cargo.toml_ تغییر دهید
و دوباره آن را منتشر کنید. برای انتخاب شمارهٔ نسخهٔ بعدی، بر اساس
نوع تغییراتی که انجام داده‌اید از قواعد
[Semantic Versioning][semver] پیروی کنید.
سپس دستور `cargo publish` را اجرا کنید تا نسخهٔ جدید بارگذاری شود.

<!-- Old headings. Do not remove or links may break. -->

<a id="removing-versions-from-cratesio-with-cargo-yank"></a>
<a id="deprecating-versions-from-cratesio-with-cargo-yank"></a>

### منسوخ کردن نسخه‌ها در Crates.io

با اینکه نمی‌توانید نسخه‌های قبلی یک Crate را حذف کنید، می‌توانید
جلوی اضافه شدن آن نسخه‌ها به‌عنوان وابستگی جدید در پروژه‌های آینده
را بگیرید. این قابلیت زمانی مفید است که نسخه‌ای از Crate به دلیلی
خراب یا مشکل‌دار باشد. Cargo برای چنین شرایطی قابلیتی به نام
Yank کردن نسخه ارائه می‌دهد.

_Yank کردن_ یک نسخه باعث می‌شود پروژه‌های جدید نتوانند به آن نسخه
وابسته شوند؛ اما پروژه‌هایی که از قبل به آن وابسته بوده‌اند همچنان
به کار خود ادامه می‌دهند. به بیان ساده، Yank باعث نمی‌شود پروژه‌هایی
که فایل _Cargo.lock_ دارند خراب شوند؛ در عین حال، فایل‌های
_Cargo.lock_ جدیدی که در آینده تولید می‌شوند از نسخهٔ Yank‌شده
استفاده نخواهند کرد.

برای Yank کردن یک نسخه، وارد پوشهٔ Crateای شوید که قبلاً منتشر
کرده‌اید، دستور `cargo yank` را اجرا کنید و نسخهٔ موردنظر را
مشخص کنید. برای مثال، فرض کنید Crateای به نام `guessing_game`
با نسخهٔ `1.0.1` منتشر کرده‌ایم و می‌خواهیم آن نسخه را Yank کنیم.
در پوشهٔ پروژهٔ `guessing_game` دستور زیر را اجرا می‌کنیم:

<!-- manual-regeneration:
cargo yank carol-test --version 2.1.0
cargo yank carol-test --version 2.1.0 --undo
-->

```console
$ cargo yank --vers 1.0.1
    Updating crates.io index
        Yank guessing_game@1.0.1
```

با اضافه کردن گزینهٔ `--undo` می‌توانید عملیات Yank را نیز
برگردانید تا پروژه‌ها دوباره بتوانند به آن نسخه وابسته شوند:

```console
$ cargo yank --vers 1.0.1 --undo
    Updating crates.io index
      Unyank guessing_game@1.0.1
```

Yank کردن _هیچ کدی را حذف نمی‌کند_. برای مثال، اگر به‌اشتباه
اطلاعات محرمانه‌ای را منتشر کرده باشید، Yank کردن نسخه نمی‌تواند
آن اطلاعات را پاک کند. در چنین شرایطی باید فوراً اطلاعات محرمانه
را تغییر دهید یا باطل کنید.

[spdx]: https://spdx.org/licenses/
[semver]: https://semver.org/

