---
layout: post
title:  "Rusty Tip: A String manages its own memory, but a string slice does not."
date:   2022-01-03 12:00:00 EST
categories: rust programming
---
| Key Point: |
| ---- |
| The difference between a `String` type and a `str` type can be confusing; you should call a `String` a *string*, and a `str` a *string slice*. Remember that you can modify a *string*, but you cannot modify a *string slice*. |

In the Rust programming language, a `String` is a data structure
(a struct) that manages its own memory. In fact, a `String` is
really just some convenience functions that operate on a
`Vec<u8>`.

Suppose I create a new `String`:

{% highlight rust %}
let mut buffer = String::new();
{% endhighlight %}

The memory for the `String` (which is
[actually a `Vec<u8>`](https://doc.rust-lang.org/src/alloc/string.rs.html)) is
valid for the scope in which it is created. As soon as we leave
this scope, then the memory goes away. This lets us modify the `String`
in place, with code like this:

{% highlight rust %}
// Reset the String to be empty, or blank
buffer.clear();
// Append some text to the end of the String
buffer.push_str("Two dozen lemurs sat on a fence.");
// Print the String (display it to the console or standard output)
println!(buffer);
{% endhighlight %}

What we cannot do, however, is return a new `String` from a function.
This is because as soon as our local scope goes away, the `String` will
loose access to its allocated memory. (The lifetime of the memory expires.)

Some useful methods to know for the `String` type are:

- [`String::new()`](https://doc.rust-lang.org/std/string/struct.String.html#method.new)
- [`String::with_capacity(size)`](https://doc.rust-lang.org/std/string/struct.String.html#method.with_capacity)
- [`String::clear()`](https://doc.rust-lang.org/std/string/struct.String.html#method.clear)
- [`String::push_str(str)`](https://doc.rust-lang.org/std/string/struct.String.html#method.push_str)
- [`String::len()`](https://doc.rust-lang.org/std/string/struct.String.html#method.len)
- [`String::from(str)`](https://doc.rust-lang.org/std/string/struct.String.html#method.from)

[Documentation for the String type](https://doc.rust-lang.org/std/string/struct.String.html)
can be found in the
[Rust Standard Library documentation](https://doc.rust-lang.org/std/index.html).

Be careful not to confuse the `String` type with the
[primitive type called `str`](https://doc.rust-lang.org/std/primitive.str.html),
or a *string slice*.
A `str` is zero or more contiguous UTF-8 characters.

The difference between a `String` type and a `str` type can be confusing;
you should call a `String` a *string*, and a `str` a *string slice*.
Remember that you can modify a *string*, but you cannot modify a *string slice*.

In the code below, the characters in the `str` (*string slice*) are in the source code, and so get
compiled into the binary.

{% highlight rust %}
// A string slice is immutable.
// A string slice cannot be modified after it is created.
let my_str = "Many good characters.";
{% endhighlight %}

In this example, `my_str` is a *string slice* that points somewhere in
memory, and you cannot modify it. A `String` can be changed (if it stays in scope),
but a `str` (*string slice*) is immutable and cannot be changed.

----

You can experiment with Rust at the [Rust playground](https://play.rust-lang.org/).
