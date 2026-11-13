<!-- Old headings. Do not remove or links may break. -->

<a id="treating-smart-pointers-like-regular-references-with-the-deref-trait"></a>
<a id="treating-smart-pointers-like-regular-references-with-deref"></a>

## رفتار Smart Pointerها مانند Referenceهای معمولی

پیاده‌سازی Trait به نام `Deref` به شما امکان می‌دهد رفتار **Dereference Operator** یعنی `*` را سفارشی کنید (آن را با عملگر ضرب یا Glob اشتباه نگیرید). اگر Trait به نام `Deref` را به‌گونه‌ای پیاده‌سازی کنید که یک Smart Pointer مانند Reference معمولی رفتار کند، می‌توانید کدی بنویسید که با Referenceها کار می‌کند و سپس همان کد را با Smart Pointerها نیز به کار ببرید.

ابتدا بررسی می‌کنیم Dereference Operator با Referenceهای معمولی چگونه کار می‌کند. سپس تلاش می‌کنیم Type سفارشی‌ای تعریف کنیم که مانند `Box<T>` رفتار کند و ببینیم چرا Dereference Operator به‌صورت پیش‌فرض با Type جدید ما مانند یک Reference رفتار نمی‌کند. در ادامه بررسی می‌کنیم پیاده‌سازی Trait به نام `Deref` چگونه باعث می‌شود Smart Pointerها رفتاری شبیه Referenceها داشته باشند. در پایان نیز با قابلیت **Deref Coercion** در Rust آشنا می‌شویم و می‌بینیم چگونه کار با Referenceها و Smart Pointerها را ساده‌تر می‌کند.

<!-- Old headings. Do not remove or links may break. -->

<a id="following-the-pointer-to-the-value-with-the-dereference-operator"></a>
<a id="following-the-pointer-to-the-value"></a>

### دنبال‌کردن Reference تا رسیدن به مقدار

یک Reference معمولی نوعی Pointer است و می‌توان Pointer را مانند پیکانی در نظر گرفت که به مقداری در محل دیگری از حافظه اشاره می‌کند. در Listing 15-6 یک Reference به مقدار `i32` ایجاد می‌کنیم و سپس با استفاده از Dereference Operator، Reference را دنبال می‌کنیم تا به مقدار مورد اشاره برسیم.

