## Control Flow

توانایی اجرا کردن مقداری کد بسته به این‌که یک شرط `true` باشد یا نه، و توانایی اجرا کردن مقداری کد به‌صورت تکراری تا وقتی یک شرط `true` است، از ساده‌ترین قطعات سازنده در بیشتر زبان‌های برنامه‌نویسی هستند. رایج‌ترین ساختارهایی که به شما اجازه می‌دهند جریان اجرای کد Rust را کنترل کنید، عبارت‌های `if` و loopها هستند.

### عبارت‌های `if`

یک عبارت `if` به شما اجازه می‌دهد کدتان را بسته به شرط‌ها شاخه‌بندی (branch) کنید. یک شرط می‌دهید و بعد می‌گویید: «اگر این شرط برقرار بود، این بلوک کد را اجرا کن. اگر شرط برقرار نبود، این بلوک کد را اجرا نکن.»

یک پروژه‌ی جدید به نام _branches_ در پوشه‌ی _projects_ خودتان بسازید تا عبارت `if` را بررسی کنید. در فایل _src/main.rs_، این را وارد کنید:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/src/main.rs}}
```

همه‌ی عبارت‌های `if` با کلیدواژه‌ی `if` شروع می‌شوند، که یک شرط بعد از آن می‌آید. در این مورد، شرط بررسی می‌کند که آیا متغیر `number` مقداری کمتر از ۵ دارد یا نه. بلوک کدی که اگر شرط `true` بود باید اجرا شود را دقیقاً بعد از شرط، داخل آکولادها می‌گذاریم. به بلوک‌های کدی که با شرط‌ها در عبارت‌های `if` مرتبط هستند، گاهی _arm_ هم می‌گویند، دقیقاً مثل armهایی که در عبارت‌های `match` که در بخش [«مقایسه‌ی حدس با عدد مخفی»][comparing-the-guess-to-the-secret-number]<!-- ignore --> از فصل ۲ بررسی کردیم.

به‌صورت اختیاری، می‌توانیم یک عبارت `else` هم بیاوریم، که این‌جا این کار را کرده‌ایم، تا به برنامه یک بلوک کد جایگزین بدهیم که اگر شرط به `false` محاسبه شد اجرا شود. اگر یک عبارت `else` نیاورید و شرط `false` باشد، برنامه فقط از بلوک `if` می‌گذرد و به قطعه‌ی بعدی کد می‌رود.

سعی کنید این کد را اجرا کنید؛ باید خروجی زیر را ببینید:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/output.txt}}
```

بیایید مقدار `number` را به مقداری تغییر دهیم که شرط را `false` می‌کند تا ببینیم چه اتفاقی می‌افتد:

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/src/main.rs:here}}
```

برنامه را دوباره اجرا کنید، و به خروجی نگاهی بیندازید:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/output.txt}}
```

همچنین این نکته ارزش دارد که بگوییم شرط در این کد _باید_ یک `bool` باشد. اگر شرط یک `bool` نباشد، یک خطا می‌گیریم. برای مثال، سعی کنید کد زیر را اجرا کنید:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/src/main.rs}}
```

این‌بار شرط `if` به مقدار `3` محاسبه می‌شود، و Rust یک خطا می‌دهد:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/output.txt}}
```

این خطا نشان می‌دهد که Rust یک `bool` انتظار داشته اما یک integer گرفته. برخلاف زبان‌هایی مثل Ruby و JavaScript، Rust به‌صورت خودکار سعی نمی‌کند تایپ‌های غیر بولین را به یک بولین تبدیل کند. باید صریح باشید و همیشه یک بولین را به‌عنوان شرط به `if` بدهید. اگر بخواهیم بلوک کد `if` فقط وقتی اجرا شود که یک عدد برابر با `0` نباشد، برای مثال، می‌توانیم عبارت `if` را به این‌طور تغییر دهیم:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-29-if-not-equal-0/src/main.rs}}
```

اجرای این کد `number was something other than zero` را چاپ می‌کند.

#### مدیریت چند شرط با `else if`

می‌توانید با ترکیب کردن `if` و `else` در یک عبارت `else if`، از چند شرط استفاده کنید. برای مثال:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/src/main.rs}}
```

