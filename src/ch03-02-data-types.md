## Data Types

ทุกๆค่าใน Rust เป็นส่วนหนึ่งของ *data type* เพื่อบอกให้ Rust รู้ว่าข้อมูลเป็นประเภทใด มันจะได้รู้ว่าจะทำงานยังไงกับข้อมูลนั้น และเราจะมาพิจารณากันถึงตัวแปรสองชนิดคือแบบ scalar และแบบ compound

พึงระลึกไว้เสมอว่า Rust เป็นภาษาแบบ *statically typed* แปลว่ามันจะต้องรู้ type ของทุกๆตัวแปรเสมอตอนคอมไพล์ และเพราะว่าคอมไพเลอร์มันมีความสามารถเดา type ได้จากค่าที่เราใช้ ในกรณีที่มีความเป็นไปได้หลาย type เช่นเมื่อเราแปลงค่าจาก `String` ไปเป็นตัวเลขด้วยการใช้ `parse` ในเรื่อง [“Comparing the Guess to the Secret
Number”][comparing-the-guess-to-the-secret-number]<!-- ignore --> ในบทที่ 2 เราจำเป็นจะต้องอธิบาย type ให้มันด้วยแบบนี้:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

ถ้าเราไม่ใส่ type ให้มันตรงนี้ Rust จะแสดง error แบบนี้ เพื่อบอกให้รู้ว่าคอมไพเลอร์ต้องการข้อมูลเพิ่มเติมจากเราว่า type อะไรกันแน่ที่เราต้องการ:

```text
error[E0282]: type annotations needed
 --> src/main.rs:2:9
  |
2 |     let guess = "42".parse().expect("Not a number!");
  |         ^^^^^
  |         |
  |         cannot infer type for `_`
  |         consider giving `guess` a type
```

เราจะเห็นวิธีอธิบาย type เมื่อเจอ type อื่นๆที่ต่างกัน

### Scalar Types

type แบบ *scalar* เป็นตัวแทนของค่าเดี่ยวๆ ซึ่ง Rust มีอยู่ด้วยกัน สี่ scalar type ได้แก่ integers, floating-point, Booleans และ characters เหมือนๆกับในภาษาโปรแกรมมิ่งอื่นๆ เราไปดูกันเลยดีว่ามันทำงานยังไงใน Rust

#### Integer Types

*integer* เป็นตัวเลขจำนวนเต็ม เราเคยได้ใช้มันครั้งหนึ่งในบทที่ 2 ก็คือ `u32` type นี้แสดงว่าค่าที่จะมาเก็บได้ควรเป็นค่าตัวเลขจำนวนเต็มที่ไม่มีประจุ (ตัวเลขที่มีประจุจะเริ่มต้นด้วยตัว `i` แทนตัว `u`) ที่มีขนาด 32 bits ดังที่แสดงในตาราง 3-1 ว่ามี integer อะไรให้ใช้บ้างใน Rust และแต่ละตัวไม่ว่าจะเป็นแบบมีหรือไม่มีประจุ (ตัวอย่างเช่น `i16`) สามารถประกาศใช้เป็น type ให้ตัวเลขจำนวนเต็มได้ทั้งนั้น

<span class="caption">Table 3-1: Integer Types in Rust</span>

| Length  | Signed  | Unsigned |
|---------|---------|----------|
| 8-bit   | `i8`    | `u8`     |
| 16-bit  | `i16`   | `u16`    |
| 32-bit  | `i32`   | `u32`    |
| 64-bit  | `i64`   | `u64`    |
| 128-bit | `i128`  | `u128`   |
| arch    | `isize` | `usize`  |

