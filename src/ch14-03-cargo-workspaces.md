## فضای کاری Cargo (Cargo Workspaces)

در فصل ۱۲ بسته‌ای ساختیم که شامل یک Crate اجرایی و یک Crate کتابخانه‌ای
بود. با رشد پروژه ممکن است متوجه شوید که Crate کتابخانه‌ای بزرگ‌تر و
بزرگ‌تر می‌شود و می‌خواهید بستهٔ خود را به چند Crate کتابخانه‌ای کوچک‌تر
تقسیم کنید. Cargo قابلیتی به نام _فضای کاری_ (Workspace) ارائه می‌دهد
که به مدیریت چند بستهٔ مرتبط با یکدیگر کمک می‌کند؛ بسته‌هایی که هم‌زمان
توسعه داده می‌شوند.

### ساختن یک فضای کاری

یک _Workspace_ مجموعه‌ای از بسته‌هاست که فایل `Cargo.lock` و پوشهٔ
خروجی مشترکی دارند. بیایید با استفاده از Workspace یک پروژه بسازیم.
برای اینکه بتوانیم روی ساختار Workspace تمرکز کنیم، از کدی ساده و
ابتدایی استفاده خواهیم کرد. روش‌های مختلفی برای سازمان‌دهی یک Workspace
وجود دارد؛ ما یکی از ساختارهای رایج را نشان می‌دهیم.

Workspace ما شامل یک برنامهٔ اجرایی و دو کتابخانه خواهد بود. برنامهٔ
اجرایی، که قابلیت اصلی را فراهم می‌کند، به آن دو کتابخانه وابسته است.
یکی از کتابخانه‌ها تابعی به نام `add_one` و دیگری تابعی به نام
`add_two` ارائه می‌کند. هر سه Crate عضو یک Workspace خواهند بود.
ابتدا یک پوشهٔ جدید برای Workspace می‌سازیم:

```console
$ mkdir add
$ cd add
```

سپس در پوشهٔ _add_ فایلی به نام _Cargo.toml_ ایجاد می‌کنیم تا کل
Workspace را پیکربندی کند. این فایل بخش `[package]` نخواهد داشت؛
در عوض با بخش `[workspace]` شروع می‌شود که به ما اجازه می‌دهد اعضای
Workspace را به آن اضافه کنیم. همچنین با قرار دادن مقدار `"3"` برای
گزینهٔ `resolver`، از جدیدترین الگوریتم حل وابستگی Cargo استفاده
می‌کنیم:

<span class="filename">نام فایل: Cargo.toml</span>

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-01-workspace/add/Cargo.toml}}
```

در مرحلهٔ بعد، با اجرای `cargo new` درون پوشهٔ _add_، Crate اجرایی
`adder` را ایجاد می‌کنیم:

<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-01-adder-crate/add
remove `members = ["adder"]` from Cargo.toml
rm -rf adder
cargo new adder
copy output below
-->

```console
$ cargo new adder
     Created binary (application) `adder` package
      Adding `adder` as member of workspace at `file:///projects/add`