این برنامه چهار مسیر ممکن دارد که می‌تواند برود. بعد از اجرایش، باید خروجی زیر را ببینید:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/output.txt}}
```

وقتی این برنامه اجرا می‌شود، هر عبارت `if` را به‌ترتیب بررسی می‌کند و اولین بدنه‌ای را اجرا می‌کند که شرطش به `true` محاسبه شود. توجه کنید که با این‌که ۶ بر ۲ بخش‌پذیر است، خروجی `number is divisible by 2` را نمی‌بینیم، و متن `number is not divisible by 4, 3, or 2` را هم از بلوک `else` نمی‌بینیم. چون Rust فقط بلوک متناظر با اولین شرط `true` را اجرا می‌کند، و همین که یکی را پیدا کند، حتی بقیه را هم بررسی نمی‌کند.

استفاده از تعداد زیادی عبارت `else if` می‌تواند کدتان را نامنظم کند، پس اگر بیشتر از یکی دارید، شاید بخواهید کدتان را بازنویسی کنید. فصل ۶ یک ساختار قدرتمند از Rust برای شاخه‌بندی به نام `match` را برای همین موارد توضیح می‌دهد.

#### استفاده از `if` در یک عبارت `let`

چون `if` یک expression است، می‌توانیم از آن در سمت راست یک عبارت `let` استفاده کنیم تا نتیجه را به یک متغیر اختصاص دهیم، همان‌طور که در Listing 3-2 نشان داده شده.

<Listing number="3-2" file-name="src/main.rs" caption="اختصاص دادن نتیجه‌ی یک عبارت `if` به یک متغیر">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-02/src/main.rs}}
```

</Listing>

متغیر `number` بر اساس نتیجه‌ی عبارت `if` به یک مقدار مقید می‌شود. این کد را اجرا کنید تا ببینید چه اتفاقی می‌افتد:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-02/output.txt}}
```

یادتان باشد که بلوک‌های کد به آخرین expressionای که در خودشان دارند محاسبه می‌شوند، و عددها هم به‌تنهایی expression هستند. در این مورد، مقدار کل عبارت `if` به این بستگی دارد که کدام بلوک کد اجرا شود. یعنی مقادیری که احتمال دارد از هر arm از `if` نتیجه شوند، باید از یک تایپ یکسان باشند؛ در Listing 3-2، نتیجه‌ی هم arm `if` و هم arm `else` عدد صحیح `i32` بودند. اگر تایپ‌ها با هم مطابقت نداشته باشند، مثل مثال زیر، یک خطا می‌گیریم:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/src/main.rs}}
```

وقتی سعی می‌کنیم این کد را کامپایل کنیم، یک خطا می‌گیریم. armهای `if` و `else` تایپ‌های مقداری دارند که با هم سازگار نیستند، و Rust دقیقاً نشان می‌دهد کجای برنامه باید مشکل را پیدا کنید:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/output.txt}}
```

expression در بلوک `if` به یک integer محاسبه می‌شود، و expression در بلوک `else` به یک رشته محاسبه می‌شود. این کار نمی‌کند، چون متغیرها باید یک تایپ واحد داشته باشند، و Rust باید در زمان کامپایل قطعاً بداند که متغیر `number` چه تایپی است. دانستن تایپ `number` به کامپایلر اجازه می‌دهد بررسی کند که آن تایپ در همه‌جایی که از `number` استفاده می‌کنیم معتبر است. Rust نمی‌توانست این کار را انجام دهد اگر تایپ `number` فقط در زمان اجرا مشخص می‌شد؛ کامپایلر پیچیده‌تر می‌شد و تضمین‌های کمتری درباره‌ی کد می‌داد، اگر مجبور بود چند تایپ فرضی را برای هر متغیر زیر نظر بگیرد.

{{#quiz ../quizzes/ch03-05-control-flow-sec1-if.toml}}

### تکرار با Loopها

معمولاً مفید است که یک بلوک کد را بیشتر از یک بار اجرا کنیم. برای این کار، Rust چند _loop_ فراهم می‌کند، که کد داخل بدنه‌ی loop را تا آخر اجرا می‌کنند و بعد بلافاصله دوباره از ابتدا شروع می‌کنند. برای آزمایش loopها، بیایید یک پروژه‌ی جدید به نام _loops_ بسازیم.

Rust سه نوع loop دارد: `loop`، `while` و `for`. بیایید هر کدام را امتحان کنیم.

#### تکرار کد با `loop`

کلیدواژه‌ی `loop` به Rust می‌گوید که یک بلوک کد را بارها و بارها اجرا کند، یا برای همیشه یا تا وقتی که صریحاً به آن بگویید متوقف شود.

برای مثال، فایل _src/main.rs_ در پوشه‌ی _loops_ خودتان را این‌طور تغییر دهید:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-loop/src/main.rs}}
```