<Listing number="15-6" file-name="src/main.rs" caption="استفاده از Dereference Operator برای دنبال‌کردن یک Reference تا مقدار `i32` مورد اشاره">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-06/src/main.rs}}
```

</Listing>

متغیر `x` مقدار `5` از نوع `i32` را نگه می‌دارد. متغیر `y` را برابر با Referenceای به `x` قرار می‌دهیم. می‌توانیم بررسی کنیم که مقدار `x` برابر با `5` است. اما اگر بخواهیم دربارهٔ مقدار داخل `y` یک Assertion بنویسیم، باید از `*y` استفاده کنیم تا Reference را دنبال کرده و به مقداری برسیم که به آن اشاره می‌کند؛ به این عمل **Dereference** می‌گویند. به این ترتیب Compiler می‌تواند مقدار واقعی را مقایسه کند. پس از Dereference کردن `y`، به مقدار عددی‌ای دسترسی داریم که `y` به آن اشاره می‌کند و می‌توانیم آن را با `5` مقایسه کنیم.

اگر به‌جای آن تلاش کنیم `assert_eq!(5, y);` را بنویسیم، با خطای Compile زیر روبه‌رو می‌شویم:

```console
{{#include ../listings/ch15-smart-pointers/output-only-01-comparing-to-reference/output.txt}}
```

مقایسهٔ یک عدد با Referenceای به یک عدد مجاز نیست؛ زیرا این دو Type متفاوتی دارند. باید از Dereference Operator استفاده کنیم تا Reference را دنبال کرده و به مقداری برسیم که به آن اشاره می‌کند.

### استفاده از `Box<T>` مانند Reference

می‌توانیم کد Listing 15-6 را به‌گونه‌ای بازنویسی کنیم که به‌جای Reference از `Box<T>` استفاده کند. Dereference Operator روی `Box<T>` در Listing 15-7 دقیقاً مانند Dereference Operator روی Reference در Listing 15-6 عمل می‌کند.

<Listing number="15-7" file-name="src/main.rs" caption="استفاده از Dereference Operator روی یک `Box<i32>`">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-07/src/main.rs}}
```

</Listing>

تفاوت اصلی Listing 15-7 با Listing 15-6 این است که در اینجا `y` را نمونه‌ای از یک Box قرار می‌دهیم که به نسخهٔ Copyشده‌ای از مقدار `x` اشاره می‌کند؛ نه Referenceای که مستقیماً به مقدار `x` اشاره داشته باشد. در Assertion آخر می‌توانیم از Dereference Operator استفاده کنیم تا Pointer داخل Box را دنبال کنیم؛ درست همان‌طور که وقتی `y` یک Reference بود این کار را انجام دادیم. در ادامه با تعریف Type مخصوص خودمان برای Box بررسی می‌کنیم چه ویژگی‌ای در `Box<T>` باعث می‌شود بتوانیم از Dereference Operator استفاده کنیم.

### تعریف Smart Pointer سفارشی

برای درک تفاوت رفتار پیش‌فرض Smart Pointerها با Referenceها، یک Wrapper Type مشابه `Box<T>` موجود در Standard Library می‌سازیم. سپس بررسی می‌کنیم چگونه قابلیت استفاده از Dereference Operator را به آن اضافه کنیم.

> نکته: میان Type به نام `MyBox<T>` که قرار است بسازیم و `Box<T>` واقعی یک تفاوت مهم وجود دارد: نسخهٔ ما داده را روی Heap ذخیره نمی‌کند. در این مثال روی `Deref` تمرکز داریم؛ بنابراین محل واقعی ذخیره‌سازی داده در مقایسه با رفتار Pointerمانند آن اهمیت کمتری دارد.

Type به نام `Box<T>` در نهایت به‌صورت یک Tuple Struct با یک عضو تعریف شده است؛ بنابراین در Listing 15-8 نیز Type به نام `MyBox<T>` را به همین شکل تعریف می‌کنیم. همچنین تابع `new` را تعریف می‌کنیم تا با تابع `new` موجود در `Box<T>` مطابقت داشته باشد.

<Listing number="15-8" file-name="src/main.rs" caption="تعریف Type به نام `MyBox<T>`">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-08/src/main.rs:here}}
```

</Listing>

یک Struct به نام `MyBox` تعریف می‌کنیم و پارامتر Generic به نام `T` را مشخص می‌کنیم؛ زیرا می‌خواهیم Type ما بتواند مقدارهایی از هر Typeای را نگه دارد. Type به نام `MyBox` یک Tuple Struct با یک عضو از نوع `T` است. تابع `MyBox::new` یک پارامتر از نوع `T` دریافت می‌کند و یک نمونه از `MyBox` برمی‌گرداند که مقدار ورودی را در خود نگه می‌دارد.

بیایید تابع `main` در Listing 15-7 را به Listing 15-8 اضافه کنیم و آن را تغییر دهیم تا به‌جای `Box<T>` از Typeای که تعریف کرده‌ایم، یعنی `MyBox<T>`، استفاده کند. کد Listing 15-9 Compile نمی‌شود؛ زیرا Rust نمی‌داند چگونه `MyBox` را Dereference کند.

<Listing number="15-9" file-name="src/main.rs" caption="تلاش برای استفاده از `MyBox<T>` مانند Referenceها و `Box<T>`">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-09/src/main.rs:here}}
```

</Listing>

