## `RefCell<T>` و الگوی Interior Mutability

**Interior Mutability** یک Design Pattern در Rust است که به شما اجازه می‌دهد حتی زمانی که Referenceهای Immutable به یک داده وجود دارند، آن داده را تغییر دهید؛ کاری که طبق قوانین Borrowing معمولاً مجاز نیست. این الگو برای تغییر داده از کد `unsafe` درون یک Data Structure استفاده می‌کند تا بتواند از قواعد معمول Rust دربارهٔ Mutation و Borrowing عبور کند. کد `unsafe` به Compiler اعلام می‌کند که به‌جای اتکا به بررسی خودکار Compiler، خودمان قوانین را بررسی می‌کنیم. در فصل ۲۰ بیشتر دربارهٔ کد `unsafe` صحبت خواهیم کرد.

فقط زمانی می‌توانیم از Typeهایی استفاده کنیم که الگوی Interior Mutability را به کار می‌گیرند که بتوانیم اطمینان حاصل کنیم قوانین Borrowing در Runtime رعایت می‌شوند؛ حتی اگر Compiler نتواند این موضوع را تضمین کند. سپس کد `unsafe` موردنیاز درون یک Safe API قرار می‌گیرد و Type بیرونی همچنان Immutable باقی می‌ماند.

برای بررسی این مفهوم، Type به نام `RefCell<T>` را مطالعه می‌کنیم که از الگوی Interior Mutability پیروی می‌کند.

<!-- Old headings. Do not remove or links may break. -->

<a id="enforcing-borrowing-rules-at-runtime-with-refcellt"></a>

### اعمال قوانین Borrowing در Runtime

برخلاف `Rc<T>`، Type به نام `RefCell<T>` نشان‌دهندهٔ Single Ownership روی داده‌ای است که در خود نگه می‌دارد. پس چه چیزی `RefCell<T>` را از Typeای مانند `Box<T>` متمایز می‌کند؟ قوانین Borrowing را که در فصل ۴ یاد گرفتید به خاطر بیاورید:

- در هر لحظه می‌توانید **یا** یک Mutable Reference داشته باشید **یا** هر تعداد Immutable Reference؛ اما نمی‌توانید هر دو را هم‌زمان داشته باشید.
- Referenceها باید همیشه معتبر باشند.

در Referenceها و `Box<T>`، Invariantهای مربوط به قوانین Borrowing در Compile Time اعمال می‌شوند. اما در `RefCell<T>` این Invariantها در **Runtime** بررسی می‌شوند. اگر با Referenceها این قوانین را نقض کنید، خطای Compiler دریافت می‌کنید؛ اما اگر با `RefCell<T>` این قوانین را نقض کنید، برنامه Panic می‌کند و از اجرا خارج می‌شود.

مزیت بررسی قوانین Borrowing در Compile Time این است که خطاها زودتر در فرایند توسعه شناسایی می‌شوند و چون تمام تحلیل‌ها پیش از اجرا انجام شده‌اند، هیچ تأثیری بر عملکرد Runtime ندارند. به همین دلایل، در بیشتر موارد بررسی قوانین Borrowing در Compile Time بهترین انتخاب است و به همین خاطر رفتار پیش‌فرض Rust محسوب می‌شود.

در مقابل، مزیت بررسی قوانین Borrowing در Runtime این است که برخی سناریوهای Memory-safe مجاز می‌شوند؛ سناریوهایی که بررسی‌های Compile Time آن‌ها را رد می‌کردند. Static Analysis، مانند کاری که Rust Compiler انجام می‌دهد، ذاتاً محافظه‌کارانه است. تشخیص برخی ویژگی‌های کد از طریق تحلیل آن غیرممکن است. مشهورترین نمونه، **Halting Problem** است که بررسی آن خارج از محدودهٔ این کتاب است، اما موضوع جالبی برای مطالعه به شمار می‌رود.

از آنجا که انجام برخی تحلیل‌ها غیرممکن است، اگر Rust نتواند مطمئن شود کد با قوانین Ownership مطابقت دارد، ممکن است یک برنامهٔ صحیح را نیز رد کند؛ این همان رویکرد محافظه‌کارانه است. اگر Rust یک برنامهٔ نادرست را بپذیرد، کاربران دیگر نمی‌توانند به تضمین‌هایی که Rust ارائه می‌کند اعتماد کنند. اما اگر Rust یک برنامهٔ صحیح را رد کند، برنامه‌نویس با زحمت و محدودیت روبه‌رو می‌شود، ولی اتفاق فاجعه‌باری رخ نمی‌دهد. Type به نام `RefCell<T>` زمانی مفید است که مطمئن هستید کدتان قوانین Borrowing را رعایت می‌کند، اما Compiler نمی‌تواند این موضوع را تشخیص دهد و تضمین کند.