وقتی این برنامه را اجرا کنیم، `again!` را می‌بینیم که مدام و پیوسته چاپ می‌شود تا وقتی که خودمان برنامه را دستی متوقف کنیم. بیشتر ترمینال‌ها از شورتکات صفحه‌کلید <kbd>ctrl</kbd>-<kbd>C</kbd> برای قطع کردن برنامه‌ای که در یک loop پیوسته گیر کرده پشتیبانی می‌کنند. امتحانش کنید:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-32-loop
cargo run
CTRL-C
-->

```console
$ cargo run
   Compiling loops v0.1.0 (file:///projects/loops)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/loops`
again!
again!
again!
again!
^Cagain!
```

نماد `^C` جایی را نشان می‌دهد که <kbd>ctrl</kbd>-<kbd>C</kbd> را زده‌اید.

ممکن است کلمه‌ی `again!` را بعد از `^C` ببینید یا نبینید، بسته به این‌که کد در چه نقطه‌ای از loop بود وقتی سیگنال قطع را گرفت.

خوشبختانه، Rust همچنین راهی برای خارج شدن از یک loop با استفاده از کد فراهم می‌کند. می‌توانید کلیدواژه‌ی `break` را داخل loop بگذارید تا به برنامه بگویید کِی باید اجرای loop را متوقف کند. یادتان باشد که در بازی حدس زدن عدد، این کار را در بخش [«خروج بعد از یک حدس درست»][quitting-after-a-correct-guess]<!-- ignore --> از فصل ۲ انجام دادیم تا وقتی کاربر با حدس زدن عدد درست برنده می‌شد، از برنامه خارج شویم.

همچنین در بازی حدس زدن عدد از `continue` استفاده کردیم، که در یک loop به برنامه می‌گوید هر کد باقی‌مانده در همین تکرار از loop را رد کند و به تکرار بعدی برود.

#### برگرداندن مقادیر از Loopها

یکی از کاربردهای یک `loop` این است که یک عملیاتی را که می‌دانید ممکن است شکست بخورد، دوباره امتحان کنید، مثل بررسی این‌که آیا یک thread کارش را تمام کرده یا نه. ممکن است لازم باشد نتیجه‌ی آن عملیات را از داخل loop به بقیه‌ی کدتان پاس بدهید. برای این کار، می‌توانید مقداری که می‌خواهید برگردانده شود را بعد از عبارت `break` که برای متوقف کردن loop استفاده می‌کنید بگذارید؛ آن مقدار از loop برگردانده می‌شود تا بتوانید استفاده‌اش کنید، همان‌طور که این‌جا نشان داده شده:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-33-return-value-from-loop/src/main.rs}}
```

قبل از loop، یک متغیر به نام `counter` اعلان می‌کنیم و آن را با `0` مقداردهی اولیه می‌کنیم. بعد، یک متغیر به نام `result` اعلان می‌کنیم تا مقدار برگردانده‌شده از loop را نگه دارد. در هر تکرار از loop، عدد `1` را به متغیر `counter` اضافه می‌کنیم، و بعد بررسی می‌کنیم که آیا `counter` برابر با `10` است یا نه.
وقتی این‌طور بود، از کلیدواژه‌ی `break` همراه با مقدار `counter * 2` استفاده می‌کنیم.
بعد از loop، از یک علامت نقطه‌ویرگول برای پایان دادن به statementای که این مقدار را به `result` اختصاص می‌دهد استفاده می‌کنیم. در نهایت، مقدار داخل `result` را چاپ می‌کنیم، که در این مورد `20` است.

همچنین می‌توانید از داخل یک loop از `return` استفاده کنید. در حالی که `break` فقط از loop فعلی خارج می‌شود، `return` همیشه از تابع فعلی خارج می‌شود.

