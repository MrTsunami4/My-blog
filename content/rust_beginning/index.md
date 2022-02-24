+++
title="the beginning of my journey with Rust"
description="a story about by humble begging with this new language"
date = 2020-06-16

[taxonomies]
tags = ["rust"]

[extra]
ToC = true
+++

# Why Rust?

I’ve been wanting to learn a low-level language for a while now. I was interested in it because I wanted to be able to make programs as optimized as possible. Indeed, in a language like C# or Java, for example, the language reaches a limit of possible optimizations. So I wanted to try a compiled language.

It is also true that Rust is currently a trendy programming language. I wanted to try this language that some people describe as revolutionary, especially its memory management.

# How I learned Rust

Usually, learning a new programming language always starts with finding a good tutorial. For me, a good tutorial is:
- simple enough that I don’t need to open additional documentation,
- complete enough that I can start doing real programs at the end, not just displaying my name on the command line,
- it doesn’t take me for a complete beginner, and explains to me for the fiftieth time what a variable is,
- and that he’s up to date enough not to have compilation errors in his examples.

Fortunately, with Rust, this search, which can sometimes be long, was not necessary. Rust has its own official tutorial, the [Rust Book](https://doc.rust-lang.org/stable/book/).  Not only is the tutorial very complete, but as it is known by the whole community, it is also regularly updated.

Reassured that I was in good hands, I launched into the “Rust Book”. The book starts by making us install rustup, the toolchain manager of Rust. This installation is done without any problems, we are then shown how to create our first project. With one small command, the project is created, with all the necessary files, and even git already configured. The Rust file is filled with my very first Rust program: “Hello world”:

```rs
fn main() {
    println!("Hello, world!");
}
```

In the rest of the tutorial, we are made to write a short program to discover the syntax of most of the basic elements of the language, like the if loop, functions and variables. This is also the moment to discover how to import an external library with cargo. I was very pleasantly surprised by the simplicity of the operation.

Without going into more detail, in the following chapters, the book takes us through most of the features of the language. My favorites are the creation of modules and their uses for working with multiple files, and the iterators and all their predefined functions. It looks like features reserved for high-level languages, like C# or Javascript, while keeping the speed of Rust. It is also at this moment that we discover how to organize data in struct and enum.

Then comes the second project of the book, a clone of the grep command line software. Now that you know the basics of the language, you can try to do the project only with the vague indications, without copying and pasting every line of code. I recommend this approach, as I think you learn better by doing it yourself. The tutorial also shows us the importance of organizing our data not only to make the program work, but also to make sense of the information.

```rs
struct Config {
    query: String,
    filename: String,
}
```
The book goes on to show us more advanced techniques, such as multithreading and traits. Then comes the last exercise, a multithreaded web server, which allows us to use all the skills learned throughout the book.

# First impressions

My first impressions are quite clear, and there’s no need to beat about the bush: I really like what I’ve seen of Rust.

## What I really like

I really like enums and structures. It’s a clear, concise and nice way to organize your code. I also like how to add methods to struc, especially the fact that the “new” method is a function, and not something else, like a constructor in C# for example. The use of traits is also, I think, a rather elegant way to inherit properties in a language that has no classes.

## What I still have a problem with

I still can’t write code that is right the first time, especially around memory issues. I don’t see this as a language problem, but rather as me not being used to it yet. And I’m very happy to say that, for now, that’s all.

# What next ?

After I finished reading the Rust book, I started to make some very simple projects, like a draft of ls, or the translation of an exercise given at school from C# to Rust.

I’m now planning to try my hand at more complicated projects, which will probably be a blog post in the future.