## Reference Cycleها می‌توانند باعث Memory Leak شوند

تضمین‌های Memory Safety در Rust ایجاد تصادفی حافظه‌ای را که هرگز پاک‌سازی نمی‌شود دشوار می‌کنند، اما غیرممکن نمی‌کنند. به این وضعیت **Memory Leak** گفته می‌شود. جلوگیری کامل از Memory Leak جزو تضمین‌های Rust نیست؛ به این معنا که Memory Leakها در Rust همچنان از نظر Memory Safety مجاز هستند. با استفاده از `Rc<T>` و `RefCell<T>` می‌توانیم ببینیم Rust اجازهٔ ایجاد Memory Leak را می‌دهد: امکان ایجاد Referenceهایی وجود دارد که آیتم‌ها در یک Cycle به یکدیگر اشاره کنند. این وضعیت باعث Memory Leak می‌شود؛ زیرا Reference Count هر آیتم درون Cycle هرگز به صفر نمی‌رسد و در نتیجه مقدارها هیچ‌وقت Drop نمی‌شوند.

### ایجاد Reference Cycle

بیایید ببینیم Reference Cycle چگونه ممکن است ایجاد شود و چگونه می‌توان از آن جلوگیری کرد. برای شروع، تعریف Enum به نام `List` و متد `tail` را در Listing 15-25 بررسی می‌کنیم.

<Listing number="15-25" file-name="src/main.rs" caption="تعریف Cons List با `RefCell<T>` برای تغییر چیزی که یک Variant از نوع `Cons` به آن اشاره می‌کند">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-25/src/main.rs:here}}
```

</Listing>

در اینجا از نسخهٔ دیگری از تعریف `List` در Listing 15-5 استفاده می‌کنیم. عضو دوم در Variant به نام `Cons` اکنون از نوع `RefCell<Rc<List>>` است. یعنی به‌جای اینکه مانند Listing 15-24 بتوانیم مقدار `i32` را تغییر دهیم، می‌خواهیم مقدار `List`ای را تغییر دهیم که یک Variant از نوع `Cons` به آن اشاره می‌کند. همچنین متد `tail` را اضافه می‌کنیم تا اگر با یک Variant از نوع `Cons` روبه‌رو بودیم، دسترسی به عضو دوم آن آسان‌تر شود.

در Listing 15-26 تابع `main` را اضافه می‌کنیم که از تعریف‌های Listing 15-25 استفاده می‌کند. این کد یک لیست در `a` و یک لیست در `b` ایجاد می‌کند که به لیست موجود در `a` اشاره می‌کند. سپس لیست موجود در `a` را تغییر می‌دهد تا به `b` اشاره کند و در نتیجه یک Reference Cycle ایجاد می‌شود. در بخش‌های مختلف کد دستورهای `println!` قرار داده‌ایم تا Reference Countها را در مراحل مختلف این فرایند مشاهده کنیم.

<Listing number="15-26" file-name="src/main.rs" caption="ایجاد Reference Cycle میان دو مقدار `List` که به یکدیگر اشاره می‌کنند">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-26/src/main.rs:here}}
```

</Listing>

یک نمونه از `Rc<List>` ایجاد می‌کنیم که مقدار `List`ای با محتوای اولیهٔ `5, Nil` را در متغیر `a` نگه می‌دارد. سپس نمونهٔ دیگری از `Rc<List>` در متغیر `b` ایجاد می‌کنیم که مقدار `List` دیگری را در خود دارد؛ این لیست شامل مقدار `10` است و به لیست موجود در `a` اشاره می‌کند.

`a` را تغییر می‌دهیم تا به‌جای `Nil` به `b` اشاره کند و یک Cycle بسازیم. برای این کار از متد `tail` استفاده می‌کنیم تا Referenceای به `RefCell<Rc<List>>` موجود در `a` به دست آوریم و آن را در متغیر `link` قرار دهیم. سپس متد `borrow_mut` را روی `RefCell<Rc<List>>` فراخوانی می‌کنیم تا مقدار داخلی آن را از `Rc<List>`ای که یک مقدار `Nil` را نگه می‌دارد به `Rc<List>` موجود در `b` تغییر دهیم.