مشابه `Rc<T>`، Type به نام `RefCell<T>` فقط برای سناریوهای Single-threaded مناسب است و اگر تلاش کنید از آن در یک Context چندنخی استفاده کنید، با خطای Compile مواجه می‌شوید. در فصل ۱۶ دربارهٔ روش دستیابی به قابلیت‌های `RefCell<T>` در یک برنامهٔ Multithreaded صحبت خواهیم کرد.

در اینجا دلایل انتخاب `Box<T>`، `Rc<T>` یا `RefCell<T>` را مرور می‌کنیم:

- `Rc<T>` امکان Multiple Ownership روی یک دادهٔ مشترک را فراهم می‌کند؛ درحالی‌که `Box<T>` و `RefCell<T>` فقط یک Owner دارند.
- `Box<T>` امکان Borrow کردن به‌صورت Immutable یا Mutable را فراهم می‌کند و این Borrowها در Compile Time بررسی می‌شوند. `Rc<T>` فقط Immutable Borrow را مجاز می‌داند که در Compile Time بررسی می‌شود. `RefCell<T>` هم Immutable Borrow و هم Mutable Borrow را مجاز می‌داند، اما بررسی آن‌ها در Runtime انجام می‌شود.
- از آنجا که `RefCell<T>` اجازهٔ Mutable Borrowهایی را می‌دهد که در Runtime بررسی می‌شوند، حتی وقتی خود `RefCell<T>` Immutable است نیز می‌توانید مقدار داخل آن را تغییر دهید.

تغییر مقدار داخل یک مقدار Immutable همان الگوی Interior Mutability است. بیایید موقعیتی را بررسی کنیم که این الگو در آن مفید است و ببینیم چگونه چنین کاری امکان‌پذیر می‌شود.

<!-- Old headings. Do not remove or links may break. -->

<a id="interior-mutability-a-mutable-borrow-to-an-immutable-value"></a>

### استفاده از Interior Mutability