```

اجرای `cargo new` درون یک Workspace، بستهٔ تازه‌ساخته‌شده را به‌طور
خودکار به فهرست کلید `members` در بخش `[workspace]` فایل
_Cargo.toml_ اضافه می‌کند؛ چیزی شبیه به این:

```toml
{{#include ../listings/ch14-more-about-cargo/output-only-01-adder-crate/add/Cargo.toml}}
```

در این مرحله می‌توانیم با اجرای `cargo build`، کل Workspace را
کامپایل کنیم. فایل‌ها و پوشه‌های موجود در مسیر _add_ باید ساختاری
شبیه به این داشته باشند:

```text
├── Cargo.lock
├── Cargo.toml
├── adder
│   ├── Cargo.toml
│   └── src
│       └── main.rs
└── target
```

Workspace فقط یک پوشهٔ `target` در سطح ریشه دارد و فایل‌های کامپایل‌شده
در همان‌جا قرار می‌گیرند؛ بستهٔ `adder` پوشهٔ `target` جداگانه‌ای
ندارد. حتی اگر دستور `cargo build` را از داخل پوشهٔ _adder_ اجرا کنیم،
خروجی کامپایل همچنان در مسیر _add/target_ قرار می‌گیرد، نه در
_add/adder/target_.

Cargo پوشهٔ `target` را در Workspace به این شکل سازمان‌دهی می‌کند،
چون Crateهای یک Workspace قرار است به یکدیگر وابسته باشند. اگر هر Crate
پوشهٔ `target` جداگانه‌ای داشت، برای قرار دادن خروجی‌ها در پوشهٔ
مخصوص خودش مجبور بود Crateهای دیگر Workspace را دوباره کامپایل کند.
با به‌اشتراک‌گذاشتن یک پوشهٔ `target`، از کامپایل‌های غیرضروری جلوگیری
می‌شود.

### ساختن دومین بسته در Workspace

حالا بیایید عضو دیگری به Workspace اضافه کنیم و نام آن را `add_one`
بگذاریم. یک Crate کتابخانه‌ای جدید با همین نام ایجاد کنید:

<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-02-add-one/add
remove `"add_one"` from `members` list in Cargo.toml
rm -rf add_one
cargo new add_one --lib
copy output below
-->

```console
$ cargo new add_one --lib
     Created library `add_one` package
      Adding `add_one` as member of workspace at `file:///projects/add`
```

اکنون فایل _Cargo.toml_ در ریشه، مسیر _add_one_ را نیز در فهرست
`members` خواهد داشت:

<span class="filename">نام فایل: Cargo.toml</span>

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-02-workspace-with-two-crates/add/Cargo.toml}}
```

در این مرحله پوشهٔ _add_ باید شامل فایل‌ها و پوشه‌هایی با ساختار زیر
باشد:

```text
├── Cargo.lock
├── Cargo.toml
├── add_one
│   ├── Cargo.toml
│   └── src
│       └── lib.rs
├── adder
│   ├── Cargo.toml
│   └── src
│       └── main.rs
└── target
```

در فایل _add_one/src/lib.rs_ تابعی به نام `add_one` اضافه می‌کنیم:

<span class="filename">نام فایل: add_one/src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch14-more-about-cargo/no-listing-02-workspace-with-two-crates/add/add_one/src/lib.rs}}
```

حالا می‌توانیم کاری کنیم که بستهٔ `adder`، که برنامهٔ اجرایی ما را
در خود دارد، به بستهٔ `add_one` که کتابخانهٔ ماست وابسته شود. ابتدا
باید یک وابستگی مبتنی بر مسیر (Path Dependency) به `add_one` در فایل
_adder/Cargo.toml_ اضافه کنیم:

<span class="filename">نام فایل: adder/Cargo.toml</span>

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-02-workspace-with-two-crates/add/adder/Cargo.toml:6:7}}
```

Cargo فرض نمی‌کند که Crateهای موجود در یک Workspace لزوماً به یکدیگر
وابسته‌اند؛ بنابراین باید رابطهٔ وابستگی میان آن‌ها را به‌صراحت مشخص
کنیم.

در مرحلهٔ بعد، از تابع `add_one` (متعلق به Crate کتابخانه‌ای
`add_one`) در Crate مربوط به `adder` استفاده می‌کنیم. فایل
_adder/src/main.rs_ را باز کنید و تابع `main` را طوری تغییر دهید
که تابع `add_one` را فراخوانی کند؛ همان‌طور که در Listing 14-7
می‌بینید.

<Listing number="14-7" file-name="adder/src/main.rs" caption="استفاده از Crate کتابخانه‌ای add_one در Crate مربوط به adder">

```rust,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-07/add/adder/src/main.rs}}
```

</Listing>

حالا با اجرای دستور `cargo build` در پوشهٔ ریشهٔ _add_، کل Workspace
را کامپایل می‌کنیم!