ตัวแปรทั้งสองแบบ ไม่ว่าจะแบบมีประจุหรือไม่มีประจุ จะมีขนาดตามที่ระบุ *Signed* และ *unsigned* เป็นตัวระบุว่ามันเป็นเลขที่มีความเป็นไปได้ว่าจะมีค่าติดลบได้หรือไม่นั่นเอง ง่ายๆคือเลขที่มีค่าติดลบได้ก็ใช้ (signed) หรือถ้ามีแต่ค่าบวกล้วนๆก็ใช้ (unsigned) แบบเดียวกับที่เราเขียนมันลงบนกระดาษนั่นแหล่ะ: เมื่อไหร่ที่ประจุสำคัญ เราก็ใส่สัญลักษณ์ บวก หรือ ลบลงไป ซึ่งบางทีเราก็ไม่ใส่สัญลักษณ์ให้มันเลยก็ได้ ในกรณีที่เรากล่าวถึงค่าที่เป็นบวก และวิธีการเก็บค่าที่มีประจุ เราใช้วิธี [two’s complement](https://en.wikipedia.org/wiki/Two%27s_complement)

ค่ามีประจุแต่ละตัวสามารถเก็บได้ตั้งแต่ -(2<sup>n - 1</sup>) ถึง 2<sup>n - 1</sup> - 1 เมื่อ *n* เท่ากับจำนวน bits ที่ตัวแปรนั้นใช้ ดังนั้น `i8` จะสามารถเก็บเลขได้ตั้งแต่ -(2<sup>7</sup>) ถึง 2<sup>7</sup> - 1 ซึ่งเท่ากับ -128 ถึง 127 ส่วนเลขที่ไม่มีประจุจะสามารถเก็บได้ตั้งแต่ 0 ถึง 2<sup>n</sup> - 1 เช่น `u8` จะสามารถเก็บเลขตั้งแต่ 0 ถึง 2<sup>8</sup> - 1 ซึ่งเท่ากับ 0 ถึง 255 นั่นเอง

เพิ่มเติมอีกนิด ชนิดตัวแปร `isize` และ `usize` ขึ้นอยู่กับคอมพิวเตอร์ที่คุณใช้รันโปรแกรมด้วยนะ: 64 bits ถ้าคุณรันบน สถาปัตยกรรม 64-bit และ 32 bits ถ้าคุณอยู่บนสถาปัตยกรรม 32-bit

คุณสามารถเขียนรูปแบบตัวเลขได้ในหลายแบบตามที่แสดงในตาราง 3-2 และมีข้อยกเว้นว่าเฉพาะตัวเลขแบบ byte ชนิดเดียวเท่านั้นที่ยอมให้เขียน type ต่อท้ายได้ เช่น `57u8` และเครื่องหมาย `_` ใช้เป็นตัวคั่นตัวเลขได้เช่น `1_000`

<span class="caption">Table 3-2: Integer Literals in Rust</span>

| Number literals  | Example       |
|------------------|---------------|
| Decimal          | `98_222`      |
| Hex              | `0xff`        |
| Octal            | `0o77`        |
| Binary           | `0b1111_0000` |
| Byte (`u8` only) | `b'A'`        |

แล้วคุณจะรู้ได้อย่างไรว่าจะใช้ integer แบบไหนดี? ถ้าคุณไม่แน่ใจ การปล่อยให้ Rust เลือกให้ก็น่าจะดีเหมือนกัน และปกติ Rust จะเลือก `i32` ให้ก่อน เพราะมันเร็วที่สุด แม่ว่าคุณจะอยู่บนระบบที่เป็น 64-bit ก็ตาม แต่ในสถานการณ์หลัก เวลาที่คุณเลือกใช้ `isize` หรือ `usize` แปลว่าคุณต้องการจะจัดกลุ่มมันให้ชัดเจนนั่นเอง

> ##### Integer Overflow
>
> Let’s say you have a variable of type `u8` that can hold values between 0 and 255.
> If you try to change the variable to a value outside of that range, such
> as 256, *integer overflow* will occur. Rust has some interesting rules
> involving this behavior. When you’re compiling in debug mode, Rust includes
> checks for integer overflow that cause your program to *panic* at runtime if
> this behavior occurs. Rust uses the term panicking when a program exits with
> an error; we’ll discuss panics in more depth in the [“Unrecoverable Errors
> with `panic!`”][unrecoverable-errors-with-panic]<!-- ignore --> section in
> Chapter 9.
>
> When you’re compiling in release mode with the `--release` flag, Rust does
> *not* include checks for integer overflow that cause panics. Instead, if
> overflow occurs, Rust performs *two’s complement wrapping*. In short, values
> greater than the maximum value the type can hold “wrap around” to the minimum
> of the values the type can hold. In the case of a `u8`, 256 becomes 0, 257
> becomes 1, and so on. The program won’t panic, but the variable will have a
> value that probably isn’t what you were expecting it to have. Relying on
> integer overflow’s wrapping behavior is considered an error. If you want to
> wrap explicitly, you can use the standard library type [`Wrapping`][wrapping].

#### Floating-Point Types

Rust also has two primitive types for *floating-point numbers*, which are
numbers with decimal points. Rust’s floating-point types are `f32` and `f64`,
which are 32 bits and 64 bits in size, respectively. The default type is `f64`
because on modern CPUs it’s roughly the same speed as `f32` but is capable of
more precision.

Here’s an example that shows floating-point numbers in action:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

Floating-point numbers are represented according to the IEEE-754 standard. The
`f32` type is a single-precision float, and `f64` has double precision.

#### Numeric Operations

Rust supports the basic mathematical operations you’d expect for all of the
number types: addition, subtraction, multiplication, division, and remainder.
The following code shows how you’d use each one in a `let` statement:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;

    // remainder
    let remainder = 43 % 5;
}
```

Each expression in these statements uses a mathematical operator and evaluates
to a single value, which is then bound to a variable. Appendix B contains a
list of all operators that Rust provides.

#### The Boolean Type

As in most other programming languages, a Boolean type in Rust has two possible
values: `true` and `false`. Booleans are one byte in size. The Boolean type in
Rust is specified using `bool`. For example:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```

