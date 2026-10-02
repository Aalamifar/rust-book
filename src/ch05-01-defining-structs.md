## تعریف و نمونه‌سازیِ ساختارها

ساختارها شبیهِ تاپل‌ها هستند، که در بخشِ [«نوعِ تاپل»][tuples]<!-- ignore --> درباره‌شان بحث کردیم، از این جهت که هر دو چندین مقدارِ مرتبط را نگه می‌دارند. مانندِ تاپل‌ها، بخش‌های یک ساختار می‌توانند نوع‌هایِ متفاوتی داشته باشند. برخلافِ تاپل‌ها، در یک ساختار هر بخشِ داده را نام‌گذاری می‌کنید، پس روشن است که مقادیر چه معنایی دارند. اضافه کردنِ این نام‌ها یعنی ساختارها انعطاف‌پذیرتر از تاپل‌ها هستند: لازم نیست برایِ مشخص کردن یا دسترسی به مقادیرِ یک نمونه، به ترتیبِ داده‌ها تکیه کنید.

برای تعریفِ یک ساختار، کلیدواژه‌ی `struct` را می‌نویسیم و به کلِ ساختار نام می‌دهیم. نامِ یک ساختار باید اهمیتِ بخش‌هایِ داده‌ای که در کنارِ هم گروه‌بندی شده‌اند را توصیف کند. سپس، درونِ آکولاد، نام‌ها و نوع‌هایِ بخش‌هایِ داده، که آن‌ها را _فیلد (field)_ می‌نامیم، تعریف می‌کنیم. برای مثال، فهرستِ ۵-۱ ساختاری را نشان می‌دهد که اطلاعاتِ مربوط به یک حسابِ کاربری را ذخیره می‌کند.

<Listing number="5-1" file-name="src/main.rs" caption="A `User` struct definition">

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-01/src/main.rs:here}}
```

</Listing>

برایِ استفاده از یک ساختار بعد از تعریفش، یک _نمونه (instance)_ از آن ساختار می‌سازیم، با مشخص کردنِ مقادیرِ عینی برایِ هر یک از فیلدها. یک نمونه را با نوشتنِ نامِ ساختار و سپس افزودنِ آکولادی حاویِ جفت‌های _`key: value`_ می‌سازیم، که در آن کلیدها نامِ فیلدها هستند و مقادیر، داده‌هایی هستند که می‌خواهیم در آن فیلدها ذخیره کنیم. لازم نیست فیلدها را به همان ترتیبی که در ساختار اعلام کرده‌ایم، مشخص کنیم. به بیانِ دیگر، تعریفِ ساختار مانندِ یک الگویِ کلی برایِ آن نوع است، و نمونه‌ها آن الگو را با داده‌ای خاص پر می‌کنند تا مقادیری از آن نوع بسازند. برای مثال، می‌توانیم یک کاربرِ خاص را همان‌طور که در فهرستِ ۵-۲ نشان داده شده، اعلام کنیم.

```aquascope,interpreter
#struct User {
#    active: bool,
#    username: String,
#    email: String,
#    sign_in_count: u64,
#}
fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };`[]`
}
```

برایِ گرفتنِ مقداری خاص از یک ساختار، از نحوِ نقطه استفاده می‌کنیم. برای مثال، برایِ دسترسی به ایمیلِ این کاربر، از `user1.email` استفاده می‌کنیم. اگر نمونه تغییرپذیر باشد، می‌توانیم با استفاده از نحوِ نقطه و انتساب به یک فیلدِ خاص، مقداری را تغییر دهیم. فهرستِ ۵-۳ نشان می‌دهد چگونه مقدارِ فیلدِ `email` را در یک نمونه‌ی تغییرپذیرِ `User` تغییر دهیم.

```aquascope,interpreter
#struct User {
#    active: bool,
#    username: String,
#    email: String,
#    sign_in_count: u64,
#}
fn main() {
    let mut user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };`[]`

    user1.email = String::from("anotheremail@example.com");`[]`
}
```

توجه کنید که کلِ نمونه باید تغییرپذیر باشد؛ راست به ما اجازه نمی‌دهد فقط برخی فیلدها را به‌عنوانِ تغییرپذیر مشخص کنیم. مانندِ هر عبارتِ دیگری، می‌توانیم یک نمونه‌ی جدیدِ ساختار را به‌عنوانِ آخرین عبارتِ بدنه‌ی تابع بسازیم تا آن نمونه‌ی جدید را به‌طورِ ضمنی برگردانیم.

فهرستِ ۵-۴ تابعی به نامِ `build_user` را نشان می‌دهد که نمونه‌ای از `User` را با ایمیل و نامِ کاربریِ داده‌شده برمی‌گرداند. فیلدِ `active` مقدارِ `true` می‌گیرد، و `sign_in_count` مقدارِ `1` می‌گیرد.

<Listing number="5-4" file-name="src/main.rs" caption="A `build_user` function that takes an email and username and returns a `User` instance">

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-04/src/main.rs:here}}
```