یکی از پیامدهای قوانین Borrowing این است که وقتی یک مقدار Immutable دارید، نمی‌توانید آن را به‌صورت Mutable Borrow کنید. برای مثال، کد زیر Compile نمی‌شود:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/no-listing-01-cant-borrow-immutable-as-mutable/src/main.rs}}
```

اگر تلاش کنید این کد را Compile کنید، خطای زیر را دریافت خواهید کرد:

```console
{{#include ../listings/ch15-smart-pointers/no-listing-01-cant-borrow-immutable-as-mutable/output.txt}}
```

بااین‌حال، موقعیت‌هایی وجود دارند که در آن‌ها مفید است یک مقدار بتواند در Methodهای خودش تغییر کند، اما از دید کدهای دیگر Immutable به نظر برسد. کد بیرون از Methodهای آن مقدار نباید قادر به تغییرش باشد. استفاده از `RefCell<T>` یکی از راه‌های دستیابی به Interior Mutability است؛ اما `RefCell<T>` قوانین Borrowing را به‌طور کامل کنار نمی‌گذارد. Borrow Checker در Compiler این نوع Interior Mutability را مجاز می‌داند و قوانین Borrowing به‌جای Compile Time در Runtime بررسی می‌شوند. اگر این قوانین را نقض کنید، به‌جای خطای Compiler با `panic!` روبه‌رو خواهید شد.

بیایید یک مثال کاربردی را بررسی کنیم که در آن می‌توانیم از `RefCell<T>` برای تغییر یک مقدار Immutable استفاده کنیم و ببینیم چرا این قابلیت مفید است.

<!-- Old headings. Do not remove or links may break. -->

<a id="a-use-case-for-interior-mutability-mock-objects"></a>

#### تست‌کردن با Mock Objectها

گاهی برنامه‌نویس هنگام تست از یک Type به‌جای Type دیگری استفاده می‌کند تا رفتار مشخصی را مشاهده کند و بررسی کند که آیا آن رفتار به‌درستی پیاده‌سازی شده است یا نه. به این Type جایگزین **Test Double** می‌گویند. آن را مانند بدل‌کار در فیلم‌سازی در نظر بگیرید؛ فردی که به‌جای بازیگر قرار می‌گیرد تا صحنه‌ای دشوار را اجرا کند. Test Doubleها هنگام اجرای تست، نقش Typeهای دیگر را بر عهده می‌گیرند. **Mock Object**ها نوع مشخصی از Test Doubleها هستند که اتفاقات رخ‌داده در طول تست را ثبت می‌کنند تا بتوانید بررسی کنید آیا عملیات موردنظر به‌درستی انجام شده‌اند یا خیر.

Rust دقیقاً به همان معنایی که برخی زبان‌های دیگر از Object استفاده می‌کنند، Object ندارد و برخلاف بعضی زبان‌ها، قابلیت Mock Object نیز در Standard Library آن تعبیه نشده است. بااین‌حال قطعاً می‌توانید Structای ایجاد کنید که همان هدف را برآورده کند.

سناریویی را که قرار است تست کنیم در نظر بگیرید: یک Library می‌سازیم که مقداری را در برابر یک مقدار حداکثری دنبال می‌کند و بر اساس میزان نزدیکی مقدار فعلی به مقدار حداکثری، پیام‌هایی ارسال می‌کند. برای مثال، این Library می‌تواند برای پیگیری سهمیهٔ تعداد API Callهایی استفاده شود که یک کاربر مجاز به انجام آن‌هاست.

Library ما فقط قابلیت پیگیری فاصلهٔ مقدار فعلی تا مقدار حداکثری و تعیین پیام مناسب در زمان‌های مختلف را فراهم می‌کند. انتظار داریم Applicationهایی که از Library استفاده می‌کنند، سازوکار ارسال پیام‌ها را خودشان فراهم کنند: Application می‌تواند پیام را مستقیماً به کاربر نمایش دهد، Email ارسال کند، Text Message بفرستد یا کار دیگری انجام دهد. Library نیازی ندارد از جزئیات این کار مطلع باشد. تنها چیزی که نیاز دارد مقداری است که Traitای را که ما به نام `Messenger` ارائه می‌کنیم پیاده‌سازی کرده باشد. Listing 15-20 کد Library را نشان می‌دهد.

<Listing number="15-20" file-name="src/lib.rs" caption="یک Library برای پیگیری میزان نزدیکی مقدار به مقدار حداکثری و هشدار در سطوح مشخص">

```rust,noplayground
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-20/src/lib.rs}}
```

</Listing>

یکی از بخش‌های مهم این کد آن است که Trait به نام `Messenger` متدی به نام `send` دارد که یک Immutable Reference به `self` و متن پیام را دریافت می‌کند. این Trait همان Interfaceای است که Mock Object ما باید پیاده‌سازی کند تا بتوان از Mock به همان شکلی استفاده کرد که از یک Object واقعی استفاده می‌شود. بخش مهم دیگر این است که می‌خواهیم رفتار متد `set_value` در `LimitTracker` را تست کنیم. می‌توانیم مقداری را که به پارامتر `value` می‌دهیم تغییر دهیم، اما متد `set_value` چیزی برنمی‌گرداند که بتوانیم روی آن Assertion بنویسیم. می‌خواهیم بتوانیم بگوییم اگر یک `LimitTracker` با مقداری که Trait به نام `Messenger` را پیاده‌سازی کرده و مقدار مشخصی برای `max` ایجاد کنیم، وقتی عددهای مختلفی را به `value` می‌دهیم، Messenger باید پیام‌های مناسب را برای ارسال دریافت کند.

به Mock Objectای نیاز داریم که به‌جای ارسال Email یا Text Message هنگام فراخوانی `send`، فقط پیام‌هایی را که از آن خواسته شده ارسال کند ثبت کند. می‌توانیم یک نمونهٔ جدید از Mock Object ایجاد کنیم، یک `LimitTracker` بسازیم که از آن Mock استفاده می‌کند، متد `set_value` را روی `LimitTracker` فراخوانی کنیم و سپس بررسی کنیم آیا Mock Object پیام‌هایی را که انتظار داریم ثبت کرده است یا نه. Listing 15-21 تلاشی برای پیاده‌سازی چنین Mock Objectای را نشان می‌دهد؛ اما Borrow Checker اجازهٔ این کار را نمی‌دهد.

<Listing number="15-21" file-name="src/lib.rs" caption="تلاش برای پیاده‌سازی `MockMessenger` که Borrow Checker آن را مجاز نمی‌داند">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-21/src/lib.rs:here}}
```

</Listing>