The main way to use Boolean values is through conditionals, such as an `if`
expression. We’ll cover how `if` expressions work in Rust in the [“Control
Flow”][control-flow]<!-- ignore --> section.

#### The Character Type

So far we’ve worked only with numbers, but Rust supports letters too. Rust’s
`char` type is the language’s most primitive alphabetic type, and the following
code shows one way to use it. (Note that `char` literals are specified with
single quotes, as opposed to string literals, which use double quotes.)

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let c = 'z';
    let z = 'ℤ';
    let heart_eyed_cat = '😻';
}
```

Rust’s `char` type is four bytes in size and represents a Unicode Scalar Value,
which means it can represent a lot more than just ASCII. Accented letters;
Chinese, Japanese, and Korean characters; emoji; and zero-width spaces are all
valid `char` values in Rust. Unicode Scalar Values range from `U+0000` to
`U+D7FF` and `U+E000` to `U+10FFFF` inclusive. However, a “character” isn’t
really a concept in Unicode, so your human intuition for what a “character” is
may not match up with what a `char` is in Rust. We’ll discuss this topic in
detail in [“Storing UTF-8 Encoded Text with Strings”][strings]<!-- ignore -->
in Chapter 8.

### Compound Types

*Compound types* can group multiple values into one type. Rust has two
primitive compound types: tuples and arrays.

#### The Tuple Type

A tuple is a general way of grouping together some number of other values
with a variety of types into one compound type. Tuples have a fixed length:
once declared, they cannot grow or shrink in size.

We create a tuple by writing a comma-separated list of values inside
parentheses. Each position in the tuple has a type, and the types of the
different values in the tuple don’t have to be the same. We’ve added optional
type annotations in this example:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

The variable `tup` binds to the entire tuple, because a tuple is considered a
single compound element. To get the individual values out of a tuple, we can
use pattern matching to destructure a tuple value, like this:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}
```