</Listing>

منطقی است که پارامترهایِ تابع را با همان نامِ فیلدهایِ ساختار نام‌گذاری کنیم، اما تکرار کردنِ نام‌های فیلدهایِ `email` و `username` و متغیرها کمی خسته‌کننده است. اگر ساختار فیلدهایِ بیشتری داشت، تکرارِ هر نام حتی آزاردهنده‌تر هم می‌شد. خوشبختانه، یک میان‌بُرِ راحت وجود دارد!

<!-- Old headings. Do not remove or links may break. -->

<a id="using-the-field-init-shorthand-when-variables-and-fields-have-the-same-name"></a>

### استفاده از میان‌بُرِ مقداردهی‌اولیه‌ی فیلد

چون نامِ پارامترها و نامِ فیلدهایِ ساختار در فهرستِ ۵-۴ دقیقاً یکسان‌اند، می‌توانیم از نحوِ _میان‌بُرِ مقداردهی‌اولیه‌ی فیلد (field init shorthand)_ استفاده کنیم تا `build_user` را بازنویسی کنیم، به‌طوری‌که دقیقاً همان رفتار را داشته باشد اما تکرارِ `username` و `email` را نداشته باشد، همان‌طور که در فهرستِ ۵-۵ نشان داده شده است.

<Listing number="5-5" file-name="src/main.rs" caption="A `build_user` function that uses field init shorthand because the `username` and `email` parameters have the same name as struct fields">

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-05/src/main.rs:here}}
```

</Listing>

در اینجا، نمونه‌ای جدید از ساختارِ `User` می‌سازیم، که فیلدی به نامِ `email` دارد. می‌خواهیم مقدارِ فیلدِ `email` را برابرِ مقدارِ درونِ پارامترِ `email` تابعِ `build_user` قرار دهیم. چون فیلدِ `email` و پارامترِ `email` هم‌نام هستند، فقط لازم است `email` بنویسیم، به‌جایِ `email: email`.

<!-- Old headings. Do not remove or links may break. -->

<a id="creating-instances-from-other-instances-with-struct-update-syntax"></a>

### ساختنِ نمونه‌ها با نحوِ به‌روزرسانیِ ساختار

اغلب مفید است نمونه‌ای جدید از یک ساختار بسازیم که بیشترِ مقادیرِ خود را از نمونه‌ی دیگری از همان نوع می‌گیرد، اما بعضی از آن‌ها را تغییر می‌دهد. می‌توانید این کار را با استفاده از نحوِ به‌روزرسانیِ ساختار (struct update syntax) انجام دهید.

اول، در فهرستِ ۵-۶ نشان می‌دهیم چگونه یک نمونه‌ی جدیدِ `User` را در `user2` به روشِ معمول، بدونِ نحوِ به‌روزرسانی، بسازیم. مقداری جدید برایِ `email` تنظیم می‌کنیم اما در باقیِ موارد از همان مقادیرِ `user1` که در فهرستِ ۵-۲ ساختیم، استفاده می‌کنیم.

```aquascope,interpreter
#struct User {
#    active: bool,
#    username: String,
#    email: String,
#    sign_in_count: u64,
#}
fn main() {
#   let user1 = User {
#      email: String::from("someone@example.com"),
#      username: String::from("someusername123"),
#      active: true,
#      sign_in_count: 1,
#   };
    // --snip--

    let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    };`[]`
}
```

<span class="caption">فهرستِ ۵-۶: ساختنِ نمونه‌ای جدیدِ `User` با استفاده از همه‌ی مقادیرِ `user1` به‌جز یکی</span>

با استفاده از نحوِ به‌روزرسانیِ ساختار، می‌توانیم همان اثر را با کدِ کمتری به دست بیاوریم، همان‌طور که در فهرستِ ۵-۷ نشان داده شده است. نحوِ `..` مشخص می‌کند که فیلدهایِ باقی‌مانده‌ای که به‌طورِ صریح تنظیم نشده‌اند، باید همان مقدارِ فیلدهایِ نمونه‌ی داده‌شده را داشته باشند.

<Listing number="5-7" file-name="src/main.rs" caption="Using struct update syntax to set a new `email` value for a `User` instance but to use the rest of the values from `user1`">

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-07/src/main.rs:here}}
```