خطای Compile حاصل به این شکل است:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-09/output.txt}}
```

نمی‌توان Type به نام `MyBox<T>` را Dereference کرد؛ زیرا هنوز این قابلیت را برای Type خود پیاده‌سازی نکرده‌ایم. برای فعال‌کردن Dereference با عملگر `*`، باید Trait به نام `Deref` را پیاده‌سازی کنیم.

<!-- Old headings. Do not remove or links may break. -->

<a id="treating-a-type-like-a-reference-by-implementing-the-deref-trait"></a>

### پیاده‌سازی Trait به نام `Deref`

همان‌طور که در بخش [«پیاده‌سازی یک Trait برای یک Type»][impl-trait]<!-- ignore --> در فصل ۱۰ گفتیم، برای پیاده‌سازی یک Trait باید پیاده‌سازی متدهای الزامی آن را ارائه کنیم. Trait به نام `Deref` که در Standard Library تعریف شده، از ما می‌خواهد متدی به نام `deref` پیاده‌سازی کنیم. این متد `self` را Borrow می‌کند و Referenceای به دادهٔ داخلی برمی‌گرداند. در Listing 15-10 پیاده‌سازی `Deref` را می‌بینید که باید به تعریف `MyBox<T>` اضافه کنیم.

<Listing number="15-10" file-name="src/main.rs" caption="پیاده‌سازی Trait به نام `Deref` برای `MyBox<T>`">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-10/src/main.rs:here}}
```

</Listing>

Syntax به‌شکل `type Target = T;` یک Associated Type تعریف می‌کند تا Trait به نام `Deref` از آن استفاده کند. Associated Typeها روش متفاوتی برای تعریف یک پارامتر Generic هستند؛ اما فعلاً نیازی نیست نگران آن‌ها باشید. در فصل ۲۰ با جزئیات بیشتری آن‌ها را بررسی خواهیم کرد.

بدنهٔ متد `deref` را با `&self.0` پر می‌کنیم تا Referenceای به مقداری برگرداند که می‌خواهیم با عملگر `*` به آن دسترسی پیدا کنیم. به یاد بیاورید که در بخش [«ایجاد Typeهای متفاوت با Tuple Structها»][tuple-structs]<!-- ignore --> در فصل ۵ گفتیم `.0` به اولین مقدار در یک Tuple Struct دسترسی پیدا می‌کند. اکنون تابع `main` در Listing 15-9 که روی مقدار `MyBox<T>` از عملگر `*` استفاده می‌کند، Compile می‌شود و Assertionها نیز موفق خواهند بود!

بدون Trait به نام `Deref`، Compiler فقط می‌تواند Referenceهای `&` را Dereference کند. متد `deref` این امکان را به Compiler می‌دهد که مقداری از هر Typeای را که Trait به نام `Deref` را پیاده‌سازی کرده است دریافت کند و متد `deref` آن را فراخوانی کند تا Referenceای به دست آورد که می‌داند چگونه آن را Dereference کند.

وقتی در Listing 15-9 عبارت `*y` را نوشتیم، Rust در پشت صحنه در واقع کد زیر را اجرا کرد:

```rust,ignore
*(y.deref())
```

Rust عملگر `*` را با فراخوانی متد `deref` و سپس یک Dereference معمولی جایگزین می‌کند؛ بنابراین لازم نیست خودمان به این فکر کنیم که آیا باید متد `deref` را فراخوانی کنیم یا نه. این قابلیت Rust اجازه می‌دهد کدی بنویسیم که چه با یک Reference معمولی و چه با Typeای که `Deref` را پیاده‌سازی کرده است، به‌شکل یکسان کار کند.

اینکه متد `deref` یک Reference به مقدار برمی‌گرداند و Dereference معمولی بیرون از پرانتز در عبارت `*(y.deref())` همچنان لازم است، به سیستم Ownership مربوط می‌شود. اگر متد `deref` به‌جای Reference، خود مقدار را برمی‌گرداند، مقدار از `self` Move می‌شد. در این مثال و در بیشتر موقعیت‌هایی که از Dereference Operator استفاده می‌کنیم، نمی‌خواهیم مالکیت مقدار داخلی `MyBox<T>` را از آن بگیریم.

توجه کنید که هر بار در کد از عملگر `*` استفاده می‌کنیم، این عملگر فقط یک بار با فراخوانی متد `deref` و سپس یک بار با عملگر `*` جایگزین می‌شود. از آنجا که جایگزینی عملگر `*` به‌صورت بازگشتی و بی‌نهایت ادامه پیدا نمی‌کند، در نهایت به داده‌ای از نوع `i32` می‌رسیم که با مقدار `5` در `assert_eq!` در Listing 15-9 مطابقت دارد.