اگر این کد را اجرا کنیم و فعلاً آخرین دستور `println!` را Comment کنیم، خروجی زیر را دریافت خواهیم کرد:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-26/output.txt}}
```

پس از آنکه لیست موجود در `a` را تغییر می‌دهیم تا به `b` اشاره کند، Reference Count نمونه‌های `Rc<List>` در هر دو متغیر `a` و `b` برابر با ۲ است. در پایان تابع `main`، Rust متغیر `b` را Drop می‌کند و Reference Count نمونهٔ `Rc<List>` موجود در `b` از ۲ به ۱ کاهش می‌یابد. در این مرحله حافظهٔ Heap مربوط به `Rc<List>` آزاد نمی‌شود؛ زیرا Reference Count آن ۱ است، نه صفر. سپس Rust متغیر `a` را Drop می‌کند و Reference Count نمونهٔ `Rc<List>` موجود در `a` نیز از ۲ به ۱ کاهش پیدا می‌کند. حافظهٔ این نمونه هم آزاد نمی‌شود؛ زیرا نمونهٔ دیگر `Rc<List>` همچنان به آن اشاره می‌کند. در نتیجه حافظهٔ تخصیص‌یافته به لیست برای همیشه بدون پاک‌سازی باقی می‌ماند. برای نمایش این Reference Cycle، نمودار Figure 15-4 را آماده کرده‌ایم.

<img alt="A rectangle labeled 'a' that points to a rectangle containing the integer 5. A rectangle labeled 'b' that points to a rectangle containing the integer 10. The rectangle containing 5 points to the rectangle containing 10, and the rectangle containing 10 points back to the rectangle containing 5, creating a cycle." src="img/trpl15-04.svg" class="center" />

<span class="caption">Figure 15-4: یک Reference Cycle که در آن لیست‌های `a` و `b` به یکدیگر اشاره می‌کنند</span>

اگر آخرین دستور `println!` را از حالت Comment خارج کنید و برنامه را اجرا کنید، Rust تلاش می‌کند این Cycle را چاپ کند؛ یعنی `a` به `b` اشاره می‌کند، `b` به `a` و این روند همین‌طور ادامه پیدا می‌کند تا در نهایت Stack سرریز شود.

در مقایسه با یک برنامهٔ واقعی، پیامدهای ایجاد Reference Cycle در این مثال چندان جدی نیست؛ زیرا برنامه بلافاصله پس از ایجاد Reference Cycle به پایان می‌رسد. اما اگر یک برنامهٔ پیچیده‌تر مقدار زیادی حافظه را در قالب یک Cycle تخصیص دهد و برای مدت طولانی نگه دارد، بیش از حافظهٔ موردنیاز خود مصرف خواهد کرد و ممکن است فشار زیادی به سیستم وارد کند؛ تا جایی که حافظهٔ در دسترس تمام شود.

ایجاد Reference Cycle کار ساده‌ای نیست، اما غیرممکن هم نیست. اگر مقدارهای `RefCell<T>` داشته باشید که مقدارهای `Rc<T>` را در خود نگه می‌دارند، یا از ترکیب‌های تو‌در‌توی مشابهی از Typeهای دارای Interior Mutability و Reference Counting استفاده کنید، باید مطمئن شوید Cycle ایجاد نمی‌کنید؛ زیرا نمی‌توانید انتظار داشته باشید Rust این مورد را تشخیص دهد. ایجاد Reference Cycle یک Logic Bug در برنامهٔ شما خواهد بود و باید با Automated Testها، Code Reviewها و سایر روش‌های توسعهٔ نرم‌افزار احتمال رخ‌دادن آن را به حداقل برسانید.

راه‌حل دیگر برای جلوگیری از Reference Cycleها، سازمان‌دهی مجدد Data Structureهاست؛ به‌گونه‌ای که بعضی Referenceها نشان‌دهندهٔ Ownership باشند و بعضی دیگر چنین رابطه‌ای را نشان ندهند. در نتیجه می‌توانید Cycleهایی داشته باشید که از ترکیبی از رابطه‌های Ownership و رابطه‌های بدون Ownership تشکیل شده‌اند و فقط رابطه‌های Ownership بر امکان Drop شدن یک مقدار تأثیر می‌گذارند. در Listing 15-25 همیشه می‌خواهیم Variantهای `Cons` مالک لیست خود باشند؛ بنابراین سازمان‌دهی مجدد Data Structure در آن مثال امکان‌پذیر نیست. بیایید مثالی با Graphهایی شامل Nodeهای والد و فرزند بررسی کنیم تا ببینیم چه زمانی رابطه‌های بدون Ownership راه مناسبی برای جلوگیری از Reference Cycleها هستند.

<!-- Old headings. Do not remove or links may break. -->

<a id="preventing-reference-cycles-turning-an-rct-into-a-weakt"></a>

### جلوگیری از Reference Cycle با استفاده از `Weak<T>`

تا اینجا نشان دادیم که فراخوانی `Rc::clone` مقدار `strong_count` یک نمونه از `Rc<T>` را افزایش می‌دهد و یک نمونه از `Rc<T>` فقط زمانی پاک‌سازی می‌شود که مقدار `strong_count` آن به صفر برسد. همچنین می‌توانید با فراخوانی `Rc::downgrade` و ارسال یک Reference به `Rc<T>`، یک Weak Reference به مقدار داخل آن ایجاد کنید. **Strong Reference**ها راهی برای به‌اشتراک‌گذاشتن Ownership یک نمونه از `Rc<T>` هستند. **Weak Reference**ها نشان‌دهندهٔ رابطهٔ Ownership نیستند و تعداد آن‌ها بر زمان پاک‌سازی یک نمونه از `Rc<T>` تأثیری ندارد. آن‌ها باعث Reference Cycle نمی‌شوند؛ زیرا هر Cycleای که شامل Weak Reference باشد، زمانی شکسته می‌شود که Strong Reference Count مقدارهای درگیر به صفر برسد.

وقتی `Rc::downgrade` را فراخوانی می‌کنید، یک Smart Pointer از نوع `Weak<T>` دریافت خواهید کرد. به‌جای اینکه `strong_count` در نمونهٔ `Rc<T>` یک واحد افزایش یابد، فراخوانی `Rc::downgrade` مقدار `weak_count` را یک واحد افزایش می‌دهد. Type به نام `Rc<T>` از `weak_count` برای پیگیری تعداد Referenceهای `Weak<T>` استفاده می‌کند؛ درست مانند `strong_count`. تفاوت این است که برای پاک‌سازی نمونهٔ `Rc<T>` لازم نیست مقدار `weak_count` به صفر برسد.

از آنجا که ممکن است مقداری که `Weak<T>` به آن اشاره می‌کند قبلاً Drop شده باشد، پیش از انجام هر کاری با مقدار مورد اشاره باید مطمئن شوید که آن مقدار هنوز وجود دارد. برای این کار متد `upgrade` را روی یک نمونه از `Weak<T>` فراخوانی می‌کنید. این متد یک `Option<Rc<T>>` برمی‌گرداند. اگر مقدار `Rc<T>` هنوز Drop نشده باشد، نتیجه `Some` خواهد بود و اگر مقدار `Rc<T>` از بین رفته باشد، نتیجه `None` است. از آنجا که `upgrade` یک `Option<Rc<T>>` برمی‌گرداند، Rust اطمینان می‌دهد که هر دو حالت `Some` و `None` مدیریت شوند و Pointer نامعتبری در اختیار شما قرار نگیرد.

به‌عنوان مثال، به‌جای استفاده از لیستی که هر عنصر آن فقط از عنصر بعدی خبر دارد، یک Tree ایجاد می‌کنیم که هر Node آن هم از فرزندان خود و هم از والد خود اطلاع دارد.

<!-- Old headings. Do not remove or links may break. -->

<a id="creating-a-tree-data-structure-a-node-with-child-nodes"></a>

#### ایجاد یک Tree Data Structure

برای شروع، Treeای می‌سازیم که Nodeهای آن از Nodeهای فرزند خود اطلاع دارند. یک Struct به نام `Node` ایجاد می‌کنیم که مقدار `i32` خودش و همچنین Referenceهایی به مقدارهای `Node` فرزند را نگه می‌دارد:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-27/src/main.rs:here}}
```

