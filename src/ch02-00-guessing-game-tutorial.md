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

ต่อมา เราเพิ่มอีกสองบรรทัดตรงกลาง ที่มีฟังก์ชั่น `rand::thread_rng` ที่จะให้ตัวสุ่มตัวเลขที่เราเจาะจงจะใช้: สิ่งนี้จะช่วยให้ thread ของเราเรียกใช้ตัวสุ่มจาก OS ได้โดยตรง หลังจากนั้นเมื่อเราเรียกเมธอด `gen_range` บนตัวสุ่มตัวเลข เมธอดนี้ถูกกำหนดไว้ใน trait `Rng` ที่เราเอาเข้ามาจาก `use rand::Rng` โดย `gen_range` จะรับอาร์กิวเม้นต์สองค่าเพื่อสุ่มเลขระหว่างสองค่านี้ ซึ่งก็คือค่าที่รวมจากขอบล่าง แต่ไม่ถึงขอบบน ดังนั้นเราจึงใส่เลข `1` และ `101` ลงไป เพราะว่าเราต้องการเลขระหว่าง 1 และ 100 นั่นเอง

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

Now that we have user input and a random number, we can compare them. That step
is shown in Listing 2-4. Note that this code won’t compile quite yet, as we
will explain.

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

The first new bit here is another `use` statement, bringing a type called
`std::cmp::Ordering` into scope from the standard library. Like `Result`,
`Ordering` is another enum, but the variants for `Ordering` are `Less`,
`Greater`, and `Equal`. These are the three outcomes that are possible when you
compare two values.

Then we add five new lines at the bottom that use the `Ordering` type. The
`cmp` method compares two values and can be called on anything that can be
compared. It takes a reference to whatever you want to compare with: here it’s
comparing the `guess` to the `secret_number`. Then it returns a variant of the
`Ordering` enum we brought into scope with the `use` statement. We use a
[`match`][match]<!-- ignore --> expression to decide what to do next based on
which variant of `Ordering` was returned from the call to `cmp` with the values
in `guess` and `secret_number`.

[match]: ch06-02-match.html

A `match` expression is made up of *arms*. An arm consists of a *pattern* and
the code that should be run if the value given to the beginning of the `match`
expression fits that arm’s pattern. Rust takes the value given to `match` and
looks through each arm’s pattern in turn. The `match` construct and patterns
are powerful features in Rust that let you express a variety of situations your
code might encounter and make sure that you handle them all. These features
will be covered in detail in Chapter 6 and Chapter 18, respectively.

Let’s walk through an example of what would happen with the `match` expression
used here. Say that the user has guessed 50 and the randomly generated secret
number this time is 38. When the code compares 50 to 38, the `cmp` method will
return `Ordering::Greater`, because 50 is greater than 38. The `match`
expression gets the `Ordering::Greater` value and starts checking each arm’s
pattern. It looks at the first arm’s pattern, `Ordering::Less`, and sees that
the value `Ordering::Greater` does not match `Ordering::Less`, so it ignores
the code in that arm and moves to the next arm. The next arm’s pattern,
`Ordering::Greater`, *does* match `Ordering::Greater`! The associated code in
that arm will execute and print `Too big!` to the screen. The `match`
expression ends because it has no need to look at the last arm in this scenario.

However, the code in Listing 2-4 won’t compile yet. Let’s try it:

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

The core of the error states that there are *mismatched types*. Rust has a
strong, static type system. However, it also has type inference. When we wrote
`let mut guess = String::new()`, Rust was able to infer that `guess` should be
a `String` and didn’t make us write the type. The `secret_number`, on the other
hand, is a number type. A few number types can have a value between 1 and 100:
`i32`, a 32-bit number; `u32`, an unsigned 32-bit number; `i64`, a 64-bit
number; as well as others. Rust defaults to an `i32`, which is the type of
`secret_number` unless you add type information elsewhere that would cause Rust
to infer a different numerical type. The reason for the error is that Rust
cannot compare a string and a number type.

Ultimately, we want to convert the `String` the program reads as input into a
real number type so we can compare it numerically to the secret number. We can
do that by adding the following two lines to the `main` function body:

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

The two new lines are:

```rust,ignore
let guess: u32 = guess.trim().parse()
    .expect("Please type a number!");
```

We create a variable named `guess`. But wait, doesn’t the program already have
a variable named `guess`? It does, but Rust allows us to *shadow* the previous
value of `guess` with a new one. This feature is often used in situations in
which you want to convert a value from one type to another type. Shadowing lets
us reuse the `guess` variable name rather than forcing us to create two unique
variables, such as `guess_str` and `guess` for example. (Chapter 3 covers
shadowing in more detail.)

We bind `guess` to the expression `guess.trim().parse()`. The `guess` in the
expression refers to the original `guess` that was a `String` with the input in
it. The `trim` method on a `String` instance will eliminate any whitespace at
the beginning and end. Although `u32` can contain only numerical characters,
the user must press <span class="keystroke">enter</span> to satisfy
`read_line`. When the user presses <span class="keystroke">enter</span>, a
newline character is added to the string. For example, if the user types <span
class="keystroke">5</span> and presses <span class="keystroke">enter</span>,
`guess` looks like this: `5\n`. The `\n` represents “newline,” the result of
pressing <span class="keystroke">enter</span>. The `trim` method eliminates
`\n`, resulting in just `5`.

