## توابع

توابع در کد Rust همه‌جا حضور دارند. یکی از مهم‌ترین توابع این زبان را قبلاً دیده‌اید: تابع `main`، که نقطه‌ی ورود بیشتر برنامه‌ها است. کلیدواژه‌ی `fn` را هم دیده‌اید، که به شما اجازه می‌دهد توابع جدیدی اعلان کنید.

کد Rust از _snake case_ به‌عنوان سبک قراردادی برای نام توابع و متغیرها استفاده می‌کند، که در آن همه‌ی حروف کوچک هستند و زیرخط بین کلمات را از هم جدا می‌کند. این‌جا برنامه‌ای می‌بینید که یک مثال از تعریف یک تابع را در خودش دارد:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-16-functions/src/main.rs}}
```

ما یک تابع را در Rust با نوشتن `fn`، دنبال شده با نام تابع و یک جفت پرانتز، تعریف می‌کنیم. آکولادها به کامپایلر می‌گویند بدنه‌ی تابع از کجا شروع می‌شود و کجا تمام می‌شود.

می‌توانیم هر تابعی که تعریف کرده‌ایم را با نوشتن نامش و بعد یک جفت پرانتز صدا بزنیم. چون `another_function` در برنامه تعریف شده، می‌تواند از داخل تابع `main` صدا زده شود. توجه کنید که `another_function` را _بعد_ از تابع `main` در کد سورس تعریف کردیم؛ می‌توانستیم آن را قبل از آن هم تعریف کنیم. برای Rust مهم نیست کجا توابعتان را تعریف می‌کنید، فقط مهم است که آن‌ها جایی در یک اسکوپ که فراخوان (caller) بتواند آن‌ها را ببیند، تعریف شده باشند.

بیایید یک پروژه‌ی باینری جدید به نام _functions_ راه بیندازیم تا توابع را بیشتر بررسی کنیم. مثال `another_function` را در _src/main.rs_ بگذارید و آن را اجرا کنید. باید خروجی زیر را ببینید:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-16-functions/output.txt}}
```

خط‌ها به همان ترتیبی که در تابع `main` می‌آیند اجرا می‌شوند. اول پیام «Hello, world!» چاپ می‌شود، و بعد `another_function` صدا زده می‌شود و پیامش چاپ می‌شود.

### پارامترها

می‌توانیم توابعی تعریف کنیم که _پارامتر_ (parameters) داشته باشند، که متغیرهای خاصی هستند و بخشی از امضای (signature) یک تابع محسوب می‌شوند. وقتی یک تابع پارامتر دارد، می‌توانید مقادیر مشخصی برای آن پارامترها فراهم کنید. از نظر فنی، به این مقادیر مشخص _آرگومان_ (arguments) می‌گویند، اما در گفت‌وگوی روزمره، افراد معمولاً کلمه‌های _پارامتر_ و _آرگومان_ را به‌جای هم استفاده می‌کنند، هم برای متغیرهایی در تعریف یک تابع و هم برای مقادیر مشخصی که موقع صدا زدن یک تابع پاس می‌دهید.

در این نسخه از `another_function`، یک پارامتر اضافه می‌کنیم:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/src/main.rs}}
```

سعی کنید این برنامه را اجرا کنید؛ باید خروجی زیر را بگیرید:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/output.txt}}
```

تعریف `another_function` یک پارامتر به نام `x` دارد. تایپ `x` به‌شکل `i32` مشخص شده. وقتی `5` را به `another_function` پاس می‌دهیم، ماکرو `println!` مقدار `5` را جایی که جفت آکولادهای حاوی `x` در رشته‌ی فرمت بودند، می‌گذارد.

در امضای توابع، _باید_ تایپ هر پارامتر را اعلان کنید. این یک تصمیم سنجیده در طراحی Rust است: نیاز به annotationهای تایپ در تعریف توابع باعث می‌شود کامپایلر تقریباً هیچ‌وقت نیاز نداشته باشد جای دیگری در کد از شما بخواهد که تایپی که منظورتان است را مشخص کنید. کامپایلر همچنین می‌تواند پیام‌های خطای مفیدتری بدهد، اگر بداند تابع چه تایپ‌هایی را انتظار دارد.