<!-- Old headings. Do not remove or links may break. -->

<a id="implicit-deref-coercions-with-functions-and-methods"></a>
<a id="using-deref-coercions-in-functions-and-methods"></a>

### استفاده از Deref Coercion در Functionها و Methodها

**Deref Coercion** یک Reference به Typeای که Trait به نام `Deref` را پیاده‌سازی کرده است، به Referenceای از Type دیگر تبدیل می‌کند. برای مثال، Deref Coercion می‌تواند `&String` را به `&str` تبدیل کند؛ زیرا `String`، Trait به نام `Deref` را به‌گونه‌ای پیاده‌سازی کرده است که `&str` برمی‌گرداند. Deref Coercion قابلیتی برای ساده‌ترکردن فراخوانی Functionها و Methodهاست که Rust هنگام ارسال آرگومان‌ها انجام می‌دهد و فقط برای Typeهایی کار می‌کند که Trait به نام `Deref` را پیاده‌سازی کرده باشند. این تبدیل زمانی به‌صورت خودکار انجام می‌شود که Referenceای به مقدار متعلق به یک Type مشخص را به Function یا Methodای ارسال کنیم، اما Type آن با Type پارامتر تعریف‌شده در آن Function یا Method مطابقت نداشته باشد. در این حالت، زنجیره‌ای از فراخوانی‌های متد `deref`، Type ورودی را به Type موردنیاز پارامتر تبدیل می‌کند.

Deref Coercion به Rust اضافه شده است تا برنامه‌نویسان هنگام نوشتن فراخوانی Functionها و Methodها مجبور نباشند تعداد زیادی Reference و Dereference صریح با `&` و `*` اضافه کنند. این قابلیت همچنین کمک می‌کند کد بیشتری بنویسیم که هم با Referenceها و هم با Smart Pointerها کار کند.

برای مشاهدهٔ Deref Coercion در عمل، از Type به نام `MyBox<T>` که در Listing 15-8 تعریف کردیم و پیاده‌سازی `Deref` که در Listing 15-10 به آن اضافه کردیم استفاده می‌کنیم. Listing 15-11 تعریف Functionای را نشان می‌دهد که یک پارامتر از نوع String Slice دارد.

<Listing number="15-11" file-name="src/main.rs" caption="تابع `hello` با پارامتر `name` از نوع `&str`">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-11/src/main.rs:here}}
```

</Listing>

می‌توانیم تابع `hello` را با یک String Slice به‌عنوان آرگومان فراخوانی کنیم؛ برای مثال `hello("Rust");`. Deref Coercion این امکان را فراهم می‌کند که تابع `hello` را با Referenceای به مقداری از نوع `MyBox<String>` نیز فراخوانی کنیم؛ همان‌طور که در Listing 15-12 می‌بینید.

<Listing number="15-12" file-name="src/main.rs" caption="فراخوانی `hello` با Referenceای به مقدار `MyBox<String>` که به‌لطف Deref Coercion امکان‌پذیر است">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-12/src/main.rs:here}}
```

</Listing>

در اینجا تابع `hello` را با آرگومان `&m` فراخوانی می‌کنیم که Referenceای به مقدار `MyBox<String>` است. از آنجا که در Listing 15-10، Trait به نام `Deref` را برای `MyBox<T>` پیاده‌سازی کردیم، Rust می‌تواند با فراخوانی `deref`، مقدار `&MyBox<String>` را به `&String` تبدیل کند. Standard Library پیاده‌سازی‌ای از `Deref` برای `String` ارائه می‌کند که یک String Slice برمی‌گرداند؛ این موضوع در API Documentation مربوط به `Deref` آمده است. سپس Rust دوباره `deref` را فراخوانی می‌کند تا `&String` را به `&str` تبدیل کند؛ Typeای که با تعریف تابع `hello` مطابقت دارد.