این کد تست Structای به نام `MockMessenger` تعریف می‌کند که یک Field به نام `sent_messages` دارد. این Field یک `Vec` از مقدارهای `String` است تا پیام‌هایی را که Mock باید ارسال کند ثبت کند. همچنین یک Associated Function به نام `new` تعریف می‌کنیم تا ساختن مقدارهای جدید `MockMessenger` که با فهرستی خالی از پیام‌ها شروع می‌شوند آسان باشد. سپس Trait به نام `Messenger` را برای `MockMessenger` پیاده‌سازی می‌کنیم تا بتوانیم یک `MockMessenger` را به `LimitTracker` بدهیم. در تعریف متد `send`، پیام دریافتی به‌عنوان پارامتر را می‌گیریم و آن را در فهرست `sent_messages` متعلق به `MockMessenger` ذخیره می‌کنیم.

در تست، بررسی می‌کنیم وقتی به `LimitTracker` گفته می‌شود مقدار `value` را روی عددی بیشتر از ۷۵ درصد مقدار `max` تنظیم کند، چه اتفاقی می‌افتد. ابتدا یک `MockMessenger` جدید می‌سازیم که فهرست پیام‌های آن خالی است. سپس یک `LimitTracker` جدید ایجاد می‌کنیم و Referenceای به `MockMessenger` جدید و مقدار `100` را برای `max` به آن می‌دهیم. متد `set_value` را روی `LimitTracker` با مقدار `80` فراخوانی می‌کنیم که بیشتر از ۷۵ درصدِ ۱۰۰ است. سپس Assertion می‌نویسیم که فهرست پیام‌های ثبت‌شده در `MockMessenger` باید اکنون شامل یک پیام باشد.