وقتی چند پارامتر تعریف می‌کنید، اعلان پارامترها را با کاما از هم جدا کنید، این‌طور:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/src/main.rs}}
```

این مثال یک تابع به نام `print_labeled_measurement` با دو پارامتر می‌سازد. پارامتر اول به نام `value` است و یک `i32` است. پارامتر دوم به نام `unit_label` است و از تایپ `char` است. بعد این تابع متنی چاپ می‌کند که هم `value` و هم `unit_label` را در خودش دارد.

بیایید این کد را اجرا کنیم. برنامه‌ای که الان در فایل _src/main.rs_ پروژه‌ی _functions_ خودتان است را با مثال قبلی جایگزین کنید و آن را با `cargo run` اجرا کنید:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/output.txt}}
```

چون تابع را با `5` به‌عنوان مقدار `value` و `'h'` به‌عنوان مقدار `unit_label` صدا زدیم، خروجی برنامه این مقادیر را در خودش دارد.

{{#quiz ../quizzes/ch03-03-functions-sec1-parameters.toml}}

### Statementها و Expressionها

بدنه‌ی توابع از یک سری statement تشکیل شده که به‌صورت اختیاری با یک expression تمام می‌شوند. تا این‌جا، توابعی که بررسی کردیم یک expression پایانی نداشته‌اند، اما یک expression را به‌عنوان بخشی از یک statement دیده‌اید. چون Rust یک زبان expression-based است، این یک تفاوت مهم است که باید بفهمید. زبان‌های دیگر همین تفاوت‌ها را ندارند، پس بیایید ببینیم statementها و expressionها چه هستند و تفاوتشان چطور روی بدنه‌ی توابع اثر می‌گذارد.

- _statementها_ دستوراتی هستند که یک کاری انجام می‌دهند و هیچ مقداری برنمی‌گردانند.
- _expressionها_ به یک مقدار نتیجه محاسبه می‌شوند.

بیایید چند مثال را ببینیم. در واقع قبلاً هم از statementها و expressionها استفاده کرده‌ایم. ساختن یک متغیر و اختصاص یک مقدار به آن با کلیدواژه‌ی `let` یک statement است. در Listing 3-1، `let y = 6;` یک statement است.

<Listing number="3-1" file-name="src/main.rs" caption="یک اعلان تابع `main` که یک statement در خودش دارد">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-01/src/main.rs}}
```

</Listing>

تعریف توابع هم statement هستند؛ کل مثال قبلی خودش یک statement است. (همان‌طور که به‌زودی می‌بینیم، صدا زدن یک تابع اما یک statement نیست.)

statementها مقداری برنمی‌گردانند. به همین دلیل، نمی‌توانید یک statement مربوط به `let` را به متغیر دیگری اختصاص دهید، همان‌طور که کد زیر سعی می‌کند این کار را انجام دهد؛ خطا خواهید گرفت:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/src/main.rs}}
```

وقتی این برنامه را اجرا کنید، خطایی که می‌گیرید این‌طور است:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/output.txt}}
```

statement `let y = 6` هیچ مقداری برنمی‌گرداند، پس چیزی برای `x` وجود ندارد که به آن مقید شود. این با اتفاقی که در زبان‌های دیگر، مثل C و Ruby، رخ می‌دهد فرق دارد، جایی که یک عبارت اختصاص (assignment) مقدار همان اختصاص را برمی‌گرداند. در آن زبان‌ها، می‌توانید `x = y = 6` بنویسید و هم `x` و هم `y` مقدار `6` را داشته باشند؛ در Rust این‌طور نیست.

expressionها به یک مقدار محاسبه می‌شوند و بیشتر بقیه‌ی کدی که در Rust می‌نویسید را تشکیل می‌دهند. یک عملیات ریاضی را در نظر بگیرید، مثل `5 + 6`، که یک expression است که به مقدار `11` محاسبه می‌شود. expressionها می‌توانند بخشی از statementها باشند: در Listing 3-1، `6` در statement `let y = 6;` یک expression است که به مقدار `6` محاسبه می‌شود. صدا زدن یک تابع یک expression است. صدا زدن یک ماکرو یک expression است. یک بلوک اسکوپ جدید که با آکولادها ساخته شده یک expression است، برای مثال:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-20-blocks-are-expressions/src/main.rs}}
```

این expression:

```rust,ignore
{
    let x = 3;
    x + 1
}
```

