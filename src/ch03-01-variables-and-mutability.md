## Variables and Mutability

จากที่ได้กล่าวไปแล้วในบทที่ 2 ว่าโดยปกติตัวแปรจะเป็นแบบเปลี่ยนแปลงค่าไม่ได้ นี่เป็นหนึ่งในสิ่งที่ Rust พยายามทำให้คุณเขียนโค้ดให้ปลอดภัยและง่ายต่อการทำงานแบบคู่ขนานในแบบที่ Rust นำเสนอได้ง่ายขึ้น แต่คุณก็ยังสามารถใช้ตัวเลือกเพื่อทำให้ตัวแปร เปลี่ยนค่าได้อยู่ดี เรามาดูกันว่า Rust จะยั่วให้คุณชอบ ตัวแปรที่เปลี่ยนค่าไม่ได้นี้ อย่างไร และทำไมเราถึงต้องทำแบบนั้น และทำไมบางครั้งคุณถึงยังอยากมีทางเลือกอื่นอีก

เมื่อตัวแปรไม่สามารถเปลี่ยนค่าได้ หมายความว่า เมื่อค่าใดก็ตามถูกนำไปใส่ในชื่อใดแล้ว คุณไม่สามารถเปลี่ยนค่านั้นได้อีก เพื่ออธิบายสิ่งนี้ เรามาสร้างโปรเจ็คใหม่กัน ให้ชื่อว่า *variables* เอาไว้ในไดเร็คทอรี่ *projects* ด้วยการสั่งว่า `cargo new variables`

จากนั้นในไดเร็คทอรี่ *variables* ให้เปิดไฟล์ *src/main.rs* แล้วเอาโค้ดนี้ไปทับโดยยังไม่ต้องคอมไพล์:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

จากนั้นบันทึกไฟล์แล้วรันโปรแกรมด้วยคำสั่ง `cargo run` คุณควรได้รับ error ตามที่เห็นนี้:

```text
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         - first assignment to `x`
3 |     println!("The value of x is: {}", x);
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable
```

ตัวอย่างนี้แสดงให้เห็นว่าตัวคอมไพเลอร์จะช่วยคุณหา error ในโปรแกรมคุณได้อย่างไร แต่ถึงแม้ว่าการที่คอมไพเลอร์แสดงข้อความ error จะดูน่าผิดหวัง แต่มันก็แค่หมายความว่าโปรแกรมของคุณมันยังไม่ปลอดภัยพอให้คุณทำงานได้เท่านั้น มัน *ไม่ได้* หมายความว่าคุณเป็นโปรแกรมเมอร์ที่ไม่เก่ง เพราะแม้แต่พวกผู้คลั่งไคล้ Rust ที่มีประสบการณ์สูงๆก็ยังได้รับ error ตอนคอมไพล์อยู่เสมอ

ข้อความ error นี้แสดงให้คุณรู้ว่า คุณไม่สามารถใส่ให้ตัวแปร x สองครั้งได้ เพราะว่าคุณพยายามกำหนดค่าครั้งที่สองให้ตัวแปร `x` ซึ่งมันเป็นแบบเปลี่ยนแปลงค่าไม่ได้นั่นเอง

การได้รับ error ตอนคอมไพล์นี้มีส่วนสำคัญมาก เมื่อเราพยายามจะเปลี่ยนค่าที่เคยถูกกำหนดไปแล้วกับตัวแปรชนิดแก้ค่าไม่ได้ เพราะมันอาจทำให้เกิดบั๊กได้ง่ายๆเลย เพราะถ้าที่หนึ่งของโค้ดเราทำงานบนความเชื่อที่ว่าค่านี้จะไม่ถูกเปลี่ยนตลอดกาล และอีกส่วนของโค้ดพยายามจะไปแก้มัน ไอ้ส่วนแรกมันก็ไม่ได้คิดเผื่อไว้ด้วยว่าต้องทำยังไง ไอ้บั๊กประเภทนี้มันหายากมากๆ โดยเฉพาะเมื่อโค้ดสองส่วนนี้ จะเปลี่ยนค่านี้แค่ *บางโอกาส* อีกต่างหาก

ตัวคอมไพเลอร์ของ Rust รับประกันว่า เมื่อเราตั้งใจว่าค่าไหนจะไม่ถูกเปลี่ยน มันจะต้องไม่ถูกเปลี่ยนแน่นอน นั่นหมายความว่า เมื่อเวลาที่คุณกำลังอ่านและเขียนโค้ดอยู่ คุณไม่ต้องไปสนใจว่าค่านี้จะไปเปลี่ยนตอนไหน โค้ดคุณจะตรงไปตรงมา

แต่ว่า ตัวแปรที่แก้ค่าได้ ก็มีประโยชน์ อย่างที่เห็นในบทที่ 2 แล้วว่าโดยปกติตัวแปรจะแก้ค่าไม่ได้ แต่เราก็ทำให้มันแก้ค่าได้ด้วยการเพิ่ม `mut` ไปข้างหน้าตัวแปร และมันยังหมายความถึงว่าผู้ที่อ่านโค้ดนี้จะรู้ว่าต้องมีส่วนใดส่วนหนึ่งของโค้ดจะมาเปลี่ยนค่านี้

เพื่อแสดงตัวอย่าง ลองเปลี่ยน *src/main.rs* ตามนี้:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

เมื่อเรารันโปรแกรม เราจะได้:

```text
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
    Finished dev [unoptimized + debuginfo] target(s) in 0.30 secs
     Running `target/debug/variables`
The value of x is: 5
The value of x is: 6
```