اگر Rust قابلیت Deref Coercion را پیاده‌سازی نکرده بود، برای فراخوانی `hello` با مقداری از نوع `&MyBox<String>` باید به‌جای کد Listing 15-12، کد Listing 15-13 را می‌نوشتیم.

<Listing number="15-13" file-name="src/main.rs" caption="کدی که در نبود Deref Coercion در Rust باید می‌نوشتیم">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-13/src/main.rs:here}}
```

</Listing>

عبارت `(*m)` مقدار `MyBox<String>` را به `String` Dereference می‌کند. سپس `&` و `[..]` یک String Slice از `String` می‌سازند که تمام رشته را در بر می‌گیرد تا با Signature تابع `hello` مطابقت داشته باشد. خواندن، نوشتن و درک این کد بدون Deref Coercion به‌دلیل وجود تمام این نمادها دشوارتر است. Deref Coercion اجازه می‌دهد Rust این تبدیل‌ها را به‌صورت خودکار برای ما انجام دهد.

وقتی Trait به نام `Deref` برای Typeهای درگیر تعریف شده باشد، Rust Typeها را بررسی می‌کند و هر چند بار که لازم باشد از `Deref::deref` استفاده می‌کند تا Referenceای با Type موردنیاز پارامتر به دست آورد. تعداد دفعاتی که باید `Deref::deref` درج شود در Compile Time مشخص می‌شود؛ بنابراین استفاده از Deref Coercion هیچ جریمهٔ عملکردی در Runtime ندارد!

<!-- Old headings. Do not remove or links may break. -->

<a id="how-deref-coercion-interacts-with-mutability"></a>

### رفتار Deref Coercion با Mutable Referenceها

همان‌طور که از Trait به نام `Deref` برای بازنویسی رفتار عملگر `*` روی Immutable Referenceها استفاده می‌کنید، می‌توانید از Trait به نام `DerefMut` نیز برای بازنویسی رفتار عملگر `*` روی Mutable Referenceها استفاده کنید.

Rust در سه حالت زیر و با توجه به Typeها و پیاده‌سازی Traitها، Deref Coercion را انجام می‌دهد:

1. از `&T` به `&U`، وقتی `T: Deref<Target=U>` باشد.
2. از `&mut T` به `&mut U`، وقتی `T: DerefMut<Target=U>` باشد.
3. از `&mut T` به `&U`، وقتی `T: Deref<Target=U>` باشد.

دو حالت اول مشابه یکدیگرند و تفاوتشان در Mutable بودن Reference در حالت دوم است. حالت اول می‌گوید اگر `&T` داشته باشید و `T`، Trait به نام `Deref` را برای Typeای به نام `U` پیاده‌سازی کرده باشد، می‌توانید به‌صورت شفاف `&U` به دست آورید. حالت دوم می‌گوید همین Deref Coercion برای Mutable Referenceها نیز انجام می‌شود.

حالت سوم کمی پیچیده‌تر است: Rust یک Mutable Reference را نیز به Immutable Reference تبدیل می‌کند. اما عکس این کار **ممکن نیست**؛ Immutable Referenceها هرگز به Mutable Reference تبدیل نمی‌شوند. طبق قوانین Borrowing، اگر یک Mutable Reference داشته باشید، آن Reference باید تنها Reference به آن داده باشد؛ در غیر این صورت برنامه Compile نخواهد شد. تبدیل یک Mutable Reference به یک Immutable Reference هرگز قوانین Borrowing را نقض نمی‌کند. اما تبدیل یک Immutable Reference به Mutable Reference مستلزم آن است که Reference اولیه تنها Immutable Reference به آن داده باشد؛ درحالی‌که قوانین Borrowing چنین چیزی را تضمین نمی‌کنند. بنابراین Rust نمی‌تواند فرض کند تبدیل یک Immutable Reference به Mutable Reference امکان‌پذیر است.

{{#quiz ../quizzes/ch15-02-deref.toml}}

[impl-trait]: ch10-02-traits.html#implementing-a-trait-on-a-type
[tuple-structs]: ch05-01-defining-structs.html#creating-different-types-with-tuple-structs
