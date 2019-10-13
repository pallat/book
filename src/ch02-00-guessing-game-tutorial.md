# Programming a Guessing Game

เรามาเริ่มต้นด้วยการทำสักหนึ่งโปรเจ็คไปพร้อมๆกันเลย และในบทนี้จะเป็นการนำคุณเข้าสู่แนวคิดเบื่้องต้นของ Rust และวิธีใช้งานในชีิวิตจริงกัน คุณจะได้เรียนรู้เกี่ยวกับ `let`, `match`, methods, associated
functions, การใช้ crates จากภายนอก, และอีกมากมาย แล้วบทถัดไปเราจะไปสำรวจมันเพิ่มเติมในรายละเอียด แต่ในส่วนของบทนี้ เราจะมาเริ่มฝึกพื้นฐานกันก่อน

เรากำลังจะสร้างโปรแกรมเพื่อแก้โจทย์พื้นๆ: เกมเดาเลข ซึ่งการทำงานของมันจะเริ่มจากสุ่มตัวเลขมาหนึ่งค่าที่อยู่ระหว่าง 1 และ 100 และคอยรับค่าจากผู้เล่น ให้เดาค่าไปเรื่อยๆ เมื่อโปรแกรมได้รับค่ามา จะนำไปเทียบว่าค่าที่เดามานั้น สูงหรือต่ำว่าคำตอบ ถ้าผู้เล่นเดาได้ถูกต้อง เกมจะแสดงข้อความแสดงความยินดีและจบโปรแกรม

## Setting Up a New Project

ในการเริ่มโปรเจ็ค ให้เราไปที่ไดเร็คทอรี่ *projects* ที่เราสร้างไว้เมื่อบทที่ 1 จากนั้นสร้างโปรเจ็คด้วย Cargo ตามนี้เลย:

```text
$ cargo new guessing_game
$ cd guessing_game
```

คำสั่งแรก `cargo new` จะรับชื่อโปรเจ็คเป็นอากิวเมนต์แรก ส่วนคำสั่งที่สองเป็นการย้ายตัวเองเข้าไปอยู่ในไดเร็คทอรี่ของโปรเจ็คนั้น

ลองไปดูที่ไฟล์ *Cargo.toml* ที่ถูกสร้างขั้นมาสักหน่อย:

<span class="filename">Filename: Cargo.toml</span>

```toml
[package]
name = "guessing_game"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2018"

[dependencies]
```

ถ้าข้อมูลผู้เขียนที่ Cargo ดึงมาจาก environment ไม่ถูกต้อง ก็ให้แก้ไขแล้วก็บันทึกไฟล์ได้เลย

ถ้ายังจำได้ในบทที่ 1 คำสั่ง `cargo new` จะสร้างโปรแกรม Hello, world! มาให้ทันทีที่ *src/main.rs*:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

ทีนี้เรามาลองคอมไพล์โปรแกรม Hello, world! แล้วลองรันดู ด้วยคำสั่ง `cargo run` ตามนี้:

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.50 secs
     Running `target/debug/guessing_game`