اما همان‌طور که در ادامه می‌بینید، این تست یک مشکل دارد:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-21/output.txt}}
```

نمی‌توانیم `MockMessenger` را برای ثبت پیام‌ها تغییر دهیم؛ زیرا متد `send` یک Immutable Reference به `self` دریافت می‌کند. همچنین نمی‌توانیم پیشنهاد پیام خطا را بپذیریم و در پیاده‌سازی متد و تعریف Trait، از `&mut self` استفاده کنیم. نمی‌خواهیم صرفاً به‌خاطر تست، Trait به نام `Messenger` را تغییر دهیم. در عوض باید راهی پیدا کنیم که کد تست با طراحی فعلی ما به‌درستی کار کند.

این دقیقاً موقعیتی است که Interior Mutability می‌تواند کمک کند! فیلد `sent_messages` را داخل یک `RefCell<T>` ذخیره می‌کنیم تا متد `send` بتواند آن را تغییر دهد و پیام‌هایی را که مشاهده کرده‌ایم در آن ثبت کند. Listing 15-22 شکل این پیاده‌سازی را نشان می‌دهد.

<Listing number="15-22" file-name="src/lib.rs" caption="استفاده از `RefCell<T>` برای تغییر مقدار داخلی، درحالی‌که مقدار بیرونی Immutable در نظر گرفته می‌شود">

```rust,noplayground
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-22/src/lib.rs:here}}
```

</Listing>

اکنون Type فیلد `sent_messages` به‌جای `Vec<String>` برابر با `RefCell<Vec<String>>` است. در تابع `new` یک نمونهٔ جدید از `RefCell<Vec<String>>` می‌سازیم که یک Vector خالی را در بر می‌گیرد.

در پیاده‌سازی متد `send`، پارامتر اول همچنان یک Immutable Borrow از `self` است و با تعریف Trait مطابقت دارد. متد `borrow_mut` را روی `RefCell<Vec<String>>` موجود در `self.sent_messages` فراخوانی می‌کنیم تا Mutable Referenceای به مقدار داخل `RefCell<Vec<String>>`، یعنی Vector، به دست آوریم. سپس می‌توانیم متد `push` را روی Mutable Reference مربوط به Vector فراخوانی کنیم تا پیام‌هایی را که در طول تست ارسال شده‌اند ثبت کنیم.

آخرین تغییری که باید انجام دهیم مربوط به Assertion است: برای مشاهدهٔ تعداد آیتم‌های داخل Vector، متد `borrow` را روی `RefCell<Vec<String>>` فراخوانی می‌کنیم تا یک Immutable Reference به Vector به دست آوریم.

اکنون که نحوهٔ استفاده از `RefCell<T>` را دیدید، بیایید با جزئیات بیشتری بررسی کنیم که چگونه کار می‌کند!

<!-- Old headings. Do not remove or links may break. -->

<a id="keeping-track-of-borrows-at-runtime-with-refcellt"></a>

#### پیگیری Borrowها در Runtime

هنگام ایجاد Immutable Reference و Mutable Reference، به‌ترتیب از Syntaxهای `&` و `&mut` استفاده می‌کنیم. اما با `RefCell<T>` از متدهای `borrow` و `borrow_mut` استفاده می‌کنیم که بخشی از Safe API متعلق به `RefCell<T>` هستند. متد `borrow` نوع Smart Pointer به نام `Ref<T>` را برمی‌گرداند و متد `borrow_mut` نوع Smart Pointer به نام `RefMut<T>` را برمی‌گرداند. هر دو Type، Trait به نام `Deref` را پیاده‌سازی می‌کنند؛ بنابراین می‌توانیم مانند Referenceهای معمولی با آن‌ها کار کنیم.

`RefCell<T>` تعداد Smart Pointerهای `Ref<T>` و `RefMut<T>` را که در حال حاضر فعال هستند دنبال می‌کند. هر بار که `borrow` را فراخوانی می‌کنیم، `RefCell<T>` شمارندهٔ Borrowهای Immutable فعال را افزایش می‌دهد. وقتی یک مقدار `Ref<T>` از Scope خارج می‌شود، تعداد Borrowهای Immutable یک واحد کاهش پیدا می‌کند. درست مانند قوانین Borrowing در Compile Time، `RefCell<T>` در هر لحظه یا تعداد زیادی Immutable Borrow یا فقط یک Mutable Borrow را مجاز می‌داند.

اگر تلاش کنیم این قوانین را نقض کنیم، به‌جای دریافت خطای Compiler ــ همان‌طور که در صورت استفاده از Referenceها رخ می‌دهد ــ پیاده‌سازی `RefCell<T>` در Runtime باعث Panic می‌شود. Listing 15-23 نسخهٔ تغییریافته‌ای از پیاده‌سازی `send` در Listing 15-22 را نشان می‌دهد. عمداً تلاش می‌کنیم دو Mutable Borrow را در یک Scope فعال ایجاد کنیم تا ببینیم `RefCell<T>` چگونه در Runtime جلوی این کار را می‌گیرد.

<Listing number="15-23" file-name="src/lib.rs" caption="ایجاد دو Mutable Reference در یک Scope برای مشاهدهٔ Panic کردن `RefCell<T>`">

```rust,ignore,panics
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-23/src/lib.rs:here}}
```

</Listing>

متغیری به نام `one_borrow` را برای Smart Pointer از نوع `RefMut<T>` ایجاد می‌کنیم که از `borrow_mut` برگردانده شده است. سپس به همین روش یک Mutable Borrow دیگر در متغیر `two_borrow` ایجاد می‌کنیم. در نتیجه دو Mutable Reference در یک Scope داریم که مجاز نیست. وقتی تست‌های Library را اجرا می‌کنیم، کد Listing 15-23 بدون هیچ خطایی Compile می‌شود، اما تست شکست می‌خورد:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-23/output.txt}}
```

توجه کنید که کد با پیام `already borrowed: BorrowMutError` دچار Panic شده است. این همان روشی است که `RefCell<T>` برای مدیریت نقض قوانین Borrowing در Runtime به کار می‌گیرد.

انتخاب بررسی خطاهای Borrowing در Runtime به‌جای Compile Time، همان‌طور که در این مثال انجام دادیم، به این معناست که ممکن است اشتباهات کد را دیرتر در فرایند توسعه پیدا کنید؛ شاید حتی تا زمانی که کد شما در Production Deploy شده باشد. همچنین کد شما به‌دلیل پیگیری Borrowها در Runtime به‌جای Compile Time، اندکی سربار عملکردی خواهد داشت. بااین‌حال، استفاده از `RefCell<T>` امکان نوشتن Mock Objectای را فراهم می‌کند که می‌تواند خودش را تغییر دهد تا پیام‌هایی را که دریافت کرده ثبت کند؛ آن هم در Contextای که فقط مقدارهای Immutable مجاز هستند. با وجود این Trade-offها، می‌توانید از `RefCell<T>` برای دستیابی به قابلیت‌هایی فراتر از آنچه Referenceهای معمولی ارائه می‌کنند استفاده کنید.

<!-- Old headings. Do not remove or links may break. -->

<a id="having-multiple-owners-of-mutable-data-by-combining-rc-t-and-ref-cell-t"></a>
<a id="allowing-multiple-owners-of-mutable-data-with-rct-and-refcellt"></a>