The [`parse` method on strings][parse]<!-- ignore --> parses a string into some
kind of number. Because this method can parse a variety of number types, we
need to tell Rust the exact number type we want by using `let guess: u32`. The
colon (`:`) after `guess` tells Rust we’ll annotate the variable’s type. Rust
has a few built-in number types; the `u32` seen here is an unsigned, 32-bit
integer. It’s a good default choice for a small positive number. You’ll learn
about other number types in Chapter 3. Additionally, the `u32` annotation in
this example program and the comparison with `secret_number` means that Rust
will infer that `secret_number` should be a `u32` as well. So now the
comparison will be between two values of the same type!

[parse]: ../std/primitive.str.html#method.parse

The call to `parse` could easily cause an error. If, for example, the string
contained `A👍%`, there would be no way to convert that to a number. Because it
might fail, the `parse` method returns a `Result` type, much as the `read_line`
method does (discussed earlier in [“Handling Potential Failure with the
`Result` Type”](#handling-potential-failure-with-the-result-type)<!-- ignore
-->). We’ll treat this `Result` the same way by using the `expect` method
again. If `parse` returns an `Err` `Result` variant because it couldn’t create
a number from the string, the `expect` call will crash the game and print the
message we give it. If `parse` can successfully convert the string to a number,
it will return the `Ok` variant of `Result`, and `expect` will return the
number that we want from the `Ok` value.

Let’s run the program now!

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

Nice! Even though spaces were added before the guess, the program still figured
out that the user guessed 76. Run the program a few times to verify the
different behavior with different kinds of input: guess the number correctly,
guess a number that is too high, and guess a number that is too low.

We have most of the game working now, but the user can make only one guess.
Let’s change that by adding a loop!

## Allowing Multiple Guesses with Looping

The `loop` keyword creates an infinite loop. We’ll add that now to give users
more chances at guessing the number:

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

As you can see, we’ve moved everything into a loop from the guess input prompt
onward. Be sure to indent the lines inside the loop another four spaces each
and run the program again. Notice that there is a new problem because the
program is doing exactly what we told it to do: ask for another guess forever!
It doesn’t seem like the user can quit!

The user could always interrupt the program by using the keyboard shortcut <span
class="keystroke">ctrl-c</span>. But there’s another way to escape this
insatiable monster, as mentioned in the `parse` discussion in [“Comparing the
Guess to the Secret Number”](#comparing-the-guess-to-the-secret-number)<!--
ignore -->: if the user enters a non-number answer, the program will crash. The
user can take advantage of that in order to quit, as shown here:

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

Typing `quit` actually quits the game, but so will any other non-number input.
However, this is suboptimal to say the least. We want the game to automatically
stop when the correct number is guessed.

### Quitting After a Correct Guess

Let’s program the game to quit when the user wins by adding a `break` statement:

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

Adding the `break` line after `You win!` makes the program exit the loop when
the user guesses the secret number correctly. Exiting the loop also means
exiting the program, because the loop is the last part of `main`.

### Handling Invalid Input

To further refine the game’s behavior, rather than crashing the program when
the user inputs a non-number, let’s make the game ignore a non-number so the
user can continue guessing. We can do that by altering the line where `guess`
is converted from a `String` to a `u32`, as shown in Listing 2-5.

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

<span class="caption">Listing 2-5: Ignoring a non-number guess and asking for
another guess instead of crashing the program</span>

Switching from an `expect` call to a `match` expression is how you generally
move from crashing on an error to handling the error. Remember that `parse`
returns a `Result` type and `Result` is an enum that has the variants `Ok` or
`Err`. We’re using a `match` expression here, as we did with the `Ordering`
result of the `cmp` method.

If `parse` is able to successfully turn the string into a number, it will
return an `Ok` value that contains the resulting number. That `Ok` value will
match the first arm’s pattern, and the `match` expression will just return the
`num` value that `parse` produced and put inside the `Ok` value. That number
will end up right where we want it in the new `guess` variable we’re creating.

If `parse` is *not* able to turn the string into a number, it will return an
`Err` value that contains more information about the error. The `Err` value
does not match the `Ok(num)` pattern in the first `match` arm, but it does
match the `Err(_)` pattern in the second arm. The underscore, `_`, is a
catchall value; in this example, we’re saying we want to match all `Err`
values, no matter what information they have inside them. So the program will
execute the second arm’s code, `continue`, which tells the program to go to the
next iteration of the `loop` and ask for another guess. So, effectively, the
program ignores all errors that `parse` might encounter!

Now everything in the program should work as expected. Let’s try it:

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

Awesome! With one tiny final tweak, we will finish the guessing game. Recall
that the program is still printing the secret number. That worked well for
testing, but it ruins the game. Let’s delete the `println!` that outputs the
secret number. Listing 2-6 shows the final code.

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

At this point, you’ve successfully built the guessing game. Congratulations!

This project was a hands-on way to introduce you to many new Rust concepts:
`let`, `match`, methods, associated functions, the use of external crates, and
more. In the next few chapters, you’ll learn about these concepts in more
detail. Chapter 3 covers concepts that most programming languages have, such as
variables, data types, and functions, and shows how to use them in Rust.
Chapter 4 explores ownership, a feature that makes Rust different from other
languages. Chapter 5 discusses structs and method syntax, and Chapter 6
explains how enums work.

[variables-and-mutability]:
ch03-01-variables-and-mutability.html#variables-and-mutability