Hello, world!
```

จะเห็นว่าคำสั่ง `run` นี้สะดวกมากๆเวลาที่เราอยากจะทำงานเร็วๆ แบบที่เราจะทำในเกมนี้กัน มันช่วยให้เราทดสอบเร็วๆ ก่อนจะทำขั้นตอนต่อไป

เปิดไฟล์ *src/main.rs* อีกครั้ง ทีนี้เราจะมาเขียนโค้ดกันแล้ว

## Processing a Guess

ส่วนแรกของโปรแกรมคือการขึ้นข้อความเเพื่อรอผู้ใช้ใส่ตัวเลข จากนั้นค่อยเอาตัวเลขนั้นไปตรวจสอบ ทีนี้เราลองมาเริ่มที่การทำให้ผู้ใช้ใส่ข้อมูลเข้ามาได้ก่อน ด้วยการเขียนโค้ดตามที่เห็นใน Listing 2-1 ลงไปในไฟล์ *src/main.rs* กันเลย

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

<span class="caption">Listing 2-1: Code that gets a guess from the user and
prints it</span>

โค้ดนี้มีข้อมูลเยอะเลย เรามาดูกันทีละบรรทัดไปด้วยกัน การที่จะรับข้อมูลจากผู้เล่น แล้วพิมพ์คำตอบออกไป เราจำเป็นต้องใช้ไลบรารี่ชื่อ `io` (input/output) เข้ามาในโปรเจ็ค โดย `io` เป็นไลบรารี่ที่มาพร้อมกับไลบรารี่มาตรฐาน (เราอาจจะเคยรู้จักในชื่อ `std`):

```rust,ignore
use std::io;
```

โดยปกติ Rust จะให้ไลบรารี่บางอย่างเข้าไปในทุกๆโปรแกรมเป็นค่าตั้งต้นเรียกว่า [the *prelude*][prelude]<!-- ignore --> ถ้าคุณต้องการใช้ของที่ไม่ได้มากับ prelude คุณจะต้องนำมันเข้ามาด้วยตัวเองด้วยการประกาศ `use` การนำไลบรารี่ `std::io` เข้ามาจะทำให้คุณใช้ของได้อีกเพียบ รวมไปถึงความสามารถในการรับข้อมูลจากผู้เล่น

[prelude]: ../std/prelude/index.html

เราเคยได้เห็นฟังก์ชั่น `main` กันมาแล้วในบทที่ 1 ว่ามันเป็นจุดเริ่มต้นการทำงานของทุกสิ่ง:

```rust,ignore
fn main() {
```

`fn` เป็นคำที่ใช้ประกาศฟังก์ชั่นใหม่ โดยมีวงเล็บ `()` บอกว่าไม่ได้รับพารามิเตอร์ใดๆ แล้วครอบเนื้อฟังก์ชั่นด้วยวงเล็บปีกกา `{` หลังจากวงเล็บปีกกาเปิดจะเป็นส่วนของการทำงานในฟังก์ชั่น

เราได้รู้แล้วว่า `println!` เป็นมาโคร จากบทที่ 1 โดยมันจะพิมพ์ข้อความออกไปทางหน้าจอ:

```rust,ignore
println!("Guess the number!");

println!("Please input your guess.");
```

โค้ดส่วนนี้จะแสดงข้อความว่าต้องการให้ผู้เล่นทำอะไรต่อไป

### Storing Values with Variables

จากนั้นเราจะสร้างของมาเก็บสิ่งที่ผู้เล่นใส่เข้ามา:

```rust,ignore
let mut guess = String::new();
```

ตรงนี้น่าสนใจมาก สังเกตว่า คำประกาศ `let` ใช้สำหรับสร้าง *variable* ลองดูอีกสักตัวอย่าง:

```rust,ignore
let foo = bar;
```

บรรทัดนี้เป็นการประกาศตัวแปรชื่อ `foo` และรับค่ามาจากตัวแปรที่ชื่อ `bar` โดยในภาษา Rust ตัวแปรจะเป็นชนิดที่แก้ไขไม่ได้โดยดีฟอลต์ เดี๋ยวเราจะมาคุยกันในรายละเอียดเรีื่องนี้ต่อกันที่ [“Variables and
Mutability”][variables-and-mutability]<!-- ignore --> ในบทที่ 3 ซึ่งจะมีตัวอย่างแสดงให้เห็นการใช้ `mut` นำหน้าตัวแปร เพื่อทำให้ตัวแปรนั้นเปลี่ยนค่าได้:

```rust,ignore
let foo = 5; // immutable
let mut bar = 5; // mutable
```

> สังเกต: สัญลักษณ์ `//` เป็นการคอมเม้นต์โค้ดหลังสัญลักษณ์นี้ไปจนถึงจบบรรทัด
> Rust จะไม่สนใจของที่อยู่ในคอมเม้นต์ โดยเราจะมาถกประเด็นนี้ต่อกันในรายละเอียดในบทที่ 3

ตอนนี้เรากลับมาที่โปรแกรมเกมของเรากันดีกว่า จากที่ได้บอกไปแล้วว่า `let mut guess` จะสร้างตัวแปร `guess` ที่สามารถเปลี่ยนค่าได้ โดยกำหนดค่าด้วยเครื่องหมายเท่ากับ (`=`) เป็นค่าที่ได้จากการเรียกฟังก์ชั่น `String::new` ที่จะคืน `String` มาให้ตัวหนึ่ง [`String`][string]<!-- ignore --> เป็น type ที่มาพร้อมไลบรารี่มาตรฐานเข้ารหัสแบบ UTF-8

[string]: ../std/string/struct.String.html

สัญลักษณ์ `::` ใน `::new` บอกให้รู้ว่า `new` เป็น *associated
function* ของ `String` และ associated function สร้างไว้บน type ซึ่งในกรณีนี้คือ `String` ในบางภาษาจะเรียกสิ่งนี้ว่า *static method*

ฟังกํชั่น `new` จะสร้างสตริงเปล่าๆขึ้นมาตัวหนึ่ง โดยปกติคุณจะเห็นว่าทุก type จะมีฟังก์ชั่น `new` มาด้วยเสมอ เพราะมันเป็นชื่อกลางๆ ที่เรามักจะใช้สร้างค่าอะไรใหม่ๆอยู่แล้ว

สรุปว่าบรรทัดของ `let mut guess = String::new();` เป็นการสร้างตัวแปรที่เปลี่ยนแปลงค่าได้ และผูกไว้กับ instance ของ `String` เฮ้อ!

ย้อนกลับไปตรงที่เราขอใช้ input/output จากไลบรารี่มาตรฐานด้วยการบอก `use std:io` ในบรรทักแรกของโปรแกรม ทีนี้เราจะเรียกใช้ฟังก์ชั่น `stdin` จากโมดูล `io`:

```rust,ignore
io::stdin().read_line(&mut guess)
    .expect("Failed to read line");
```

แต่ถ้าเรายังไม่ได้ใส่ `use std::io` ไว้ที่บรรทัดแรกๆของโปรแกรม เราสามารถเขียนแบบนี้แทนได้  `std::io::stdin` โดยฟังก์ชั่น `stdin` จะคืน instance ของ [`std::io::Stdin`][iostdin]<!-- ignore --> ซึ่งเป็น type ที่จะจัดการเกี่ยวกับมาตรฐานการรับค่าจากเทอมินัลนั่นเอง

[iostdin]: ../std/io/struct.Stdin.html

โค้ดในส่วนต่อมาคือ `.read_line(&mut guess)` การเรียกเมธอด [`read_line`][read_line]<!-- ignore --> เป็นการรอรับค่าจากผู้เล่น เราจึงเอาตัวแปร `&mut guess` ไปใส่ไว้ใน `read_line`

[read_line]: ../std/io/struct.Stdin.html#method.read_line

งานของ `read_line` คือการรับค่าอะไรก็ตามที่ผู้เล่นจะใส่เข้ามาเป็นสตริง เข้ามาเป็นอาร์กิวเมนต์ โดยเจ้าอาร์กิวเมนต์นี้จะต้องเป็นแบบที่เปลี่ยนค่าได้ เพื่อให้เมธอดเปลี่ยนค่าในนั้นเป็นค่าที่ผู้เล่นส่งเข้ามา

เครื่องหมาย `&` บอกให้รู้ว่าอาร์กิวเมนต์นั้นเป็นแบบ *reference* เป็นการเปิดช่องให้โค้ดจากหลายๆส่วน เป็นค่าเดิียวกันจากหน่วยความจำ โดยไม่ต้องสำเนาค่านั้นหลายๆครั้งในหน่วยความจำ ซึ่งเจ้า References นี่แหล่ะ เป็นส่วนหลักใน Rust ที่ซับซ้อนมากส่วนหนึ่ง เพราะมันจะต้องใช้ได้ง่ายและปลอดภัยด้วย แต่คุณไม่ต้องรู้รายละเอียดเกี่ยวกับมันมากนักหรอก ตอนนี้สิ่งที่คุณต้องสนใจคือตัวแปรที่เป็น references จะเป็นตัวแปรที่แก้ไขค่าไม่ได้เป็นค่าตั้งต้น และด้วยเหตุนี้ คุณจึงต้องเขียน `&mut guess` แทนที่จะเขียนว่า `&guess` เพื่อทำให้มันแก้ไขค่าได้นั่นเอง (เดี๋ยวเราจะมาอธิบายเรื่อง references กันอีกทีในบทที่ 4)

### Handling Potential Failure with the `Result` Type

เรายังไม่จบกับบรรทัดนี้ แม้ว่าเราจะคุยกันในแต่ละบรรทัดกันจริงจังมากแล้ว เราเพิ่งจะเล่าไปแค่ส่วนเดียวของบรรทัดนี้เอง และส่วนที่สองของมันคือเมธอดนี้:

```rust,ignore
.expect("Failed to read line");
```

ตอนที่เราเรียกเมธอด `.foo()` แบบนี้ มันฉลาดพอที่จะขึ้นบรรทัดใหม่ให้พร้อมกับเว้นวรรคเพื่อไม่ให้ยาวเกินไปแบบนี้:

```rust,ignore
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

ด้วยเหตุว่าถ้าบรรทัดไหนมันยาว จะทำให้อ่านยาก ซึ่งมันจะดีกว่าถ้าแบ่งเป็นสองบรรทัดซะ เอาละ ทีนี้เรามาดูต่อกันดีกว่าว่าบรรทัดนี้มันทำอะไร

จากที่เราได้บอกไปก่อนหน้านี้แล้วว่า `read_line` จะส่งสิ่งที่ผู้เล่นพิมพ์เข้ามาเป็นสตริง ในขณะเดียวกันมันก็จะคืนค่ากลับมาให้ด้วย ซึ่งในที่นี้มันคือ [`io::Result`][ioresult]<!-- ignore --> โดยที่ Rust เองก็มี type ที่ชื่อว่า `Result` อยู่พอสมควรในไลบรารี่มาตรฐาน: [`Result`][result]<!-- ignore --> ที่เป็น generic จะเหมือนกันกับตัวที่แทรกตามโมดูลต่างๆ เช่นใน `io::Result`

[ioresult]: ../std/io/type.Result.html
[result]: ../std/result/enum.Result.html

type `Result` นั้นเป็น [*enumerations*][enums]<!-- ignore --> ซึ่งบ่อยครั้งเราจะเห็นในชื่อ *enums* โดยเจ้า enumeration นี้เป็น type ที่สามารถเก็บเซ็ตของค่าคงที่ได้ และค่าพวกนั้นจะถูกเรียกว่า *variants* ของ enum โดยในบทที่ 6 จะมีรายละเอียดเกี่ยวกับ enum อีกที

[enums]: ch06-00-enums.html

สำหรับ `Result` จะมีค่า variants คือ `Ok` หรือ `Err` โดยถ้าเป็น `Ok` จะหมายความว่าการทำงานนั้นสำเร็จ และในนั้นจะมีค่าต่างๆที่บอกว่าทำสำเร็จอย่างไร ส่วน `Err` จะหมายถึงว่า การทำงานนั้นล้มเหลว และจะมีรายละเอียดถึงสาเหตุว่าทำไมถึงล้มเหลวมาให้ด้วย

ประโยชน์ของ `Result` คือการแปลข้อความจาก error คือว่า ค่าที่ได้จาก `Result` เนี่ย มันก็ไม่ต่างจากค่าที่ได้จาก type อื่นๆ คือมันมีเมธอดหลายตัวให้ใช้ เช่น อินสแตนซ์ของ `io::Result` จะมีเมธอด [`expect` method][expect]<!-- ignore --> ให้เรียกใช้ ซึ่งถ้าเจ้า อินสแตนซ์ของ `io::Result` เป็นค่า `Err` จะทำให้ `expect` พังและแสดงข้อความที่เราส่งเป็นอาร์กิวเมนต์ให้ `expect` ออกมา กลับมาที่ `read_line` ถ้ามันคืน `Err` เราก็จะได้ผลลัพธ์ที่เป็น error กลับมา แต่ถ้าอินสแตนซ์ของ `io::Result` ได้เป็นค่า `Ok` แทน `expect` ก็จะรับเอาผลลัพธ์กลับมาให้เรานำไปใช้ต่อได้ โดยในตัวอย่างนี้ ค่าที่ได้กลับมาก็คือจำนวนไบต์ที่ผู้เล่นพิมพ์เข้ามา

[expect]: ../std/result/enum.Result.html#method.expect

ถ้าคุณไม่เรียก `expect` คุณก็สามารถคอมไพล์โปรแกรมได้นะ แต่คุณจะได้รับคำเตือนแบบนี้:

```text
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
warning: unused `std::result::Result` which must be used
  --> src/main.rs:10:5
   |
10 |     io::stdin().read_line(&mut guess);
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: #[warn(unused_must_use)] on by default
```

Rust เตือนให้คุณรู้ว่า คุณไม่ได้รับ `Result` กลับมาจาก `read_line` ซึ่งหมายความว่าโปรแกรมของคุณจะไม่รู้เมื่อเกิด error ขึ้นด้วย

ความจริงคือคุณต้องเชื่อคำเตือนแล้วจัดการรับ error มาแก้ไขสถานการณ์ซะ แต่เพราะว่าคุณแค่อยากจะให้โปรแกรมมันพังเมื่อเกิดปัญหา คุณก็แค่ใช้ `expect` ซึ่งคุณจะได้เรียนรู้เกี่ยวกับการ กู้สถานการณ์จาก error ได้ในบทที่ 9

### Printing Values with `println!` Placeholders

ก่อนนจะถึงวงเล็บปิด ยังเหลืออีกหนึ่งบรรทัดที่เราจะกล่าวถึง:

```rust,ignore
println!("You guessed: {}", guess);
```

บรรทัดนี้จะพิมพ์สตริงว่าเราได้บันทึกสิ่งที่ผู้เล่นใส่เข้ามาเรียบร้อยแล้ว โดยชุดของวงเล็บก้ามปูเปิดปิด `{}` คือตัวแทนของค่าตัวแปรด้านหลัง ให้คิดซะว่ามันเหมือนก้ามปูที่เอาไว้หนีบค่าเอาไว้ โดยคุณจะพิมพ์มันออกไปกี่ค่าก็ได้ด้วยการใช้เครื่องหมายนี้: โดยก้ามปูแรกจะแทนด้วยค่าแรกหลังฟอร์แมตนี้ ก้ามปูที่สองก็แทนด้วยค่าที่สองและสามต่อไปเรื่อยๆถ้ามี โดยการพิมพ์ค่าออกไปหลายๆค่าด้วย `println!` จะทำลักษณะนี้:

```rust
let x = 5;
let y = 10;

println!("x = {} and y = {}", x, y);
```

โค้ดนี้จะพิมพ์ข้อความว่า `x = 5 and y = 10`.

### Testing the First Part

ถึงเวลาทดสอบโปรแกรมด้วยการรันคำสั่ง `cargo run`:

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```

ถึงจุดนี้ ขั้นตอนแรกของโปรแกรมเราก็สำเร็จ: เราสามารถรับค่าจากคีย์บอร์ดแล้วพิมพ์มันออกมาดูได้แล้ว

## Generating a Secret Number

ต่อไป เรามาสร้างตัวเลขลับที่จะให้ผู้เล่นเดากัน ด้วยการทำให้มันไม่ซ้ำกันในแต่ละครั้งที่เล่น เพื่อความสนุก ด้วการสุ่มตัวเลขระหว่าง 1 ถึง 100 เกมส์จะได้ไม่ยากมากเกินไป เนื่องด้วยการสุ่มตัวเลขมันไม่ได้ถูกรวมมากับไลบรารี่พื้นฐาน Rust แต่ทีม Rust ก็ทำไว้ให้ที่ [`rand` crate][randcrate]

[randcrate]: https://crates.io/crates/rand

### Using a Crate to Get More Functionality

ถ้ายังจำได้ crate คือชุดรวบรวมไฟล์ซอสโค้ดของ Rust เช่นโปรเจ็คที่เราทำกันนี่จะเรียกว่า *binary crate* หมายถึงโปรแกรมที่ถูกเรียกใช้ได้ทันที ส่วน crate ของ `rand` เป็น *library crate* หมายถึงโค้ดที่สามารถถูกเรียกใช้จากโปรแกรมอื่นได้

การใช้ Cargo กับ crate จากภายนอกจะต้องเป็นที่รู้จักจริงๆ และก่อนที่จะใช้ `rand` ได้ เราต้องแก้ไขไฟล์ *Cargo.toml* เสียก่อน ด้วยการเพิ่ม crate `rand` เข้าไปใน dependency ทำได้ง่ายๆด้วยการเปิดไฟล์มาเพิ่มหนึ่งบรรทัดภายใต้ส่วน `[dependencies]` แบบนี้:

<span class="filename">Filename: Cargo.toml</span>

```toml
[dependencies]

rand = "0.3.14"
```

ในไฟล์ *Cargo.toml* ทุกๆอย่างจะอยู่ภายใต้หัวข้อของแต่ละส่วนไปเรื่อยๆจนกว่าจะถึงหัวข้อใหม่ และในหัวข้อ `[dependencies]` เป็นส่วนที่เอาไว้ให้คุณบอก Cargo ว่าโปรเจ็คคุณต้องการเรียกใช้ crate อะไรบ้าง และเวอร์ชั่นอะไร ด้วยการบอกเป็น semantic version แบบเจาะจงไปเลยเช่น `0.3.14` ซึ่ง Cargo จะเข้าใจ [Semantic Versioning][semver]<!-- ignore --> อยู่แล้ว (บางครั้งจะเรียกย่อๆว่า *SemVer*) เป็นมาตรฐาน และสามารถใช้รูปย่อแบบนี้ `^0.3.14` เพื่อบอกว่า “สามารถใช้เวอร์ชั่นไหนก็ได้ที่สอดคล้องกับเวอร์ชั่น 0.3.14”

[semver]: http://semver.org

ตอนนี้ โดยไม่ต้องแก้ไขโค้ดเราลองมาบิวด์โปรเจ็คกันตามที่แสดงให้เห็นใน Listing 2-2

```text
$ cargo build
    Updating registry `https://github.com/rust-lang/crates.io-index`
 Downloading rand v0.3.14
 Downloading libc v0.2.14
   Compiling libc v0.2.14
   Compiling rand v0.3.14
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
```

<span class="caption">Listing 2-2: The output from running `cargo build` after
adding the rand crate as a dependency</span>

คุณอาจจะเห็นเวอร์ชั่นที่ไม่ตรงกันก็ได้ (แต่มันจะสอดคล้องกับสิ่งที่โค้ดเรียกใช้ได้, ขอบคุณ SemVer!) และอาจจะสลับบรรทัดกันก็ไม่ผิด

ตอนนี้เรามี dependency จากภายนอกใช้ละ Cargo จะไปดึงเวอร์ชั่นล่าสุดของทุกๆอย่างจาก *registry*  ที่สำเนาข้อมูลมาจาก [Crates.io][cratesio] อีกที โดย Crates.io เป็นที่ ที่ผู้คนที่ใช้ Rust นำเอาโค้ดมาแบ่งปันให้คนอื่นๆได้ใช้กัน

[cratesio]: https://crates.io/

หลังจากที่ได้ปรับปรุง registry กันไปแล้ว Cargo จะตรวจสอบในส่วน `[dependencies]` และดาวน์โหลด crate ที่คุณยังไม่เคยมีมาก่อนลงมา ซึ่งในกรณีนี้เรามีแค่ `rand` ตัวเดียวโดย Cargo จะไปหยิบ `libc` มาด้วย เพราะว่า `rand` อ้างอิงไปใช้ `libc` อีกทีเพื่อทำงาน หลังจากดาวน์โหลดมาเสร็จแล้ว Rust จะคอมไพล์มันและจากนั้นค่อยคอมไพล์โปรเจ็ค

ถ้าคุณลองรัน `cargo build` ดูอีกทีโดยไม่ได้แก้ไขอะไรเลย คุณจะไม่เห็นข้อความ `Finished` แล้ว เพราะ Cargo รู้ว่าได้ดาวน์โหลดและคอมไพล์ dependency แล้ว และคุณไม่ได้แก้ไขอะไรในไฟล์ *Cargo.toml* แต่อย่างใด มันจึงไม่ต้องคอมไพล์ซ้ำ

ถ้าคุณเปิดไฟล์ *src/main.rs* ขึ้นมาแล้วแก้อะไรซักนิดดู พอบันทึกไฟล์แล้วบิวด์ซ้ำ คุณจะเห็นสองบรรทัดนี้ออกมา:

```text
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
```

สองบรรทัดนี้แสดงให้เห็นว่า Cargo ปรับปรุงเฉพาะส่วนที่คุณแก้ไขเล็กน้อยในไฟล์ *src/main.rs* โดยคุณไม่ได้แก้ไข dependencies อะไรเลย Cargo รู้ว่ามันสามารถใช้ของที่มีอยู่แล้วได้เลย มันจึงแค่บิวด์ซ้ำเฉพาะโค้ดของคุณเพียงอย่างเดียว

#### Ensuring Reproducible Builds with the *Cargo.lock* File

Cargo มีกลไกที่จะมั่นใจได้ว่าคุณจะได้ของที่เหมือนเดิมเสมอ ไม่ว่าคุณหรือใครก็ตาม มาบิวด์โค้ดของคุณ เพราะ Cargo จะใช้แต่เวอร์ชั่นของ dependencies ที่คุณระบุไว้จนกว่าคุณจะเปลี่ยนมัน ตัวอย่างเช่น จะเกิดอะไรขึ้นถ้าสัปดาห์หน้า crate `rand` มีเวอร์ชั่นใหม่ 0.3.15 ออกมา และมันได้แก้ไขบั๊กที่สำคัญมากๆแต่มันจะทำให้โค้ดคุณพัง

คำตอบของปัญหานี้คือไฟล์ *Cargo.lock* ที่ถูกสร้างไว้ตั้งแต่ตอนที่คุณรัน `cargo build` ที่อยู่ในไดเร็คทอรี่ *guessing_game* โดยเมื่อตอนที่คุณบิวด์โปรเจ็คครั้งแรก Cargo จะตรวจสอบเวอร์ชั่นของทุกๆ dependencies ให้ตรงกับเงื่อนไขและเขียนมันลงไปในไฟล์ *Cargo.lock* หลังจากนั้น ทุกครั้งที่คุณเบิวด์โปรเจ็ค Cargo จะไปดูเวอร์ชั่นใน *Cargo.lock* แทนที่จะไปตรวจสอบใหม่ทุกครั้ง สิ่งนี้จะทำให้คุณได้ของเหมือนเดิมโดยอัตโนมัติ หรือในอีกทางหนึ่ง มันจะคงใช้เวอร์ชั่น `0.3.14` ไปจนกว่าคุณจะจงใจอัพเกรดมันเอง ขอบคุณไฟล์ *Cargo.lock*

#### Updating a Crate to Get a New Version

เมื่อใดที่คุณอยากที่จะอัพเดท crate (จริงๆ) Cargo ได้เตรียมคำสั่ง `update` ไว้ให้แล้ว โดยมันจะไม่สนใจไฟล์ *Cargo.lock* และไปตรวจสอบเวอร์ชั่นล่าสุดใหม่หมด ตามที่คุณระบุไว้ใน *Cargo.toml* และถ้ามันใช้แทนกันได้ Cargo จะเขียนเวอร์ชั่นนั้นลงไปในไฟล์ *Cargo.lock* ใหม่ให้

แต่โดยปกติ Cargo จะมองหาเวอร์ชั่นที่มากกว่า `0.3.0` และน้อยกว่า `0.4.0` ถ้า `rand` เกิดมีสองเวอร์ชั่นนี้ขึ้นมาคือ `0.3.15` กับ `0.4.0` คุณจะเห็นแบบนี้ถ้าคุณรัน `cargo update`:

```text
$ cargo update
    Updating registry `https://github.com/rust-lang/crates.io-index`
    Updating rand v0.3.14 -> v0.3.15
```

ตอนนี้คุณจะสังเกตว่าไฟล์ *Cargo.lock* จะเปลี่ยนเวอร์ชั่นของ `rand` ไปเป็น `0.3.15`

ถ้าคุณอยากใช้ `rand` เวอรชั่น `0.4.0` หรือเวอร์ชั่นประมาณ `0.4.x` คุณจะต้องอัพเดทในไฟล์ `Cargo.toml` แบบนี้แทน:

```toml
[dependencies]

rand = "0.4.0"
```

ทีนี้ตอนที่คุณรัน `cargo build` ครั้งถัดไป Cargo จะอัพเดทการลงทะเบียน crate ตามที่คุณร้องขอใช้ `rand` ให้พร้อมใช้ทันที

ยังมีอีกเยอะเกี่ยวกับ [Cargo][doccargo]<!-- ignore --> และ [ระบบนิเวศของมัน][doccratesio]<!-- ignore --> ที่เราจะคุยกันในบทที่ 14 แต่ตอนนี้รู้แค่นี้ก็เพียงพอให้คุณรู้แล้วว่า Cargo จะช่วยให้ชีวิตคุณง่ายในการใช้ไลบรารี่ นั่นทำให้ ผู้คลั่งใคล้ Rust สามารถเขียนโปรเจ็คได้เล็กลงโดยการประกอบร่างจากแพ็คเก็จจำนวนหนึ่งแทน

[doccargo]: http://doc.crates.io
[doccratesio]: http://doc.crates.io/crates-io.html

### Generating a Random Number

มาถึงตอนนี้ คุณได้เพิ่ม crate `rand` เข้าไปใน *Cargo.toml* เรียบร้อย เรามาเริ่มใช้ `rand` กัน โดยในขั้นตอนต่อไปเราจะมาแก้ไข *src/main.rs* ตามที่เห็นใน Listing 2-3

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

<span class="caption">Listing 2-3: Adding code to generate a random
number</span>

อย่างแรก เราเพิ่มบรรทัด `use` แบบนี้: `use rand::Rng` โดยที่ `Rng` กำหนด trait ให้ตัวสุ่มเลข และ trait นี้ต้องอยู่ในสโคปที่เราจะใช้งานมัน ซึ่งเราจะไปดูรายละเอียดเกี่ยวกับ trait ในบทที่ 10 อีกที

ต่อมา เราเพิ่มอีกสองบรรทัดตรงกลาง ที่มีฟังก์ชั่น `rand::thread_rng` ที่จะให้ตัวสุ่มตัวเลขที่เราเจาะจงจะใช้: สิ่งนี้จะช่วยให้ thread ของเราใช้กลไกการสุ่มมาจาก OS ได้ใน thread เลย หลังจากนั้นเมื่อเราเรียกเมธอด `gen_range` บนตัวสุ่มตัวเลข เมธอดนี้ถูกกำหนดไว้ใน trait `Rng` ที่เราเอาเข้ามาจาก `use rand::Rng` โดย `gen_range` จะรับอาร์กิวเม้นต์สองค่าเพื่อสุ่มเลขระหว่างสองค่านี้ ซึ่งก็คือค่าที่รวมจากขอบล่าง แต่ไม่ถึงขอบบน ดังนั้นเราจึงใส่เลข `1` และ `101` ลงไป เพราะว่าเราต้องการเลขระหว่าง 1 และ 100 นั่นเอง

> สังเกต: คุณไม่จำเป็นต้องรู้ว่า traits ไหนใช้เมธอดอะไร ฟังก์ชั่นอะไรได้บ้าง 
> เพราะมันมีอยู่ในเอกสารของแต่ละ crate อยู่แล้ว โดยคุณสามารถใช้อีกความสามารถหนึ่งของ Cargo 
> ด้วยการรันคำสั่ง `cargo doc --open` แล้วมันจะสร้างเอกสารตาม dependencies
> ที่คุณใช้แล้วก็เปิดให้บนเว็บเบราวเซอร์ ตัวอย่างเช่นถ้าคุณสนใจฟังก์ชั่นอื่นๆใน crate `rand`
> คุณก็รัน `cargo doc --open` แล้วก็คลิกที่ `rand` ในแถบด้านซ้ายมือได้เลย

บรรทัดที่สองที่เราเพิ่มเข้าไปตรงกลางของโค้ดเพื่อพิมพ์ตัวเลขลับ นี่มีประโยชน์มากในระหว่างที่เรากำลังเขียนโปรแกรม เราจะได้ทดสอบมันได้ แต่เราจะลบมันออกในตอนที่เขียนเสร็จ เพราะไม่งั้นมันก็ดูไม่ค่อยจะเป็นเกมเท่าไหร่นะ ถ้าโปรแกรมพิมพ์คำตอบตั้งแต่ต้นน่ะ!

ลองรันโปรแกรมสักสองสามครั้งดู:

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4
$ cargo run
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
```

คุณควรจะต้องได้เลขที่แตกต่างกันในแต่ละครั้ง และต้องเป็นเลขที่อยู่ระหว่าง 1 และ 100 ด้วยนะ สุดยอดเลย!

## Comparing the Guess to the Secret Number

ทีนี้เราก็ได้เลขที่ผู้เล่นใส่เข้ามา แล้วก็ตัวเลขสุ่ม มาแล้ว เราสามารถเอามันมาเทียบกันได้ละ ตามที่เห็นใน Listing 2-4 แต่เราจะยังไม่คอมไพล์มันตอนนี้ ขออธิบายก่อน

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {

    // ---snip---

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

<span class="caption">Listing 2-4: Handling the possible return values of
comparing two numbers</span>

อย่างแรกมีของใหม่นิดหน่อยกับการใช้ `use` เพื่อนำเข้า type จากไลบรารี่มาตรฐาน ที่ชื่อ `std::cmp::Ordering` เข้ามาในสโคป ซึ่งมันจะคล้ายกับ `Result` ก่อนหน้านี้ `Ordering` เป็น enum อีกตัวนึง แต่ค่าที่อยู่ใน `Ordering` จะเป็น `Less` กับ `Greater` และ `Equal` แทน ซึ่งจะเป็นผลลัพธ์ที่เป็นไปได้เวลาที่เราเปรียบเทียบค่าสองค่า

จากนั้นเราจะเพิ่มอีกห้าบรรทัดข้างล่างที่ใช้ type `Ordering` โดยใช้เมธอด `cmp` เปรียบเทียบค่าสองค่า ซึ่งใช้เปรียบเทียบค่าอะไรก็ได้ที่มันจะสามารถเทียบกันได้ โดยใส่ค่าที่อ้างถึงของที่คุณต้องการจะเทียบลงไป: ในกรณีนี้มันใช้เทียบ `guess` กับ `secret_number` จากนั้นมันจะคือค่า enum `Ordering` ที่เรานำเข้ามาโดย `use` แล้วเราก็จะใช้ [`match`][match]<!-- ignore --> เพื่อตัดสินใจว่าจะทำอะไรในแต่ละกรณี

[match]: ch06-02-match.html

คำ `match` มาพร้อมกับ *แขน* แต่ละ แขน จะประกอบไปด้วย *รูปแบบ* และโค้ดเริ่มต้นที่คำว่า `match` ว่าจะตรงกับรูปแบบของแขนไหน Rust จะเอาค่าที่ได้มาให้ `match` เพื่อดูว่าแขนไหนกลับมา การสร้าง `match` และรูปแบบการใช้ใน Rust นี้มันทรงพลังมากที่ยอมให้คุณจัดการสถานการณ์ที่มีทางเป็นไปได้หลากหลาย ให้คุณมั่นใจว่าจะจัดการมันได้หมดทุกกรณี และเราจะมีรายละเอียดเรื่องนี้ต่อในบทที่ 6 และ 18

มาดูตัวอย่างของเราต่อกันว่าจะเกิดอะไรขึ้นเวลาใช้ `match` ยกตัวอย่างเช่น เมื่อผู้เล่นเดาเลข 50 เข้ามา ในขณะที่ตัวเลขคำตอบเป็น 38 เมื่อโค้ดเทียบค่าด้วย `cmp` มันจะคืนค่า `Ordering::Grater` เพราะว่า 50 มีค่ามากกว่า 38 จากนั้น `match` จะเอาค่า `Ordering::Greater` มาดูที่ขาแรก `Ordering::Less` และดูว่า `Ordering::Greater` ไม่ตรงกับ `Ordering::Less` มันก็เลยไม่สนใจแขนนี้แล้วย้ายไปแขนอื่นต่อ ทีนี้แขนต่อไปก็คือ `Ordering::Greater` ซึ่งมัน *ตรง* กันกับ `Ordering::Greater` ที่ได้มาพอดี มันก็จะทำงานตามแขนนี้แล้วพิพม์คำว่า `Too big!` ออกมาบนหน้าจอ จากนั้นก็จะจบการทำงานของ `match` เพราะมันไม่จำเป็นที่จะต้องไปดูแขนสุดท้ายแล้ว

แต่ในโค้ด Listing 2-4 เรายังไม่ได้คอมไพล์นะ ลองทำแบบนี้ดู:

```text
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
error[E0308]: mismatched types
  --> src/main.rs:23:21
   |
23 |     match guess.cmp(&secret_number) {
   |                     ^^^^^^^^^^^^^^ expected struct `std::string::String`, found integral variable
   |
   = note: expected type `&std::string::String`
   = note:    found type `&{integer}`

error: aborting due to previous error
Could not compile `guessing_game`.
```

ใจความสำคัญของข้อผิดพลาดนี้ก็คือ *mismatched types* ซึ่ง Rust ใช้ระบบ strong static type แต่ก็มีความสามารถเดา type ได้ เวลาที่เราเขียนแบบนี้ `let mut guess = String::new()` Rust เดาได้ว่า `guess` ควรเป็น `String` และทำให้เราไม่ต้องเขียน type มันลงไป ส่วน `secret_number` ก็จะเป็น type number เนื่องจากมี type แบบ number สองสามตัวที่สามารถมีค่าระหว่าง 1 ถึง 100 ได้เช่น: `i32` เป็นเลข 32-bit หรือ `u32` เป็นเลขจำนวนบวก 32-bit หรือ `i64` เป็นเลข 64-bit แต่เบื้อต้น Rust จะกำหนด `i32` ให้ `secret_number` เว้นเสียแต่ว่าคุณจะเพิ่มข้อมูลให้มากกว่านี้หน่อย Rust อาจจะเดาเป็น type อื่นให้ แต่ว่าข้อผิดพลาดที่เราเจอนั้น มันหมายความว่าไม่สามารถเปรียบเทียบค่าที่เป็น string กับตัวเลขได้

สุดท้ายแล้วเราต้อง แปลงค่าที่เรารับมาเป็น `String` ไปเป็นตัวเลขจริงๆ ที่เราจะได้่เอาไปเปรียบเทียบกับคำตอบได้ เราทำสิ่งนี้ได้ด้วยการเพิ่มสองบรรทัดตามนี้ใน `main`

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
// --snip--

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = guess.trim().parse()
        .expect("Please type a number!");

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

สองบรรทัดที่ว่าก็คือ:

```rust,ignore
let guess: u32 = guess.trim().parse()
    .expect("Please type a number!");
```

เราสร้างตัวแปรชื่อ `guess` ตัวหนึ่ง แต่เดี๋ยวก่อน โปรแกรมยอมให้มีตัวแปลชื่อซ่้ำกันได้ด้วยเหรอ? คำตอบคือได้ แต่ Rust จะยอมให้เราใช้มันแบบ *เงา* ของค่าก่อนหน้านี้ ได้เป็นตัวใหม่ ซึ่งฟีเจอร์นี้ถูกใช้บ่อยๆในกรณีที่คุณต้องการแปลงค่าจาก type หนึ่งไปเป็นอีก type หนึ่ง การสร้างเงา ทำให้เราสามารถใช้ตัวแปรชื่อ `guess` ได้เลย แทนที่จะไปสร้างตัวแปรชื่ออีกเช่น `guess_str` ในขณะที่ก็มี `guess` อีกตัว (ในบทที่ 3 จะมีรายละเอียดเรื่องการสร้างเงา)

เราผูก `guess` ไว้แบบนี้ `guess.trim().parse()` โดย `guess` ตัวนี้อ้างถึง `guess` ตัวแรกที่เป็น `String` โดยมันจะกำจัดพวก whitespace ที่อยู่หัวและท้ายข้อความออกไป ถึงแม้ว่า `u32` เก็บได้เฉพาะตัวเลขเท่านั้น แต่ตอนที่ผู้เล่นกดปุ่ม <span class="keystroke">enter</span> เพื่อส่งคำตอบให้ `read_line` ตอนที่ผู้เล่นกด <span class="keystroke">enter</span> จะมีตัวอักษรขึ้นบรรทัดใหม่เติมเข้ามาใน string ตัวอย่างเช่น ถ้าผู้เล่นพิมพ์ <span class="keystroke">5</span> แล้วกดปุ่ม <span class="keystroke">enter</span> เราจะได้ `5\n` เข้ามา โดยเจ้า `\n` ก็คือเครื่องหมายการ “ขึ้นบรรทัดใหม่” เราจึงใช้ `trim` เพื่อกำจัด `\n` ให้เหลือแค่ `5`

เมธอด [`parse` method on strings][parse]<!-- ignore --> จะแปลง string ไปเป็นตัวเลข แต่เพราะว่าเมธอดนี้สามารถเปลี่ยนไปเป็นตัวเลขชนิดใดก็ได้หลากหลายมาก เราจึงจำเป็นต้องบอก Rust ให้ชัดเจนว่าเราต้องการได้ type อะไรด้วยการใส่ `let guess: u32` โดยเครื่องหมายโคลอน (`:`) หลัง `guess` เป็นการอธิบาย type ให้ตัวแปร โดยที่ Rust มี type ที่เป็นตัวเลขของตัวเองแค่สองสามชนิด โดย `u32` ที่เห็นนี้เป็นตัวเลขแบบจำนวนเต็มบวกขนาด 32-bit มันน่าจะเป็นตัวเลือกที่ดีสำหรับเลขจำนวนน้อยๆที่มีแต่ค่าบวก ซึ่งเดี๋ยวเราจะได้เรียนเกี่ยวกับ type ตัวเลขในบทที่ 3 และเพิ่มอีกนิดว่า `u32` ที่เราใส่เข้าไปในตัวอย่างนี้ เพื่อไปเทียบกับ `secret_number` ยังหมายถึงว่า Rust จะเดาค่า `secret_number` ให้เป็น `u32` ไปด้วยเลย ดังนั้นตอนนี้เราจะสามารถเทีบบค่าทั้งสองได้แล้วเพราะว่ามี type ตรงกัน

[parse]: ../std/primitive.str.html#method.parse

`parse` มันเกิด error ง่ายมาก เช่นถ้าไปเจอสตริงที่มี `A👍%` มันแต่นอนว่าไม่สามารถแปลงค่าไปเป็นตัวเลขได้ และมันจะทำให้เกิดความผิดพลาดขึ้นและ `parse` จะคืนค่า `Result` แบบเดียวกับที่ `read_line` ทำ (เดี๋ยวเราจะไปคุยเรื่องนี้กันต่อใน [“Handling Potential Failure with the `Result` Type”](#handling-potential-failure-with-the-result-type)<!-- ignore -->) แล้วเราก็จะจัดการกับ `Result` แบบเดียวกันด้วยการใช้ `expect` ถ้า `parse` คืน `Err` มาใน `Result` ด้วยเหตุว่ามันไม่สามารถเปลี่ยนตัวหนังสือเป็นตัวเลขได้ จะทำให้ `expect` เกมแคลช และแสดงข้อความที่เราให้มันไป แต่ถ้า `parse` แปลงค่าได้สำเร็จมันจะคืน `Result` ที่เป็น `Ok` มาให้ แล้ว `expect` ก็จะคืนตัวเลขที่ได้มาในค่า `Ok`

ลองรันโปรแกรมดูเลย!

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43 secs
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

เยี่ยม! ถึงแม่ว่าจะมีการเว้นวรรคหนึ่งก่อนจะใส่เลขเข้ามา โปรแกรมก็จะจับได้ค่า 76 แน่นอน ลองรันดูสักสองสามรอบ เพื่อตรวจสอบว่าได้พฤติกรรมที่แตกต่างกันในแต่ละครั้ง: เดาดูก, เดาเลขสูงเกินไป และเดาเลขต่ำเกินไป

ตอนนี้เราได้เกมที่เล่นได้แล้ว แต่ผู้เล่นจะเดาได้แค่งครั้งเดียว เราจะมาแก้ให้มันวนลูปกันต่อ

## Allowing Multiple Guesses with Looping

คีย์เวิร์ด `loop` ใช้สร้างลูปไม่รู้จบ และเราจะเพิ่มมันเข้าไปเพื่อให้ผู้เล่นมีโอกาสเดาไปเรื่อยๆ:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
// --snip--

    println!("The secret number is: {}", secret_number);

    loop {
        println!("Please input your guess.");

        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => println!("You win!"),
        }
    }
}
```

อย่างที่เห็น เราได้ย้ายทุกอย่างเข้าไปในลูปเริ่มตั้งแต่คำชวนให้ผู้เล่นใส่ตัวเลขกันเลย อย่าลืมว่าแต่ละบรรทัดในลูปจะต้องเว้นย่อหน้าเข้าไปสี่วรรคด้วยนะ แล้วลองรันดูอีกที คราวนี้เราจะเจอปัญหาใหม่อีก เพราะว่าโปรแกรมทำงานตามที่เราสั่งเป๊ะๆคือ ถ้าไปเรื่อยๆ และดูเหมือนผู้เล่นจะไม่สามารถออกจากเกมได้!

ที่จริงผู้เล่นสามารถจะหยุดการทำงานของโปรแกรมได้ทุกเมื่อด้วยการใช้คีย์ลัด <span
class="keystroke">ctrl-c</span> แต่ก็มีทางอื่นอีกที่จะใช้ออกจากเจ้าปีศาจตะกละตัวนี้ ถ้ายังจำได้ว่าเราเคยคุยกันเรื่อง `parse` ในตอน [“Comparing the Guess to the Secret Number”](#comparing-the-guess-to-the-secret-number)<!-- ignore -->: ถ้าผู้เล่นใส่อะไรเข้ามาที่มันไม่ใช่ตัวเลข โปรแกรมจะแคลช จำได้ไหม เราใช้ประโยชน์จากจุดนั้นเพื่อจะออกจากโปรแกรมได้ ลองดูตัวอย่าง:

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.50 secs
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/libcore/result.rs:785
note: Run with `RUST_BACKTRACE=1` for a backtrace.
error: Process didn't exit successfully: `target/debug/guess` (exit code: 101)
```

พิมพ์ `quit` ใส่เข้ามามันจะออกจากเกมได้ แต่มันก็แค่การใส่อะไรที่ไม่ใช่ตัวเลขเข้าไปแค่นั้นนะ อย่างไรก็ตาม อย่างน้อยนี่ก็เป็นผลพลอยได้นะ ต่อไปเรายังอยากให้เกมจบทันทีที่ผู้เล่นเดาถูกด้วย

### Quitting After a Correct Guess

เรามาทำให้เกมจบเมื่อผู้เล่นชนะด้วยการเพิ่ม `break` เข้าไปกัน:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
// --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

เพิ่มบรรทัด `break` หลังจาก `You win!` เพื่อให้โปรแกรมจบลูปเมื่อผู้เล่นเดาตัวเลขได้ถูกต้อง และการออกจากลูปยังหมายถึงการออกจากโปรแกรมไปด้วยเลยในตัว เพราะว่าลูปเป็นขั้นตอนสุดท้ายใน `main` แล้วนั่นเอง

### Handling Invalid Input

เพื่อให้เกมปรับปรุงพฤติกรรมให้ดีขึ้น แทนที่จะให้มันแคลชเวลาที่ผู้เล่นใส่ของที่ไม่ใช่ตัวเลขเข้ามา เรามาทำให้มันเพิกเฉยของพวกนั้นกัน แล้วยอมให้ผู้เล่นเดาเลขต่อไปได้ โดยเราสามารถทำได้ด้วยการปรับปรุงบรรทันที่ `guess` แปลงค่าจาก `String` ไปเป็น `u32` ตามที่ได้แสดงให้เห็นใน Listing 2-5

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
// --snip--

io::stdin().read_line(&mut guess)
    .expect("Failed to read line");

let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};

println!("You guessed: {}", guess);

// --snip--
```

<span class="caption">Listing 2-5: เพิกเฉยต่อสิ่งที่ไม่ใช้ตัวเลข แล้วถามหาคำตอบต่อไปโดยไม่ทำให้โปรแกรมแคลช</span>

การเปลี่ยนจากการใช้ `expect` ไปใช้ `match` เป็นท่าปกติที่จะแก้จากการที่เราปล่อยให้่โปรแกรมแคลชไปเฉยๆ ไปจัดการกับ error แทน ถ้ายังจำได้ `parse` จะคืน `Result` และ `Result` เป็น enum ที่มีค่าที่เป็นไปได้คือ `Ok` หรือ `Err` แบบเดียวกับที่เราทำตอน `Ordering` แล้วได้ผลลัพธ์จากเมธอด `cmp` นั่นเอง

ถ้า `parse` สามารถทำงานได้สำเร็จโดยการแปลงค่าสตริงไปเป็นตัวเลขได้ มันจะคือค่า `Ok` ที่มีตัวเลขที่ได้มาให้ `Ok` จะตรงกับแขนแรกในรูปแบบของ match และมันก็แค่คืนค่า `num` ที่ถูกสร้างจาก `parse` มาใน `Ok` แล้วสุดท้ายตัวเลขนั้นก็จะจบด้วยการไปเป็นค่าในตัวแปร `guess` ที่เราสร้างรอไว้

กลับกันถ้า `parse` *ไม่* สามารถเปลี่ยนค่าจากสตริงไปเป็นตัวเลขได้ มันจะคืน `Err` ที่บรรจุรายละเอียดเกี่ยวกับ error ที่เกิดขึ้น และมันจะไม่ตรงกับรูปบบ `Ok(num)` ในแขนแรกของ `match` แต่จะไปตรงกับรูปแบบ `Err(_)` ในแขนที่สองแทน โดยที่ขีดล่าง `_` เป็นการบอกว่าทุกกรณี จากตัวอย่างนี้เราบอกเลยว่า ไม่ว่าจะเกิด error อะไรก็แล้วแต่โดยไม่สนว่าจะมีข้อมูลอะไรในนั้น เราจะให้โปรแกรมทำงานต่อไปด้วยการใช้ `continue` มันได้ผล เพราะโปรแกรมจะไม่สน error อะไรเลย

ตอนนี้โปรแกรมเราน่าจะทำงานได้ดีหมดแล้ว ทดลองดู:

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 61
Please input your guess.
10
You guessed: 10
Too small!
Please input your guess.
99
You guessed: 99
Too big!
Please input your guess.
foo
Please input your guess.
61
You guessed: 61
You win!
```

เยี่ยมมาก! เหลืออีกนิดเดียว เราจะทำเกมเดาคำเสร็จแล้ว ยังจำได้ไหมว่าโปรแกรมเรายังพิมพ์ผลลัพธ์ออกมาโต้งๆอยู่เลย ซึ่งมันดีตอนที่เรากำลังทดสอบอยู่ แต่มันคงไม่สนุกตอนที่เล่นจริง เรามาลบ `println!` ทิ้งกัน และสุดท้ายจะได้โค้ดหน้าตาแบบนี้ตามที่แสดงให้เห็นใน Listing 2-6

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin().read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

<span class="caption">Listing 2-6: Complete guessing game code</span>

## Summary

มาถึงจุดนี้ เราก็ได้ทำเกมเดาคำสำเร็จเรียบร้อยแล้ว ขอแสดงความยินดีด้วย!

ประสบการณ์ที่เราได้จากโปรเจ็คนี้ คือการแนะนำให้คุณได้รู้จักคอนเซ็ปหลายๆอย่างของ Rust: เช่น การใช้เมธอด `let` และ `match` และฟังก์ชั่นที่เกี่ยวข้องกับมัน การใช้ crate จากภายนอก และอีกหลายๆสิ่ง ในบทต่อๆไป คุณจะได้เรียนรู้เกี่ยวกับ หลักการพวกนี้ในรายละเอียดเพิ่มเติม โดยในบทที่ 3 จะครอบคลุมถึงหลักการส่วนใหญ่ของภาษาโปรแกรมมิ่ง เช่น ตัวแปร ชนิดของตัวแปร และฟังก์ชั่น พร้อมวิธีใช้ใน Rust ส่วนในบทที่ 4 จะพาไปเกี่ยวกับฟีเจอร์ ownership ว่ามันทำให้ Rust แตกต่างจากภาษาอื่นๆอย่างไร และในบทที่ 5 จะถกประเด็นเรื่อง struct และ method และสุดท้ายในบทที่ 6 จะอธิบายว่า enum ทำงานอย่างไร

[variables-and-mutability]:
ch03-01-variables-and-mutability.html#variables-and-mutability
