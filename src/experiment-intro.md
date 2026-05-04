# What's Different About This Book?

<div style="display: flex; gap: 2em"> 

This book is an experimental fork of [*The Rust Programming Language*](http://doc.rust-lang.org/book/) created by researchers at the <a href="https://cel.cs.brown.edu/">Cognitive Engineering Lab</a> at Brown University. If you're curious, this page explains what makes this book different from the original TRPL book. But if you just want to get started learning Rust, then feel free to skip this page and come back later.

<div style="display: flex; flex-direction: column; justify-content: center">
  <img src="img/experiment/brown-logo.png" style="min-width: 150px" />
</div>

</div>


## Interactive Mechanics

This book introduces mechanics for you to actively engage with Rust while learning. First, you'll see quizzes like the one below. Try it out by clicking "Start".

{{#quiz ../quizzes/example-quiz.toml}}

If you get a question incorrect, you can choose to either retry the quiz, or see the correct answers. We encourage you to retry the quiz until you get 100% &mdash; feel free to review the content before retrying the quiz. Note that once you see the correct answers, you cannot retry the quiz.

Second, you can also annotate any piece of text to record your thoughts about it. Once you select some text, click the ✏️ button, and leave an optional comment.

👉 Try highlighting this text! 👈

> **Note:** your highlights will disappear if we change the content that you've highlighted. Also, your highlights are stored as a cookie. If you block cookies or change browsers, then you won't see your previous highlights.

## Content Changes

This book's content is mostly similar to TRPL, and we synchronize the books every few months. The biggest difference is the chapter on [Understanding Ownership][understanding-ownership]. This book explains ownership using ideas and visualizations that our research has demonstrated can better improve your understanding of Rust compared to the original book. You will see many diagrams like the ones below, which visualize the compile-time and run-time behavior of Rust using [Aquascope][aquascope]:

```aquascope,interpreter,horizontal
#fn main() {
let mut s = String::from("Hello world");`[]`
let hello = &s[0..5];`[]`
s.push_str("!");`[]`
drop(s);`[]`
#}
```

Beyond ownership, we've made a number of small edits to the book to target misconceptions observed in the quiz responses. If you spot an issue in a quiz or other part of the book, you can file an issue on our Github repository: <https://github.com/cognitive-engineering-lab/rust-book>

_Interested in participating in other experiments about making Rust easier to learn and use? Please sign up here:_ <https://forms.gle/U3jEUkb2fGXykp1DA>


## Publications

Thus far, this experiment has led to two open-access publications. Check them out if you're interested to see the academic research behind this book:

* ["Profiling Programming Language Learning"](https://dl.acm.org/doi/10.1145/3649812) <br />
  [Will Crichton][will] and [Shriram Krishnamurthi][shriram]. OOPSLA 2024. (Distinguished Paper.)

* ["A Grounded Conceptual Model for Ownership Types in Rust"](https://dl.acm.org/doi/10.1145/3622841) <br />
  [Will Crichton][will], [Gavin Gray][gavin], and [Shriram Krishnamurthi][shriram]. OOPSLA 2023. (SIGPLAN Research Highlight and Communications of the ACM Research Highlight.)

## Acknowledgments

This work was partially supported by the DARPA under Agreement No. HR00112420354, partially supported by the NSF under Award No. CCF-2227863, and partially supported by Amazon Web Services. Any opinions, findings, and conclusions or recommendations expressed in this material are those of the authors and do not reflect the views of our funders. We are grateful to Carol Nichols and the Rust Foundation for helping publicize the experiment. TRPL is the product of many people's hard work before we started this experiment.

[understanding-ownership]: ch04-00-understanding-ownership.html
[aquascope]: https://cognitive-engineering-lab.github.io/aquascope/
[will]: https://willcrichton.net/
[gavin]: https://gavinleroy.com/
[shriram]: https://cs.brown.edu/people/sk/