> *نکته:* علامت نقطه‌ویرگول بعد از `break counter * 2` از نظر فنی اختیاری است. `break` خیلی شبیه `return` است، از این نظر که هر دو می‌توانند به‌صورت اختیاری یک expression را به‌عنوان آرگومان بگیرند، و هر دو باعث یک تغییر در control flow می‌شوند.
> کد بعد از یک `break` یا `return` هیچ‌وقت اجرا نمی‌شود، پس کامپایلر Rust یک عبارت `break` و یک عبارت `return` را طوری در نظر می‌گیرد که گویی مقدار unit، یا `()`، را دارند.

<!-- Old headings. Do not remove or links may break. -->
<a id="loop-labels-to-disambiguate-between-multiple-loops"></a>

#### رفع ابهام با Loop Labelها

اگر loopهایی داخل loopهای دیگر داشته باشید، `break` و `continue` در آن نقطه روی درونی‌ترین loop اثر می‌گذارند. می‌توانید به‌صورت اختیاری یک _loop label_ روی یک loop مشخص کنید که بعد می‌توانید همراه با `break` یا `continue` استفاده کنید تا مشخص کنید که آن کلیدواژه‌ها به‌جای درونی‌ترین loop، روی loopای که آن label را دارد اثر بگذارند. loop labelها باید با یک علامت نقل‌قول تکی شروع شوند. این‌جا یک مثال با دو loop تودرتو (nested) را می‌بینید:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/src/main.rs}}
```

loop بیرونی label `'counting_up` را دارد، و از ۰ تا ۲ می‌شمرد. loop درونی، که هیچ labelای ندارد، از ۱۰ تا ۹ می‌شمرد به‌سمت پایین. اولین `break` که هیچ labelای مشخص نمی‌کند، فقط از loop درونی خارج می‌شود. عبارت `break 'counting_up;` از loop بیرونی خارج می‌شود. این کد این را چاپ می‌کند:

```console
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/output.txt}}
```

<!-- Old headings. Do not remove or links may break. -->
<a id="conditional-loops-with-while"></a>

#### ساده‌تر کردن Loopهای شرطی با while

یک برنامه معمولاً باید یک شرط را داخل یک loop ارزیابی کند. تا وقتی شرط `true` است، loop اجرا می‌شود. وقتی شرط دیگر `true` نباشد، برنامه `break` را صدا می‌زند و loop را متوقف می‌کند. می‌شود همین رفتار را با استفاده از ترکیبی از `loop`، `if`، `else` و `break` پیاده‌سازی کرد؛ اگر دوست دارید می‌توانید همین الان این را در یک برنامه امتحان کنید. با این حال، این الگو آن‌قدر رایج است که Rust یک ساختار زبانی از‌پیش‌ساخته برایش دارد، به نام یک loop `while`. در Listing 3-3، از `while` استفاده می‌کنیم تا برنامه را سه بار loop کنیم، هر بار به‌سمت پایین شمارش کنیم، و بعد، بعد از loop، یک پیام چاپ کنیم و خارج شویم.

<Listing number="3-3" file-name="src/main.rs" caption="استفاده از یک loop `while` برای اجرای کد تا وقتی که یک شرط به `true` محاسبه شود">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-03/src/main.rs}}
```

</Listing>

این ساختار خیلی از تودرتویی (nesting)ای که لازم می‌شد اگر از `loop`، `if`، `else` و `break` استفاده می‌کردید را حذف می‌کند، و واضح‌تر است. تا وقتی یک شرط به `true` محاسبه می‌شود، کد اجرا می‌شود؛ در غیر این صورت، از loop خارج می‌شود.

#### Loop زدن روی یک Collection با `for`

همچنین می‌توانید از ساختار `while` استفاده کنید تا روی المان‌های یک collection، مثل یک آرایه، loop بزنید. برای مثال، loop در Listing 3-4 هر المان در آرایه‌ی `a` را چاپ می‌کند.

<Listing number="3-4" file-name="src/main.rs" caption="Loop زدن روی هر المان یک collection با استفاده از یک loop `while`">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-04/src/main.rs}}
```

</Listing>

این‌جا، کد از المان‌های آرایه به‌سمت بالا می‌شمرد. از ایندکس `0` شروع می‌شود و بعد loop می‌زند تا به آخرین ایندکس در آرایه برسد (یعنی، وقتی `index < 5` دیگر `true` نباشد). اجرای این کد هر المان در آرایه را چاپ می‌کند:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-04/output.txt}}
```