</Listing>

کدِ فهرستِ ۵-۷ هم نمونه‌ای در `user2` می‌سازد که مقداری متفاوت برایِ `email` دارد اما مقادیرِ فیلدهایِ `username`، `active` و `sign_in_count` را همان مقادیرِ `user1` نگه می‌دارد. `..user1` باید در آخر بیاید تا مشخص کند هر فیلدِ باقی‌مانده باید مقدارش را از فیلدِ متناظرش در `user1` بگیرد، اما می‌توانیم برایِ هر تعداد فیلد که بخواهیم، به هر ترتیبی، مقدار مشخص کنیم، فارغ از ترتیبِ فیلدها در تعریفِ ساختار.

توجه کنید که نحوِ به‌روزرسانیِ ساختار از `=` مانندِ یک انتساب استفاده می‌کند؛ به این دلیل که داده را جابه‌جا می‌کند، دقیقاً همان‌طور که در بخشِ [«مالکیت چیست؟»][move]<!-- ignore --> دیدیم. در این مثال، بعد از ساختِ `user2`، `user1` تا حدی نامعتبر می‌شود چون `String` درونِ فیلدِ `username` در `user1` به `user2` منتقل شده است. اگر برایِ هر دویِ `email` و `username` در `user2`، مقادیرِ `String` جدیدی داده بودیم، و در نتیجه فقط از مقادیرِ `active` و `sign_in_count` در `user1` استفاده می‌کردیم، آن‌گاه `user1` بعد از ساختِ `user2` همچنان کاملاً معتبر می‌ماند. نوع‌هایِ `active` و `sign_in_count` نوع‌هایی هستند که traitِ `Copy` را پیاده‌سازی می‌کنند، پس رفتاری که در بخشِ [«کپی کردن در برابرِ خارج کردن از یک مجموعه»][copy]<!-- ignore --> بحث کردیم، در اینجا هم صدق می‌کند.

<!-- Old headings. Do not remove or links may break. -->

<a id="using-tuple-structs-without-named-fields-to-create-different-types"></a>

### ساختنِ نوع‌هایِ متفاوت با استراکت‌های تاپلی

راست همچنین از ساختارهایی پشتیبانی می‌کند که شبیهِ تاپل‌ها هستند، به نامِ _استراکت‌های تاپلی (tuple structs)_. استراکت‌های تاپلی معنایِ اضافه‌ای دارند که نامِ ساختار فراهم می‌کند، اما نامی مرتبط با فیلدهایشان ندارند؛ در عوض، فقط نوعِ فیلدها را دارند. استراکت‌های تاپلی وقتی مفیدند که بخواهید به کلِ تاپل نامی بدهید و آن تاپل را از تاپل‌های دیگر به نوعی متفاوت تبدیل کنید، و وقتی نام‌گذاریِ هر فیلد مانندِ یک استراکتِ معمولی، پرحرف یا زائد باشد.

برای تعریفِ یک استراکتِ تاپلی، با کلیدواژه‌ی `struct` و نامِ ساختار شروع می‌کنیم و بعد نوع‌هایِ درونِ تاپل می‌آیند. برای مثال، در اینجا دو استراکتِ تاپلی به نام‌هایِ `Color` و `Point` تعریف و استفاده می‌کنیم:

<Listing file-name="src/main.rs">

```aquascope,interpreter
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);`[]`
}
```

</Listing>

توجه کنید که مقادیرِ `black` و `origin` نوع‌های متفاوتی هستند چون نمونه‌هایِ استراکت‌های تاپلیِ متفاوتی‌اند. هر ساختاری که تعریف می‌کنید، نوعِ خودش را دارد، حتی اگر فیلدهایِ درونِ آن ساختار نوع‌هایِ یکسانی داشته باشند. برای مثال، تابعی که پارامتری از نوعِ `Color` می‌گیرد، نمی‌تواند یک `Point` را به‌عنوانِ آرگومان بگیرد، حتی اگر هر دو نوع از سه مقدارِ `i32` تشکیل شده باشند. در غیرِ این صورت، نمونه‌های استراکتِ تاپلی شبیهِ تاپل‌ها هستند، از این جهت که می‌توانید آن‌ها را به بخش‌های منفردشان تجزیه کنید، و می‌توانید از یک `.` به‌همراهِ اندیس برایِ دسترسی به یک مقدارِ منفرد استفاده کنید. برخلافِ تاپل‌ها، استراکت‌های تاپلی نیاز دارند وقتی آن‌ها را تجزیه می‌کنید، نوعِ ساختار را نام ببرید. برای مثال، برایِ تجزیه‌ی مقادیرِ درونِ نقطه‌ی `origin` به متغیرهایی به نامِ `x`، `y` و `z`، می‌نویسیم `let Point(x, y, z) = origin;`.