เรายอมให้เปลี่ยนค่า `x` จาก `5` ไปเป็น `6` ได้ด้วยการใช้ `mut` ในบางกรณี คุณอาจจะต้องการให้ตัวแปร เปลี่ยนค่าได้ เพราะมันจะทำให้คุณเขียนโค้ดสะดวกขึ้นมากกว่าการใช้ตัวแปรที่เปลี่ยนค่าไม่ได้

มันมีหลายๆอย่างที่ต้องแลก ถ้าคุณต้องการป้องกันการเกิดบั๊ก ตัวอย่างเช่น ในกรณีที่คุณใช้ data structures ที่ใหญ่มากๆ การสร้างอินสแตนซ์ที่เปลี่ยนค่าได้ จะทำให้มันทำงานเร็วกว่าที่จะมาสำเนาของไปสร้างใหม่อยู่เรื่อยๆ ในขณะที่ถ้ามันเป็น data structures ที่เล็ก การสร้างอินสแตนซ์ใหม่และเขียนแบบ functional style อาจจะทำให้เราคิดได้ง่ายขึ้น นั่นหมายความว่า ขึ้นอยู่กับว่า คุณจะยอมให้ประสิทธิภาพตกลงเพื่อแลกกับความชัดเจนที่เพิ่มขึ้นหรือไม่

### Differences Between Variables and Constants

การที่เราพูดกันถึงตัวแปรที่ไม่สามารถเปลี่ยนค่าได้ อาจจะทำให้คุณไปนึกถึงหลักการของภาษาโปรแกรมมิ่งอื่นส่วนใหญ่ที่จะมี *constants* ซึ่งมันคล้ายกับตัวแปรที่แก้ค่าไม่ได้เลย เพราะ constant เป็นค่าที่ใส่เข้าไปในชื่อไหนแล้วก็เปลี่ยนค่าไม่ได้อีกต่อไป แต่มันก็มีความแตกต่างกันเล็กน้อยระหว่าง constants กับตัวแปร

อย่างแรก คุณไม่สามารถใช้ `mut` กับ constants ได้ เพระา constants ไม่ใช่แค่เปลี่ยนค่าไม่ได้โดยปกติ แต่มันเปลี่ยนค่าไม่ได้เลยด้วยซ้ำ

You declare constants using the `const` keyword instead of the `let` keyword,
and the type of the value *must* be annotated. We’re about to cover types and
type annotations in the next section, [“Data Types,”][data-types]<!-- ignore
--> so don’t worry about the details right now. Just know that you must always
annotate the type.

Constants can be declared in any scope, including the global scope, which makes
them useful for values that many parts of code need to know about.

The last difference is that constants may be set only to a constant expression,
not the result of a function call or any other value that could only be
computed at runtime.

Here’s an example of a constant declaration where the constant’s name is
`MAX_POINTS` and its value is set to 100,000. (Rust’s naming convention for
constants is to use all uppercase with underscores between words, and
underscores can be inserted in numeric literals to improve readability):

```rust
const MAX_POINTS: u32 = 100_000;
```

Constants are valid for the entire time a program runs, within the scope they
were declared in, making them a useful choice for values in your application
domain that multiple parts of the program might need to know about, such as the
maximum number of points any player of a game is allowed to earn or the speed
of light.

Naming hardcoded values used throughout your program as constants is useful in
conveying the meaning of that value to future maintainers of the code. It also
helps to have only one place in your code you would need to change if the
hardcoded value needed to be updated in the future.

### Shadowing

As you saw in the guessing game tutorial in the [“Comparing the Guess to the
Secret Number”][comparing-the-guess-to-the-secret-number]<!-- ignore -->
section in Chapter 2, you can declare a new variable with the same name as a
previous variable, and the new variable shadows the previous variable.
Rustaceans say that the first variable is *shadowed* by the second, which means
that the second variable’s value is what appears when the variable is used. We
can shadow a variable by using the same variable’s name and repeating the use
of the `let` keyword as follows:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    let x = x * 2;

    println!("The value of x is: {}", x);
}
```

This program first binds `x` to a value of `5`. Then it shadows `x` by
repeating `let x =`, taking the original value and adding `1` so the value of
`x` is then `6`. The third `let` statement also shadows `x`, multiplying the
previous value by `2` to give `x` a final value of `12`. When we run this
program, it will output the following:

```text
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
    Finished dev [unoptimized + debuginfo] target(s) in 0.31 secs
     Running `target/debug/variables`
The value of x is: 12
```

Shadowing is different from marking a variable as `mut`, because we’ll get a
compile-time error if we accidentally try to reassign to this variable without
using the `let` keyword. By using `let`, we can perform a few transformations
on a value but have the variable be immutable after those transformations have
been completed.

The other difference between `mut` and shadowing is that because we’re
effectively creating a new variable when we use the `let` keyword again, we can
change the type of the value but reuse the same name. For example, say our
program asks a user to show how many spaces they want between some text by
inputting space characters, but we really want to store that input as a number:

```rust
let spaces = "   ";
let spaces = spaces.len();
```

This construct is allowed because the first `spaces` variable is a string type
and the second `spaces` variable, which is a brand-new variable that happens to
have the same name as the first one, is a number type. Shadowing thus spares us
from having to come up with different names, such as `spaces_str` and
`spaces_num`; instead, we can reuse the simpler `spaces` name. However, if we
try to use `mut` for this, as shown here, we’ll get a compile-time error:

```rust,ignore,does_not_compile
let mut spaces = "   ";
spaces = spaces.len();
```

The error says we’re not allowed to mutate a variable’s type:

```text
error[E0308]: mismatched types
 --> src/main.rs:3:14
  |
3 |     spaces = spaces.len();
  |              ^^^^^^^^^^^^ expected &str, found usize
  |
  = note: expected type `&str`
             found type `usize`
```

Now that we’ve explored how variables work, let’s look at more data types they
can have.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[data-types]: ch03-02-data-types.html#data-types