This program first creates a tuple and binds it to the variable `tup`. It then
uses a pattern with `let` to take `tup` and turn it into three separate
variables, `x`, `y`, and `z`. This is called *destructuring*, because it breaks
the single tuple into three parts. Finally, the program prints the value of
`y`, which is `6.4`.

In addition to destructuring through pattern matching, we can access a tuple
element directly by using a period (`.`) followed by the index of the value we
want to access. For example:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

This program creates a tuple, `x`, and then makes new variables for each
element by using their index. As with most programming languages, the first
index in a tuple is 0.

#### The Array Type

Another way to have a collection of multiple values is with an *array*. Unlike
a tuple, every element of an array must have the same type. Arrays in Rust are
different from arrays in some other languages because arrays in Rust have a
fixed length, like tuples.

In Rust, the values going into an array are written as a comma-separated list
inside square brackets:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}
```

Arrays are useful when you want your data allocated on the stack rather than
the heap (we will discuss the stack and the heap more in Chapter 4) or when
you want to ensure you always have a fixed number of elements. An array isn’t
as flexible as the vector type, though. A vector is a similar collection type
provided by the standard library that *is* allowed to grow or shrink in size.
If you’re unsure whether to use an array or a vector, you should probably use a
vector. Chapter 8 discusses vectors in more detail.

An example of when you might want to use an array rather than a vector is in a
program that needs to know the names of the months of the year. It’s very
unlikely that such a program will need to add or remove months, so you can use
an array because you know it will always contain 12 items:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

You would write an array’s type by using square brackets, and within the
brackets include the type of each element, a semicolon, and then the number of
elements in the array, like so:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Here, `i32` is the type of each element. After the semicolon, the number `5`
indicates the element contains five items.

Writing an array’s type this way looks similar to an alternative syntax for
initializing an array: if you want to create an array that contains the same
value for each element, you can specify the initial value, followed by a
semicolon, and then the length of the array in square brackets, as shown here:

```rust
let a = [3; 5];
```

The array named `a` will contain `5` elements that will all be set to the value
`3` initially. This is the same as writing `let a = [3, 3, 3, 3, 3];` but in a
more concise way.

##### Accessing Array Elements

An array is a single chunk of memory allocated on the stack. You can access
elements of an array using indexing, like this:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

In this example, the variable named `first` will get the value `1`, because
that is the value at index `[0]` in the array. The variable named `second` will
get the value `2` from index `[1]` in the array.

##### Invalid Array Element Access

What happens if you try to access an element of an array that is past the end
of the array? Say you change the example to the following code, which will
compile but exit with an error when it runs:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,panics
fn main() {
    let a = [1, 2, 3, 4, 5];
    let index = 10;

    let element = a[index];

    println!("The value of element is: {}", element);
}
```

Running this code using `cargo run` produces the following result:

```text
$ cargo run
   Compiling arrays v0.1.0 (file:///projects/arrays)
    Finished dev [unoptimized + debuginfo] target(s) in 0.31 secs
     Running `target/debug/arrays`
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is
 10', src/main.rs:5:19
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```

The compilation didn’t produce any errors, but the program resulted in a
*runtime* error and didn’t exit successfully. When you attempt to access an
element using indexing, Rust will check that the index you’ve specified is less
than the array length. If the index is greater than or equal to the array
length, Rust will panic.

This is the first example of Rust’s safety principles in action. In many
low-level languages, this kind of check is not done, and when you provide an
incorrect index, invalid memory can be accessed. Rust protects you against this
kind of error by immediately exiting instead of allowing the memory access and
continuing. Chapter 9 discusses more of Rust’s error handling.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[control-flow]: ch03-05-control-flow.html#control-flow
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[wrapping]: ../std/num/struct.Wrapping.html