یک بلوک است که در این مورد، به `4` محاسبه می‌شود. آن مقدار به `y` مقید می‌شود، به‌عنوان بخشی از statement `let`. توجه کنید که خط `x + 1` بدون علامت نقطه‌ویرگول در انتها آمده، که برخلاف بیشتر خط‌هایی است که تا الان دیده‌اید. expressionها علامت نقطه‌ویرگول پایانی ندارند. اگر یک علامت نقطه‌ویرگول به انتهای یک expression اضافه کنید، آن را به یک statement تبدیل می‌کنید، و در آن صورت دیگر هیچ مقداری برنمی‌گرداند. همین موضوع را در ذهن داشته باشید وقتی در ادامه مقادیر بازگشتی توابع و expressionها را بررسی می‌کنیم.

### توابع با مقادیر بازگشتی

توابع می‌توانند مقادیری را به کدی که آن‌ها را صدا زده برگردانند. ما مقادیر بازگشتی را نام‌گذاری نمی‌کنیم، اما باید تایپشان را بعد از یک پیکان (`->`) اعلان کنیم. در Rust، مقدار بازگشتی یک تابع معادل مقدار آخرین expression در بلوک بدنه‌ی آن تابع است. می‌توانید با استفاده از کلیدواژه‌ی `return` و مشخص کردن یک مقدار، زودتر از یک تابع خارج شوید، اما بیشتر توابع آخرین expression را به‌صورت implicit برمی‌گردانند. این‌جا یک مثال از تابعی که یک مقدار برمی‌گرداند را می‌بینید:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/src/main.rs}}
```

هیچ فراخوانی تابع، ماکرو، یا حتی statement مربوط به `let`ای در تابع `five` وجود ندارد — فقط خودِ عدد `5` به‌تنهایی. این یک تابع کاملاً معتبر در Rust است. توجه کنید که تایپ بازگشتی تابع هم مشخص شده، به‌شکل `-> i32`. سعی کنید این کد را اجرا کنید؛ خروجی باید این‌طور باشد:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/output.txt}}
```

`5` در `five` مقدار بازگشتی تابع است، به همین دلیل تایپ بازگشتی `i32` است. بیایید این را با جزئیات بیشتری بررسی کنیم. دو نکته‌ی مهم هست: اول، خط `let x = five();` نشان می‌دهد که از مقدار بازگشتی یک تابع برای مقداردهی اولیه‌ی یک متغیر استفاده می‌کنیم. چون تابع `five` مقدار `5` را برمی‌گرداند، آن خط با خط زیر یکسان است:

```rust
let x = 5;
```

دوم، تابع `five` هیچ پارامتری ندارد و تایپ مقدار بازگشتی را تعریف می‌کند. بدنه‌ی تابع فقط یک `5` تنهاست بدون هیچ علامت نقطه‌ویرگولی، چون این یک expression است که می‌خواهیم مقدارش را برگردانیم.

بیایید یک مثال دیگر را ببینیم:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-22-function-parameter-and-return/src/main.rs}}
```

اجرای این کد `The value of x is: 6` را چاپ می‌کند. اما اگر یک علامت نقطه‌ویرگول به انتهای خطی که `x + 1` را دارد اضافه کنیم چه اتفاقی می‌افتد، و آن را از یک expression به یک statement تبدیل کنیم؟

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/src/main.rs}}
```

کامپایل کردن این کد یک خطا تولید می‌کند، این‌طور:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/output.txt}}
```

پیام اصلی خطا، `mismatched types`، مشکل اصلی این کد را نشان می‌دهد. تعریف تابع `plus_one` می‌گوید که یک `i32` برمی‌گرداند، اما statementها به یک مقدار محاسبه نمی‌شوند، که با `()`، تایپ unit، بیان می‌شود. به همین دلیل، هیچ چیزی برگردانده نمی‌شود، که با تعریف تابع در تناقض است و منجر به یک خطا می‌شود. در این خروجی، Rust پیامی می‌دهد که احتمالاً به رفع این مشکل کمک می‌کند: پیشنهاد می‌دهد که علامت نقطه‌ویرگول را حذف کنید، که خطا را برطرف می‌کند.

{{#quiz ../quizzes/ch03-03-functions-sec2-expressions.toml}}