می‌خواهیم هر `Node` مالک فرزندان خود باشد و این Ownership را با متغیرها به اشتراک بگذاریم تا بتوانیم مستقیماً به هر `Node` در Tree دسترسی داشته باشیم. برای این کار، آیتم‌های `Vec<T>` را از نوع `Rc<Node>` تعریف می‌کنیم. همچنین می‌خواهیم بتوانیم Nodeهایی را که فرزند یک Node دیگر هستند تغییر دهیم؛ بنابراین در Field به نام `children` یک `RefCell<T>` قرار می‌دهیم که `Vec<Rc<Node>>` را در بر می‌گیرد.

در ادامه از تعریف Struct خود استفاده می‌کنیم و یک نمونه از `Node` به نام `leaf` با مقدار `3` و بدون فرزند می‌سازیم. سپس نمونهٔ دیگری به نام `branch` با مقدار `5` ایجاد می‌کنیم که `leaf` یکی از فرزندان آن است؛ همان‌طور که در Listing 15-27 نشان داده شده است.

<Listing number="15-27" file-name="src/main.rs" caption="ایجاد Nodeای به نام `leaf` بدون فرزند و Nodeای به نام `branch` که `leaf` یکی از فرزندان آن است">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-27/src/main.rs:there}}
```

</Listing>

`Rc<Node>` موجود در `leaf` را Clone می‌کنیم و آن را در `branch` ذخیره می‌کنیم. در نتیجه Node موجود در `leaf` اکنون دو Owner دارد: متغیر `leaf` و متغیر `branch`. می‌توانیم از طریق `branch.children` به `leaf` برسیم، اما راهی برای رسیدن از `leaf` به `branch` وجود ندارد. دلیلش این است که `leaf` هیچ Referenceای به `branch` ندارد و از رابطهٔ میان آن‌ها اطلاعی ندارد. می‌خواهیم `leaf` بداند که `branch` والد آن است. در ادامه این کار را انجام می‌دهیم.

#### افزودن Reference از فرزند به والد

برای اینکه Node فرزند از والد خود اطلاع داشته باشد، باید یک Field به نام `parent` به تعریف Struct به نام `Node` اضافه کنیم. مسئله این است که Type این Field چه باید باشد. می‌دانیم نمی‌تواند شامل `Rc<T>` باشد؛ زیرا در این صورت با اشارهٔ `leaf.parent` به `branch` و اشارهٔ `branch.children` به `leaf`، یک Reference Cycle ایجاد می‌شود و مقدار `strong_count` آن‌ها هرگز به صفر نمی‌رسد.

اگر رابطه‌ها را از زاویهٔ دیگری بررسی کنیم، یک Node والد باید مالک فرزندان خود باشد: اگر Node والد Drop شود، Nodeهای فرزند آن نیز باید Drop شوند. اما یک Node فرزند نباید مالک والد خود باشد؛ یعنی اگر Node فرزند را Drop کنیم، والد باید همچنان وجود داشته باشد. این دقیقاً موقعیتی است که Weak Referenceها برای آن مناسب‌اند!

بنابراین به‌جای `Rc<T>`، Type مربوط به Field به نام `parent` را از `Weak<T>` انتخاب می‌کنیم؛ به‌طور مشخص از `RefCell<Weak<Node>>` استفاده خواهیم کرد. اکنون تعریف Struct به نام `Node` به شکل زیر است:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-28/src/main.rs:here}}
```