### داشتن چند Owner برای دادهٔ Mutable

یکی از روش‌های رایج استفاده از `RefCell<T>`، ترکیب آن با `Rc<T>` است. به یاد بیاورید که `Rc<T>` امکان داشتن چند Owner برای یک داده را فراهم می‌کند، اما فقط دسترسی Immutable به آن داده را می‌دهد. اگر یک `Rc<T>` داشته باشید که `RefCell<T>` را در خود نگه می‌دارد، می‌توانید مقداری داشته باشید که هم چند Owner دارد و هم قابل تغییر است!

برای مثال، مثال Cons List در Listing 15-18 را به یاد بیاورید که در آن از `Rc<T>` استفاده کردیم تا چند لیست بتوانند در Ownership یک لیست دیگر شریک شوند. از آنجا که `Rc<T>` فقط مقدارهای Immutable را نگه می‌دارد، پس از ایجاد لیست نمی‌توانیم هیچ‌یک از مقدارهای آن را تغییر دهیم. حالا `RefCell<T>` را برای قابلیت تغییر مقدارهای داخل لیست به آن اضافه می‌کنیم. Listing 15-24 نشان می‌دهد که با استفاده از `RefCell<T>` در تعریف `Cons` می‌توانیم مقدار ذخیره‌شده در تمام لیست‌ها را تغییر دهیم.

<Listing number="15-24" file-name="src/main.rs" caption="استفاده از `Rc<RefCell<i32>>` برای ایجاد `List`ای که می‌توانیم آن را تغییر دهیم">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-24/src/main.rs}}
```

</Listing>

مقداری ایجاد می‌کنیم که نمونه‌ای از `Rc<RefCell<i32>>` است و آن را در متغیری به نام `value` ذخیره می‌کنیم تا بعداً بتوانیم مستقیماً به آن دسترسی داشته باشیم. سپس لیستی در `a` ایجاد می‌کنیم که Variant آن از نوع `Cons` است و `value` را در خود نگه می‌دارد. باید `value` را Clone کنیم تا هم `a` و هم `value` مالک مقدار داخلی `5` باشند؛ به‌جای اینکه Ownership از `value` به `a` منتقل شود یا `a` از `value` Borrow کند.

لیست `a` را داخل یک `Rc<T>` قرار می‌دهیم تا وقتی لیست‌های `b` و `c` را ایجاد می‌کنیم، هر دو بتوانند به `a` اشاره کنند؛ درست همان کاری که در Listing 15-18 انجام دادیم.

پس از ایجاد لیست‌های `a`، `b` و `c`، می‌خواهیم عدد ۱۰ را به مقدار موجود در `value` اضافه کنیم. این کار را با فراخوانی `borrow_mut` روی `value` انجام می‌دهیم. این فراخوانی از قابلیت Automatic Dereferencing که در فصل ۴ بررسی کردیم استفاده می‌کند تا `Rc<T>` را Dereference کرده و به مقدار داخلی `RefCell<T>` برسد. متد `borrow_mut` یک Smart Pointer از نوع `RefMut<T>` برمی‌گرداند و با استفاده از Dereference Operator روی آن، مقدار داخلی را تغییر می‌دهیم.

وقتی `a`، `b` و `c` را چاپ می‌کنیم، می‌بینیم که مقدار تغییر‌یافتهٔ `15` را دارند، نه `5`:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-24/output.txt}}
```

این روش واقعاً جالب است! با استفاده از `RefCell<T>`، یک مقدار `List` داریم که از بیرون Immutable به نظر می‌رسد. اما می‌توانیم از متدهای `RefCell<T>` که دسترسی به قابلیت Interior Mutability را فراهم می‌کنند استفاده کنیم تا هر زمان لازم بود دادهٔ خود را تغییر دهیم. بررسی قوانین Borrowing در Runtime از ما در برابر Data Race محافظت می‌کند و گاهی ارزش دارد برای دستیابی به این انعطاف‌پذیری در Data Structureها، کمی از سرعت صرف‌نظر کنیم. توجه کنید که `RefCell<T>` برای کدهای Multithreaded مناسب نیست! `Mutex<T>` نسخهٔ Thread-safe از `RefCell<T>` است و در فصل ۱۶ دربارهٔ آن صحبت خواهیم کرد.

{{#quiz ../quizzes/ch15-05-interior-mutability.toml}}

[wheres-the---operator]: ch05-03-method-syntax.html#wheres-the---operator