<!-- Old headings. Do not remove or links may break. -->

<a id="unit-like-structs-without-any-fields"></a>

### تعریفِ استراکت‌هایِ شبه‌واحد

همچنین می‌توانید ساختارهایی تعریف کنید که هیچ فیلدی ندارند! این‌ها را _استراکت‌های شبه‌واحد (unit-like structs)_ می‌نامند چون رفتاری شبیهِ `()`، نوعِ واحدی که در بخشِ [«نوعِ تاپل»][tuples]<!-- ignore --> به آن اشاره کردیم، دارند. استراکت‌های شبه‌واحد وقتی مفیدند که بخواهید یک trait را روی نوعی پیاده‌سازی کنید اما هیچ داده‌ای نداشته باشید که بخواهید در خودِ آن نوع ذخیره کنید. درباره‌ی traitها در فصلِ ۱۰ بحث خواهیم کرد. در ادامه مثالی از اعلام و نمونه‌سازیِ یک استراکتِ واحد به نامِ `AlwaysEqual` می‌آید:

```aquascope,interpreter
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;`[]`
}
```

برایِ تعریفِ `AlwaysEqual`، از کلیدواژه‌ی `struct`، نامی که می‌خواهیم و سپس یک نقطه‌ویرگول استفاده می‌کنیم. نیازی به آکولاد یا پرانتز نیست! سپس، می‌توانیم نمونه‌ای از `AlwaysEqual` را در متغیرِ `subject` به روشی مشابه بگیریم: با استفاده از نامی که تعریف کردیم، بدونِ هیچ آکولاد یا پرانتزی. تصور کنید بعدها رفتاری برایِ این نوع پیاده‌سازی می‌کنیم که هر نمونه‌ای از `AlwaysEqual` همیشه با هر نمونه‌ای از هر نوعِ دیگری برابر باشد، شاید برایِ داشتنِ نتیجه‌ای شناخته‌شده برایِ هدفِ آزمون. برایِ پیاده‌سازیِ آن رفتار، نیازی به هیچ داده‌ای نداشتیم! در فصلِ ۱۰ خواهید دید چگونه traitها را تعریف و روی هر نوعی، شاملِ استراکت‌های شبه‌واحد، پیاده‌سازی کنید.

> ### مالکیتِ داده‌های ساختار
>
> در تعریفِ ساختارِ `User` در فهرستِ ۵-۱، به‌جایِ نوعِ اسلایسِ رشته‌ایِ `&str`، از نوعِ مالکیت‌دارِ `String` استفاده کردیم. این انتخابی عمدی است چون می‌خواهیم هر نمونه از این ساختار مالکِ همه‌ی داده‌اش باشد و آن داده تا زمانی‌که کلِ ساختار معتبر است، معتبر باشد.
>
> همچنین ممکن است ساختارها ارجاع‌هایی به داده‌ای که چیزِ دیگری مالکِ آن است را ذخیره کنند، اما این کار به استفاده از _طولِ‌عمرها (lifetimes)_، ویژگی‌ای در راست که در فصلِ ۱۰ بحث خواهیم کرد، نیاز دارد. طولِ‌عمرها تضمین می‌کنند داده‌ای که یک ساختار به آن ارجاع می‌دهد، تا زمانی‌که خودِ ساختار معتبر است، معتبر بماند. فرض کنید سعی می‌کنید ارجاعی را در یک ساختار بدونِ مشخص کردنِ طولِ‌عمرها ذخیره کنید، مانندِ این در *src/main.rs*؛ این کار نتیجه نمی‌دهد:
>
> <Listing file-name="src/main.rs">
>
> <!-- CAN'T EXTRACT SEE https://github.com/rust-lang/mdBook/issues/1127 -->
>
> ```rust,ignore,does_not_compile
> struct User {
>     active: bool,
>     username: &str,
>     email: &str,
>     sign_in_count: u64,
> }
>
> fn main() {
>     let user1 = User {
>         active: true,
>         username: "someusername123",
>         email: "someone@example.com",
>         sign_in_count: 1,
>     };
> }
> ```
>
> </Listing>
>
> کامپایلر شکایت می‌کند که به مشخص‌کننده‌های طولِ‌عمر نیاز دارد:
>
> ```console
> $ cargo run
>    Compiling structs v0.1.0 (file:///projects/structs)
> error[E0106]: missing lifetime specifier
>  --> src/main.rs:3:15
>   |
> 3 |     username: &str,
>   |               ^ expected named lifetime parameter
>   |
> help: consider introducing a named lifetime parameter
>   |
> 1 ~ struct User<'a> {
> 2 |     active: bool,
> 3 ~     username: &'a str,
>   |
>
> error[E0106]: missing lifetime specifier
>  --> src/main.rs:4:12
>   |
> 4 |     email: &str,
>   |            ^ expected named lifetime parameter
>   |
> help: consider introducing a named lifetime parameter
>   |
> 1 ~ struct User<'a> {
> 2 |     active: bool,
> 3 |     username: &str,
> 4 ~     email: &'a str,
>   |
>
> For more information about this error, try `rustc --explain E0106`.
> error: could not compile `structs` (bin "structs") due to 2 previous errors
> ```
>
> در فصلِ ۱۰ بحث خواهیم کرد چگونه این خطاها را رفع کنیم تا بتوانید ارجاع‌ها را در ساختارها ذخیره کنید، اما فعلاً، خطاهایی مانندِ این را با استفاده از نوع‌هایِ مالکیت‌دار مانندِ `String` به‌جایِ ارجاع‌هایی مانندِ `&str` رفع می‌کنیم.