یک Node می‌تواند به Node والد خود اشاره کند، اما مالک آن نیست. در Listing 15-28 تابع `main` را به‌روزرسانی می‌کنیم تا با استفاده از تعریف جدید، Node به نام `leaf` بتواند به والد خود، یعنی `branch`، اشاره کند.

<Listing number="15-28" file-name="src/main.rs" caption="یک Node به نام `leaf` با Weak Referenceای به Node والد خود، یعنی `branch`">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-28/src/main.rs:there}}
```

</Listing>

ایجاد Node به نام `leaf` مشابه Listing 15-27 است، به‌جز Field به نام `parent`: در ابتدا `leaf` هیچ والدی ندارد؛ بنابراین یک نمونهٔ جدید و خالی از Reference نوع `Weak<Node>` ایجاد می‌کنیم.

در این مرحله، اگر با استفاده از متد `upgrade` تلاش کنیم Referenceای به والد `leaf` به دست آوریم، مقدار `None` دریافت می‌کنیم. این موضوع در خروجی اولین دستور `println!` دیده می‌شود:

```text
leaf parent = None
```

هنگام ایجاد Node به نام `branch`، آن نیز در Field به نام `parent` یک Reference جدید از نوع `Weak<Node>` خواهد داشت؛ زیرا `branch` Node والد ندارد. همچنان `leaf` را به‌عنوان یکی از فرزندان `branch` نگه می‌داریم. پس از ایجاد نمونهٔ `Node` در `branch`، می‌توانیم `leaf` را تغییر دهیم تا یک Reference از نوع `Weak<Node>` به والد خود داشته باشد. متد `borrow_mut` را روی `RefCell<Weak<Node>>` موجود در Field به نام `parent` در `leaf` فراخوانی می‌کنیم و سپس با استفاده از تابع `Rc::downgrade`، از `Rc<Node>` موجود در `branch` یک Reference از نوع `Weak<Node>` به `branch` ایجاد می‌کنیم.

وقتی دوباره والد `leaf` را چاپ می‌کنیم، این بار یک Variant از نوع `Some` دریافت می‌کنیم که `branch` را در خود نگه می‌دارد: اکنون `leaf` می‌تواند به والد خود دسترسی داشته باشد! هنگام چاپ `leaf` نیز از Cycleای که در Listing 15-26 در نهایت به Stack Overflow منجر می‌شد جلوگیری می‌کنیم؛ Referenceهای `Weak<Node>` به‌صورت `(Weak)` چاپ می‌شوند:

```text
leaf parent = Some(Node { value: 5, parent: RefCell { value: (Weak) },
children: RefCell { value: [Node { value: 3, parent: RefCell { value: (Weak) },
children: RefCell { value: [] } }] } })
```

نبود خروجی بی‌نهایت نشان می‌دهد این کد Reference Cycle ایجاد نکرده است. با بررسی مقدارهایی که از فراخوانی `Rc::strong_count` و `Rc::weak_count` به دست می‌آوریم نیز می‌توانیم این موضوع را تشخیص دهیم.

#### مشاهدهٔ تغییرات `strong_count` و `weak_count`

بیایید تغییر مقدارهای `strong_count` و `weak_count` نمونه‌های `Rc<Node>` را بررسی کنیم. برای این کار یک Scope داخلی جدید ایجاد می‌کنیم و ساخت `branch` را به داخل آن انتقال می‌دهیم. با این کار می‌توانیم ببینیم وقتی `branch` ایجاد می‌شود و سپس با خروج از Scope از بین می‌رود، چه اتفاقی می‌افتد. تغییرات در Listing 15-29 نشان داده شده‌اند.

<Listing number="15-29" file-name="src/main.rs" caption="ایجاد `branch` در یک Scope داخلی و بررسی Strong Reference Count و Weak Reference Count">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-29/src/main.rs:here}}
```