<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
$ cargo build
   Compiling add_one v0.1.0 (file:///projects/add/add_one)
   Compiling adder v0.1.0 (file:///projects/add/adder)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.22s
```

برای اجرای Crate باینری از پوشهٔ _add_، می‌توانیم با استفاده از آرگومان
`-p` و نام بسته به Cargo بگوییم کدام بسته از Workspace را اجرا کند:

<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo run -p adder
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
$ cargo run -p adder
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/adder`
Hello, world! 10 plus one is 11!
```

این دستور کد موجود در _adder/src/main.rs_ را اجرا می‌کند؛ کدی که به
Crate کتابخانه‌ای `add_one` وابسته است.

<!-- Old headings. Do not remove or links may break. -->

<a id="depending-on-an-external-package-in-a-workspace"></a>

### وابستگی به یک بستهٔ خارجی در Workspace

توجه کنید که Workspace فقط یک فایل `Cargo.lock` در پوشهٔ ریشه دارد،
نه یک فایل `Cargo.lock` در پوشهٔ هر Crate. این کار تضمین می‌کند که
تمام Crateها از نسخهٔ یکسانی از وابستگی‌ها استفاده کنند.

اگر بستهٔ `rand` را به فایل‌های _adder/Cargo.toml_ و
_add_one/Cargo.toml_ اضافه کنیم، Cargo برای هر دو بسته یک نسخه از
`rand` را انتخاب می‌کند و اطلاعات آن را در همان فایل `Cargo.lock`
ثبت می‌کند. یکسان بودن نسخهٔ وابستگی‌ها در تمام Crateهای Workspace
باعث می‌شود آن‌ها همواره با یکدیگر سازگار باشند.

بیایید Crate مربوط به `rand` را به بخش `[dependencies]` در فایل
_add_one/Cargo.toml_ اضافه کنیم تا بتوانیم در Crate `add_one` از
آن استفاده کنیم:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:

* ch01-01-installation.md
* ch02-00-guessing-game-tutorial.md
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
-->

<span class="filename">نام فایل: add_one/Cargo.toml</span>

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-03-workspace-with-external-dependency/add/add_one/Cargo.toml:6:7}}
```

اکنون می‌توانیم عبارت `use rand;` را به فایل
_add_one/src/lib.rs_ اضافه کنیم. با اجرای `cargo build` در پوشهٔ
_add_، کل Workspace ساخته می‌شود و Crate مربوط به `rand` نیز دریافت
و کامپایل خواهد شد. چون هنوز از `rand`ای که وارد محدوده کرده‌ایم
استفاده‌ای نمی‌کنیم، یک هشدار دریافت خواهیم کرد:

<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-03-workspace-with-external-dependency/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
$ cargo build
    Updating crates.io index
  Downloaded rand v0.10.1
   --snip--
   Compiling rand v0.10.1
   Compiling add_one v0.1.0 (file:///projects/add/add_one)
warning: unused import: `rand`
 --> add_one/src/lib.rs:1:5
  |
1 | use rand;
  |     ^^^^
  |
  = note: `#[warn(unused_imports)]` (part of `#[warn(unused)]`) on by default

warning: `add_one` (lib) generated 1 warning (run `cargo fix --lib -p add_one` to apply 1 suggestion)
   Compiling adder v0.1.0 (file:///projects/add/adder)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.95s
```

اکنون فایل `Cargo.lock` در ریشه، اطلاعات وابستگی `add_one` به
`rand` را در خود دارد. با این حال، با اینکه در بخشی از Workspace
از `rand` استفاده شده است، نمی‌توانیم در Crateهای دیگر Workspace
از آن استفاده کنیم، مگر اینکه `rand` را به فایل _Cargo.toml_ همان
بسته‌ها نیز اضافه کنیم.

برای مثال، اگر عبارت `use rand;` را به فایل _adder/src/main.rs_
در بستهٔ `adder` اضافه کنیم، با خطا روبه‌رو می‌شویم:

<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-03-use-rand/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
$ cargo build
  --snip--
   Compiling adder v0.1.0 (file:///projects/add/adder)
error[E0432]: unresolved import `rand`
 --> adder/src/main.rs:2:5
  |
2 | use rand;
  |     ^^^^ no external crate `rand`
```

برای رفع این خطا، فایل _Cargo.toml_ مربوط به بستهٔ `adder` را ویرایش
کنید و مشخص کنید که `rand` یکی از وابستگی‌های آن بسته نیز هست.
با ساخت بستهٔ `adder`، وابستگی `rand` به فهرست وابستگی‌های این بسته
در فایل `Cargo.lock` اضافه می‌شود؛ اما نسخهٔ دیگری از `rand`
دانلود نخواهد شد.

Cargo تضمین می‌کند که تمام Crateهای تمام بسته‌های Workspace که از
`rand` استفاده می‌کنند، تا زمانی که نسخه‌های سازگاری از آن را
درخواست کرده باشند، از همان نسخه استفاده کنند. این کار هم در فضای
دیسک صرفه‌جویی می‌کند و هم سازگاری Crateهای Workspace با یکدیگر را
حفظ می‌کند.

اگر Crateهای موجود در Workspace نسخه‌های ناسازگاری از یک وابستگی
مشترک درخواست کنند، Cargo می‌تواند هر دو نسخه را انتخاب کند؛ با این
حال تلاش می‌کند تعداد نسخه‌های متفاوت را تا حد امکان کم نگه دارد.

### اضافه کردن آزمون به Workspace

برای بهبود بیشتر، بیایید آزمونی برای تابع `add_one::add_one` درون
Crate مربوط به `add_one` بنویسیم:

<span class="filename">نام فایل: add_one/src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add/add_one/src/lib.rs}}
```

حالا دستور `cargo test` را در پوشهٔ ریشهٔ _add_ اجرا کنید. اجرای
`cargo test` در Workspaceای با این ساختار، آزمون‌های تمام Crateهای
موجود در Workspace را اجرا می‌کند:

<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test
copy output below; the output updating script doesn't handle subdirectories in
paths properly
-->

```console
$ cargo test
   Compiling add_one v0.1.0 (file:///projects/add/add_one)
   Compiling adder v0.1.0 (file:///projects/add/adder)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.20s
     Running unittests src/lib.rs (target/debug/deps/add_one-93c49ee75dc46543)

running 1 test
test tests::it_works ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests src/main.rs (target/debug/deps/adder-3a47283c568d2b6a)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

   Doc-tests add_one

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

بخش اول خروجی نشان می‌دهد آزمون `it_works` در Crate مربوط به
`add_one` با موفقیت اجرا شده است. بخش بعدی نشان می‌دهد که در Crate
`adder` هیچ آزمونی پیدا نشده و بخش پایانی نیز نشان می‌دهد که هیچ
آزمون مستندسازی‌ای (Documentation Test) در Crate `add_one` وجود
نداشته است.

همچنین می‌توانیم از پوشهٔ ریشه، آزمون‌های یک Crate مشخص در Workspace
را اجرا کنیم. برای این کار از پرچم `-p` استفاده می‌کنیم و نام Crate
موردنظر را به آن می‌دهیم:

<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test -p add_one
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
$ cargo test -p add_one
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.00s
     Running unittests src/lib.rs (target/debug/deps/add_one-93c49ee75dc46543)

running 1 test
test tests::it_works ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

   Doc-tests add_one

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

این خروجی نشان می‌دهد که دستور `cargo test` فقط آزمون‌های Crate
`add_one` را اجرا کرده و آزمون‌های Crate `adder` را اجرا نکرده است.

اگر Crateهای موجود در Workspace را در
[crates.io](https://crates.io/)<!-- ignore --> منتشر کنید، هر Crate
باید جداگانه منتشر شود. درست مانند `cargo test`، برای انتشار یک Crate
مشخص از Workspace نیز می‌توانیم از پرچم `-p` استفاده کنیم و نام
بستهٔ موردنظر را مشخص کنیم.

برای تمرین بیشتر، یک Crate دیگر به نام `add_two` به همین روش و با
الگو گرفتن از Crate `add_one` به Workspace اضافه کنید!

با بزرگ‌تر شدن پروژه، استفاده از Workspace را در نظر بگیرید. Workspace
به شما اجازه می‌دهد پروژه را به بخش‌های کوچک‌تر و قابل‌فهم‌تری تقسیم
کنید، به‌جای اینکه همه‌چیز در یک تودهٔ بزرگ از کد قرار بگیرد. علاوه بر
این، اگر Crateها اغلب هم‌زمان تغییر می‌کنند، نگهداری آن‌ها در یک
Workspace هماهنگی میانشان را آسان‌تر می‌کند.

> 💡 **نکتهٔ مدرس:** سه مفهوم را از هم جدا نگه دارید:
> `[workspace].members` مشخص می‌کند چه بسته‌هایی عضو فضای کاری‌اند؛
> اما این عضویت به‌تنهایی به معنی وابستگی یک بسته به بستهٔ دیگر نیست.
> برای وابستگی محلی باید در `[dependencies]` مسیر آن بسته را بنویسیم.
> همچنین `-p` برای انتخاب یک بسته از میان اعضای Workspace استفاده
> می‌شود؛ مثلاً `cargo test -p add_one`.