### قرض گرفتنِ فیلدهایِ یک ساختار

مشابهِ بحثی که در [«فیلدهایِ مختلفِ تاپل»][differentfields] داشتیم، بررسی‌کننده‌ی قرضِ راست، اجازه‌های مالکیت را هم در سطحِ ساختار و هم در سطحِ فیلد ردیابی می‌کند. برای مثال، اگر فیلدِ `x` از یک ساختارِ `Point` را قرض بگیریم، آن‌گاه هم `p` و هم `p.x` به‌طورِ موقت اجازه‌هایشان را از دست می‌دهند (اما `p.y` این‌طور نیست):

```aquascope,permissions,stepper,boundaries
#fn main() {
struct Point { x: i32, y: i32 }

let mut p = Point { x: 0, y: 0 };`(focus,paths:p)`
let x = &mut p.x;`(focus,paths:p)`
*x += 1;`(focus,paths:p)`
println!("{}, {}", p.x, p.y);
#}
```

در نتیجه، اگر سعی کنیم درحالی‌که `p.x` به‌طورِ تغییرپذیر قرض داده شده، از `p` استفاده کنیم، مثلِ این:

```aquascope,permissions,stepper,boundaries,shouldFail
struct Point { x: i32, y: i32 }

fn print_point(p: &Point) {
    println!("{}, {}", p.x, p.y);
}

fn main() {
    let mut p = Point { x: 0, y: 0 };`(focus,paths:p)`
    let x = &mut p.x;`(focus,paths:p)`
    print_point(&p);`{}`
    *x += 1;`(focus,paths:p)`
}
```

آن‌گاه کامپایلر برنامه‌ی ما را با این خطا رد می‌کند:

```text
error[E0502]: cannot borrow `p` as immutable because it is also borrowed as mutable
  --> test.rs:10:17
   |
9  |     let x = &mut p.x;
   |             -------- mutable borrow occurs here
10 |     print_point(&p);
   |                 ^^ immutable borrow occurs here
11 |     *x += 1;
   |     ------- mutable borrow later used here
```

به‌طورِ کلی‌تر، اگر با خطای مالکیتی روبه‌رو شدید که یک ساختار در آن دخیل است، باید در نظر بگیرید کدام فیلدهایِ ساختارتان قرار است با چه اجازه‌هایی قرض داده شوند. اما مراقبِ محدودیت‌هایِ بررسی‌کننده‌ی قرض باشید، چون راست گاهی ممکن است فرض کند بیش از آنچه واقعاً قرض داده شده، فیلد قرض داده شده است.

{{#quiz ../quizzes/ch05-01-structs.toml}}


<!-- manual-regeneration
for the error above
after running update-rustc.sh:
pbcopy < listings/ch05-using-structs-to-structure-related-data/no-listing-02-reference-in-struct/output.txt
paste above
add `> ` before every line -->

[tuples]: ch03-02-data-types.html#the-tuple-type
[move]: ch04-01-what-is-ownership.html
[copy]: ch04-03-fixing-ownership-errors.html#fixing-an-unsafe-program-copying-vs-moving-out-of-a-collection
[differentfields]: ch04-03-fixing-ownership-errors.html#fixing-a-safe-program-mutating-different-tuple-fields