</Listing>

پس از ایجاد `leaf`، نمونهٔ `Rc<Node>` آن دارای Strong Count برابر با ۱ و Weak Count برابر با ۰ است. در Scope داخلی، `branch` را ایجاد می‌کنیم و آن را به `leaf` مرتبط می‌سازیم. در این نقطه، وقتی Countها را چاپ می‌کنیم، نمونهٔ `Rc<Node>` موجود در `branch` دارای Strong Count برابر با ۱ و Weak Count برابر با ۱ خواهد بود؛ زیرا `leaf.parent` با استفاده از `Weak<Node>` به `branch` اشاره می‌کند. هنگام چاپ Countهای مربوط به `leaf` می‌بینیم Strong Count آن برابر با ۲ است؛ زیرا اکنون `branch` یک Clone از `Rc<Node>` مربوط به `leaf` را در `branch.children` ذخیره کرده است. بااین‌حال Weak Count آن همچنان صفر باقی می‌ماند.

وقتی Scope داخلی به پایان می‌رسد، `branch` از Scope خارج می‌شود و Strong Count مربوط به `Rc<Node>` به صفر کاهش می‌یابد؛ بنابراین `Node` آن Drop می‌شود. Weak Count برابر با ۱ که از `leaf.parent` باقی مانده است، هیچ تأثیری بر Drop شدن یا نشدن `Node` ندارد؛ بنابراین Memory Leak نخواهیم داشت!