هر پنج مقدار آرایه، همان‌طور که انتظارش را داشتیم، در ترمینال ظاهر می‌شوند. با این‌که `index` در یک نقطه به مقدار `5` می‌رسد، loop قبل از این‌که سعی کند ششمین مقدار را از آرایه بگیرد، متوقف می‌شود.

با این حال، این روش مستعد خطاست؛ اگر مقدار ایندکس یا شرط تست نادرست باشد، می‌توانیم باعث شویم برنامه panic کند. برای مثال، اگر تعریف آرایه‌ی `a` را به چهار المان تغییر می‌دادید اما فراموش می‌کردید شرط را به `while index < 4` به‌روزرسانی کنید، این کد panic می‌کرد. همچنین این روش کند است، چون کامپایلر کدی در زمان اجرا اضافه می‌کند تا در هر تکرار از loop، بررسی شرطی این‌که آیا ایندکس در بازه‌ی آرایه است یا نه را انجام دهد.

به‌عنوان یک روش خلاصه‌تر، می‌توانید از یک loop `for` استفاده کنید و برای هر آیتم در یک collection، مقداری کد اجرا کنید. یک loop `for` شبیه کد در Listing 3-5 است.

<Listing number="3-5" file-name="src/main.rs" caption="Loop زدن روی هر المان یک collection با استفاده از یک loop `for`">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-05/src/main.rs}}
```

</Listing>

وقتی این کد را اجرا می‌کنیم، همان خروجی Listing 3-4 را می‌بینیم. مهم‌تر از آن، حالا امنیت کد را بیشتر کرده‌ایم و شانس باگ‌هایی که ممکن است از رفتن به بعد از پایان آرایه یا نرفتن به‌اندازه‌ی کافی و از دست دادن چند آیتم ناشی شود را از بین برده‌ایم. کد ماشینی که از loopهای `for` تولید می‌شود هم می‌تواند کارآمدتر باشد، چون در هر تکرار نیازی نیست ایندکس با طول آرایه مقایسه شود.

با استفاده از loop `for`، اگر تعداد مقادیر داخل آرایه را تغییر می‌دادید، لازم نبود یادتان بماند که کد دیگری را تغییر دهید، برخلاف روشی که در Listing 3-4 استفاده شد.

امنیت و خلاصگی loopهای `for` باعث می‌شود که رایج‌ترین ساختار loop در Rust باشند. حتی در وضعیت‌هایی که می‌خواهید مقداری کد را یک تعداد مشخص بار اجرا کنید، مثل مثال شمارش معکوس که در Listing 3-3 از یک loop `while` استفاده کرد، بیشتر Rustaceanها از یک loop `for` استفاده می‌کنند. راه انجام آن این است که از یک `Range` استفاده کنید، که کتابخانه‌ی استاندارد فراهم کرده، و همه‌ی اعداد را به‌ترتیب، شروع از یک عدد و پایان قبل از عدد دیگری، تولید می‌کند.

این‌جا شمارش معکوس با استفاده از یک loop `for` و یک متد دیگر که هنوز درباره‌اش صحبت نکرده‌ایم، `rev`، برای برعکس کردن range، این‌طور خواهد بود:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-34-for-range/src/main.rs}}
```

این کد کمی زیباتر است، نه؟

{{#quiz ../quizzes/ch03-05-control-flow-sec2-loops.toml}}

## جمع‌بندی

موفق شدید! این یک فصل بزرگ بود: درباره‌ی متغیرها، تایپ‌های داده‌ی scalar و compound، توابع، کامنت‌ها، عبارت‌های `if` و loopها یاد گرفتید! برای تمرین کردن با مفاهیمی که در این فصل بررسی شدند، سعی کنید برنامه‌هایی بسازید که این کارها را انجام دهند:

- دما را بین فارنهایت و سلسیوس تبدیل کنید.
- اُمین عدد فیبوناچی را تولید کنید.
- شعر آهنگ کریسمس «دوازده روز کریسمس» را چاپ کنید، با استفاده از تکراری که در این آهنگ وجود دارد.

وقتی آماده‌ی ادامه‌ی مسیر بودید، درباره‌ی مفهومی در Rust صحبت می‌کنیم که معمولاً در زبان‌های برنامه‌نویسی دیگر وجود _ندارد_: ownership.

[comparing-the-guess-to-the-secret-number]: ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[quitting-after-a-correct-guess]: ch02-00-guessing-game-tutorial.html#quitting-after-a-correct-guess
