---
title: "Notes on Rust For Rustaceans"
date: 2024-06-23T20:19:47-05:00
lastmod: 2024-06-23T20:19:47-05:00
publishDate: 2024-06-22T20:19:47-05:00
tags: ["rust"]
ShowReadingTime: true
ShowToc: true
---

## Intro

I'm really enjoying reading [Jon Gjenset](https://x.com/jonhoo)'s [Rust for Rustaceans](https://rust-for-rustaceans.com/). I appreciated the author's decision to assume his target audience to be people who had just finished [The Book](https://doc.rust-lang.org/book/title-page.html), making it sort of an expansion or a Level II.

However, I found it light on contrasting examples, as well as images. I'm more of a visual learner, so I find them very useful to understand what's right (or wrong), or to help me create a mental model of a concept being described.

Therefore, to help me _really_ grasp the topics discussed in the book, I'm writing the following notes. I decided to share them here in case they're useful to someone else.

> ℹ️ I have written some of these notes out of my own intuition. If you find an incorrect explanation, or consider my mental model for a concept to be wrong, please [let me know](https://x.com/_0x5d)!

> ℹ️ This is a WIP. I'll add more as I make progress through the book and write more examples & explanations.

> ⚠️ All the base code snippets I'm quoting here are © 2022 John Gjengset. Please buy the book, it's a must-read if you're serious about learning Rust.

I'm dividing the post by chapters & sections within them. There might be missing sections, which would mean I didn't feel the need to supplement my reading with additional examples.

But anyway, enough talking. Here it is.

## Chapter 1: Foundations

### Section 1.3: Borrowing and Lifetimes

#### Generic Lifetimes

This subsection talks about how sometimes you need to specify different lifetimes for different fields in your types (making your type generic over multiple lifetimes). Here's the example presented in the book.

```rust
struct StrSplit<'s, 'p> {
    document: &'s str,
    delimiter: &'p str,
}

impl<'s, 'p> Iterator for StrSplit<'s, 'p> {
    type Item = &'s str;
    fn next(&mut self) -> Option<Self::Item> {
        todo!()
    }
}

fn str_before(s: &str, c: char) -> Option<&str> {
    fn str_before(s: &str, c: char) -> Option<&str> {
        StrSplit {document: s, delimiter: &c.to_string()}.next()
    }
}
```

The author then mentions that making `StrSplit` generic over a single lifetime would cause a compilation error. Indeed, if we make the change...

```rust
struct StrSplit<'s> {
    delimiter: &'s str,
    document: &'s str,
}

impl<'s> Iterator for StrSplit<'s> {
    // ... contents remain the same
}

fn str_before(s: &str, c: char) -> Option<&str> {
    StrSplit {document: s, delimiter: &c.to_string()}.next()
}
```

and run `cargo build`, we get the following error:

```bash
error[E0515]: cannot return value referencing temporary value
  --> src/main_wrong.rs:14:5
   |
14 |     StrSplit {document: s, delimiter: &c.to_string()}.next()
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^-------------^^^^^^^^
   |     |                                  |
   |     |                                  temporary value created here
   |     returns a value referencing data owned by the current function
```

It becomes more evident if we mark the lifetimes using the notation used in
The Book:
```rust
                                //---------------------------+- s & c are 'a
fn str_before(s: &str, c: char) -> Option<&str> {//          |
    let delim = &c.to_string(); //--+- delim must also be 'a |
    StrSplit {                  //  |                        |
        document: s,            //  |                        |
        delimiter: delim,       //  |                        |
    }                           //  |                        |
    .next()                     //  |                        |
                                //--+ delim is dropped       |
}                               //                           |
                                //---------------------------+ s & c live on!
```

`delim`'s timeline is shorter than `s`'s and `c`'s, causing a contradiction: `'a` < `'a`.
