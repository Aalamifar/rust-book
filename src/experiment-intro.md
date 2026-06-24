# این کتاب چه تفاوتی دارد؟

<div style="display: flex; gap: 2em"> 

این کتاب یک fork آزمایشی از [*The Rust Programming Language*](http://doc.rust-lang.org/book/) است که پژوهشگران <a href="https://cel.cs.brown.edu/">Cognitive Engineering Lab</a> در دانشگاه Brown آن را ساخته‌اند. اگر کنجکاو هستید، این صفحه توضیح می‌دهد این کتاب چه تفاوتی با کتاب اصلی TRPL دارد. اما اگر فقط می‌خواهید یادگیری Rust را شروع کنید، می‌توانید این صفحه را رد کنید و بعدا به آن برگردید.

<div style="display: flex; flex-direction: column; justify-content: center">
  <img src="img/experiment/brown-logo.png" style="min-width: 150px" />
</div>

</div>


## سازوکارهای تعاملی

این کتاب سازوکارهایی معرفی می‌کند تا هنگام یادگیری، فعالانه با Rust درگیر شوید. نخست، آزمون‌هایی مانند آزمون زیر خواهید دید. با کلیک روی «Start» آن را امتحان کنید.

{{#quiz ../quizzes/example-quiz.toml}}

اگر به پرسشی پاسخ نادرست بدهید، می‌توانید آزمون را دوباره انجام دهید یا پاسخ‌های درست را ببینید. پیشنهاد می‌کنیم آزمون را آن‌قدر تکرار کنید تا به ۱۰۰٪ برسید &mdash; پیش از تلاش دوباره، آزادانه متن را مرور کنید. توجه کنید که پس از دیدن پاسخ‌های درست، دیگر نمی‌توانید همان آزمون را دوباره انجام دهید.

دوم، می‌توانید هر بخش از متن را حاشیه‌نویسی کنید و یادداشت‌های خود را دربارهٔ آن ثبت کنید. وقتی بخشی از متن را انتخاب کردید، روی دکمهٔ ✏️ کلیک کنید و اگر خواستید دیدگاهی بنویسید.

👈 این متن را هایلایت کنید! 👉

> **نکته:** اگر محتوایی را که هایلایت کرده‌اید تغییر دهیم، هایلایت‌های شما ناپدید می‌شوند. همچنین هایلایت‌ها به‌صورت cookie ذخیره می‌شوند. اگر cookieها را مسدود کنید یا مرورگر خود را تغییر دهید، هایلایت‌های قبلی را نخواهید دید.

## تغییرات محتوا

محتوای این کتاب بیشتر شبیه TRPL است و هر چند ماه یک‌بار این دو کتاب را همگام می‌کنیم. بزرگ‌ترین تفاوت در فصل [درک مالکیت][understanding-ownership] است. این کتاب مالکیت را با ایده‌ها و تصویرسازی‌هایی توضیح می‌دهد که پژوهش ما نشان داده در مقایسه با کتاب اصلی، می‌توانند درک شما از Rust را بهتر کنند. در سراسر کتاب نمودارهای زیادی شبیه نمونهٔ زیر خواهید دید که با استفاده از [Aquascope][aquascope] رفتار زمان کامپایل و زمان اجرای Rust را نمایش می‌دهند:

```aquascope,interpreter,horizontal
#fn main() {
let mut s = String::from("Hello world");`[]`
let hello = &s[0..5];`[]`
s.push_str("!");`[]`
drop(s);`[]`
#}
```

فراتر از مالکیت، چند ویرایش کوچک دیگر هم در کتاب انجام داده‌ایم تا برداشت‌های نادرستی را که در پاسخ‌های آزمون‌ها دیده‌ایم هدف بگیریم. اگر در یک آزمون یا بخش دیگری از کتاب مشکلی دیدید، می‌توانید در مخزن GitHub ما issue ثبت کنید: <https://github.com/cognitive-engineering-lab/rust-book>

_دوست دارید در آزمایش‌های دیگری دربارهٔ آسان‌تر کردن یادگیری و استفاده از Rust شرکت کنید؟ از اینجا نام‌نویسی کنید:_ <https://forms.gle/U3jEUkb2fGXykp1DA>


## انتشارهای علمی

تا اینجا، این آزمایش به دو مقالهٔ دسترسی‌آزاد منجر شده است. اگر می‌خواهید پژوهش دانشگاهی پشت این کتاب را ببینید، آن‌ها را بخوانید:

* ["Profiling Programming Language Learning"](https://dl.acm.org/doi/10.1145/3649812) <br />
  [Will Crichton][will] and [Shriram Krishnamurthi][shriram]. OOPSLA 2024. (Distinguished Paper.)

* ["A Grounded Conceptual Model for Ownership Types in Rust"](https://dl.acm.org/doi/10.1145/3622841) <br />
  [Will Crichton][will], [Gavin Gray][gavin], and [Shriram Krishnamurthi][shriram]. OOPSLA 2023. (SIGPLAN Research Highlight and Communications of the ACM Research Highlight.)

## سپاسگزاری

این کار بخشی با پشتیبانی DARPA تحت Agreement No. HR00112420354، بخشی با پشتیبانی NSF تحت Award No. CCF-2227863، و بخشی با پشتیبانی Amazon Web Services انجام شده است. هر نظر، یافته، نتیجه‌گیری یا پیشنهادی که در این متن آمده، متعلق به نویسندگان است و لزوما بازتاب‌دهندهٔ دیدگاه پشتیبانان مالی ما نیست. از Carol Nichols و Rust Foundation برای کمک به معرفی این آزمایش سپاسگزاریم. TRPL پیش از آغاز این آزمایش، حاصل تلاش سخت افراد بسیاری بوده است.

[understanding-ownership]: ch04-00-understanding-ownership.html
[aquascope]: https://cognitive-engineering-lab.github.io/aquascope/
[will]: https://willcrichton.net/
[gavin]: https://gavinleroy.com/
[shriram]: https://cs.brown.edu/people/sk/