اگر پس از پایان Scope تلاش کنیم به والد `leaf` دسترسی پیدا کنیم، دوباره `None` دریافت خواهیم کرد. در پایان برنامه، نمونهٔ `Rc<Node>` موجود در `leaf` دارای Strong Count برابر با ۱ و Weak Count برابر با ۰ است؛ زیرا متغیر `leaf` دوباره تنها Reference به آن `Rc<Node>` است.

تمام منطق مدیریت Countها و Drop شدن مقدارها درون `Rc<T>` و `Weak<T>` و پیاده‌سازی Trait به نام `Drop` برای آن‌ها قرار دارد. با مشخص‌کردن اینکه رابطهٔ میان فرزند و والد در تعریف `Node` باید از نوع Reference به `Weak<T>` باشد، می‌توانید کاری کنید که Nodeهای والد به Nodeهای فرزند اشاره کنند و برعکس، بدون اینکه Reference Cycle یا Memory Leak ایجاد شود.

## جمع‌بندی

در این فصل دیدیم چگونه از Smart Pointerها برای ایجاد تضمین‌ها و انتخاب Trade-offهایی متفاوت با رفتار پیش‌فرض Referenceهای معمولی در Rust استفاده کنیم. Type به نام `Box<T>` اندازه‌ای مشخص دارد و به داده‌ای اشاره می‌کند که روی Heap تخصیص داده شده است. Type به نام `Rc<T>` تعداد Referenceهای دادهٔ قرارگرفته روی Heap را دنبال می‌کند تا آن داده بتواند چند Owner داشته باشد. Type به نام `RefCell<T>` با استفاده از Interior Mutability، زمانی کاربرد دارد که به Typeای Immutable نیاز داریم، اما باید بتوانیم مقدار داخلی آن را تغییر دهیم. این Type همچنین قوانین Borrowing را به‌جای Compile Time در Runtime اعمال می‌کند.

همچنین Traitهای `Deref` و `Drop` را بررسی کردیم که بخش زیادی از قابلیت‌های Smart Pointerها را ممکن می‌کنند. Reference Cycleهایی را بررسی کردیم که می‌توانند باعث Memory Leak شوند و دیدیم چگونه می‌توان با استفاده از `Weak<T>` از آن‌ها جلوگیری کرد.

اگر این فصل علاقهٔ شما را برانگیخته است و می‌خواهید Smart Pointerهای خودتان را پیاده‌سازی کنید، برای اطلاعات بیشتر به [«The Rustonomicon»][nomicon] مراجعه کنید.

در فصل بعد دربارهٔ Concurrency در Rust صحبت خواهیم کرد. حتی با چند Smart Pointer جدید نیز آشنا خواهید شد.

{{#quiz ../quizzes/ch15-06-reference-cycles.toml}}

[nomicon]: https://doc.rust-lang.org/nomicon/index.html
