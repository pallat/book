# Introduction

> สังเกต: หนังสือฉบับนี้เหมือนกันกับ [The Rust Programming
> Language][nsprust] โดยหาซื้อได้ทั้งรูปแบบพิมพ์และ ebook ได้จาก [No Starch
> Press][nsp]

[nsprust]: https://nostarch.com/rust
[nsp]: https://nostarch.com/

ยินดีต้อนรับสู่ *The Rust Programming Language* หนังสือเกี่ยวกับภาษา Rust เบื้องต้น
The Rust programming language จะช่วยให้คุณทำซอท์ฟแวร์ได้เร็วและดีขึ้นกว่าเดิม
บ่อยครั้งที่เราจะพบว่าการเขียนโปรแกรมระดับสูงกับระดับล่าง จะมีประเด็นเกี่ยวกับการออกแบบตัวภาษา
แต่ Rust จะท้าทายประเด็นนี้ ด้วยการสร้างความสมดุลทางด้านเทคนิคของทั้งสองฝั่ง และเสนอตัวเลือก
ให้คุณควบคุมในรายละเอียดเชิงลึก(เช่นการใช้หน่วยความจำ) ที่ไม่ยุ่งยากแบบที่เคยเจอมา

## Rust เหมาะกับใครบ้าง

Rust เหมาะสมอย่างยิ่งกับหลายๆผู้คน ด้วยหลายๆเหตุและผล เราลองมาดูสักสองสามกลุ่มสำคัญๆกัน

### ทีมนักพัฒนาซอฟท์แวร์

Rust พิสูจน์ให้เห็นว่าเป็นเครื่องมือที่มีประสิทธิภาพในการทำงานกับทีมใหญ่ๆ ที่มีระดับความรู้หลากหลาย
ในเรื่องการเขียนโปรแกรมระบบ ซึ่งปกติโค้ดระบบเชิงลึกมีแนวโน้มที่จะเกิดบั๊กเยอะแยะ ซึ่งถ้าเป็นภาษาอื่นๆ
คุณจะต้องเขียนเทสกันไว้ และให้คนมีประสบการณ์มาตรวจทานด้วยความรอบคอบก่อนเสมอ
แต่ใน Rust จะมีตัวคอมไพเลอร์ที่ทำหน้าที่เป็น รปภ โดยจะไม่ยอมคอมไพล์โค้ดที่อาจจะก่อนให้เกิดบั๊กได้
รวมไปถึงบั๊กของการทำงานแบบ concurrency ด้วย และด้วยเหตุนี้ ทีมจะได้ไม่ต้องมาพะวงเรื่องนี้
แล้วเอาเวลาไปทำ logic ให้ดีแทน

Rust ยังจะมีเครื่องมีที่ทันสมัยมาช่วยให้ชีวิตโปรแกรมเมอร์ง่ายขึ้นอีก:

* Cargo, ที่มีตัวจัดการ dependency และ build tool โดยมันจะช่วยในเรื่องการเพิ่ม lib
  การคอมไพล์ และจัดการความเข้ากันของระบบนิเวศ ของ Rust
* Rustfmt มาช่วยให้โค้ดของนักพัฒนาแต่ละคน หน้าตาเหมือนกัน
* Rust Language Server จะทำงานร่วมกับ IDE เช่น ช่วยเติมโค้ด แจ้ง error

ด้วยการใช้ tools เหล่านี้ในระบบนิเวศของ Rust จะทำให้นักพัฒนามีทำงานได้อย่างมีประสิทธิภาพ
มากขึ้น

### นักเรียน

Rust เหมาะกับนักเรียนที่มีความสนใจเกี่ยวกับ แนวคิดของระบบ ซึ่งการใช้ Rust จะทำให้ผู้คนได้เรียนรู้
เกี่ยวกับหัวข้อเรื่องการพัฒนา OS และชุมชนนักพัฒนาของเรา ยินดีเป็นอย่างมากที่จะช่วยตอบคำถามจาก
นักเรียน ด้วยความพยายามแบบเดียวกับหนังสือเล่มนี้ ทีมงาน Rust ต้องการสร้างแนวคิด ให้ใครๆก็สามารถ
เข้าถึงได้ โดยเฉพาะอย่างยิ่ง นักเขียนโปรแกรมหน้าใหม่ทั้งหลาย

### บริษัทต่างๆ

มีบริษัทจำนวนมาก ทั้งเล็กใหญ่ ที่ใช้ Rust บน production ในหลายๆรูปแบบ มีทั้งแบบทำเป็น command line,
web services, เครื่องมือสำหรับ DevOps, ใส่ลงไปในอุปกรณ์ต่างๆ, การแปลงและวิเคราะห์ภาพและเสียง,
cryptocurrencies, bioinformatics, search engines, Internet of Things applications, machine
learning, และส่วนหลักๆใน Firefox web browser

### นักพัฒนา Open Source

Rust เหมาะกับคนที่ต้องการมาช่วยกันสร้าง ภาษา Rust, ร่วมกับชุมชนของเรา, สร้างเครื่องมือสำหรับนักพัฒนา,
และ libraries ต่างๆ เรายินดีที่คุณจะมาช่วยกันเป็นส่วนหนึ่งของภาษา Rust

### คนที่ให้ความสำคัญกับความเร็วและเสถียรภาพ

Rust เหมาะสำหรับคนที่กระหายความเร็วและความเสถียรของตัวภาษา โดยทางด้านความเร็ว
เราหมายถึงความเร็วในการเขียนโปรแกรมด้วยภาษา Rust และวิธีที่ Rust นำเสนอให้คุณเขียนมัน
ตัวคอมไพเลอร์ของ Rust จะตรวจสอบความเสถียรอยู่เสมอ ไม่ว่าคุณจะเพิ่มฟีเจอร์หรือ refactor ก็ตาม
ซึ่งตรงข้ามกับโค้ดแบบเก่าๆที่เขียนด้วยภาษาที่ไม่มีการตรวจสอบแบบนี้ และบ่อยครั้งมันนทำให้นักพัฒนาไม่
กล้าที่จะแก้ไขอะไรเลย ด้วยความพยายามอย่างหนักของ zero-cost abstractions ทำให้ฟีเจอร์
ในระดับบน ถูกคอมไพล์สู่โค้ดระดับล่างด้วยความเร็วสูงเหมือนไม่มี abstractions อยู่เลย นี่คือสิ่งที่
Rust พยายามทำ เพื่อให้โค้ดมีความปลอดภัยและเร็วที่สุดเท่าที่จะทำได้

ภาษา Rust หวังว่าจะสนับสนุนกลุ่มอื่นๆเช่นกัน ที่เรากล่าวถึงนี้เป็นเพียงส่วนหนึ่ง และ Rust มีความ
มุ่งมั่นที่จะทะลายกรอบความคิดที่ว่า โปรแกรมเมอร์จะต้องเลือกระหว่าง ความปลอดภัยและประสิทธิภาพ,
ความเร็วกับการออกแบบที่ดี ให้โอกาส Rust ให้ทดลองให้คุณเห็นเผื่อว่านี่คือสิ่งที่คุณกำลังตามหา

## หนังสือเล่มนี้เหมาะกับใคร

หนังสือเล่มนี้คาดว่าคุณได้เคยเขียนโปรแกรมด้วยภาษาอื่นมาบ้างแต่ไม่ได้เจาะจงว่าจะเป็นภาษาใด
เราพยายามทำเนื้อหาให้กว้างพอสำหรับคนที่มีพื้นเพจากภาษาใดก็ได้ และเราคงจะไม่เสียเวลาไปกับการ
บอกว่า การเขียนโปรแกรมต้องทำอย่างไร หรือคิดอย่างไร ซึ่งถ้าคุณเป็นมือใหม่ มันจะดีกับตัวคุณเองถ้า
คุณเริ่มหาอ่านหนังสือที่เกี่ยวกับการเขียนโปรแกรมเบื้องต้นก่อน

## จะใช้หนังสือนี้อย่างไร

โดยปกติ หนังสือเล่มนี้จะหวังให้คุณอ่านตั้งแต่ด้านหน้าไปจนถึงด้านท้าย เพราะบทท้ายๆจะอ้างอิงจาก
บทก่อนหน้า และบทแรกๆจะไม่ค่อยลงรายละเอียดในหัวข้อต่างๆ เพราะเราจะทำมันในบทท้ายๆ

คุณจะพบเนื้อหา 2 แบบในหนังสือเล่มนี้ คือบทที่เกี่ยวกับแนวคิด และบทที่เป็นโปรเจ็ค โดยในบทที่กล่าวถึง
เรื่องแนวคิด คุณจะได้เรียนรู้แง่มุมต่างๆของ Rust และในบทที่เป็นโปรเจ็ค คุณจะได้ทดลองสร้างโปรเจ็ค
เล็กๆไปด้วยกันกับเรา ด้วยการประยุกต์ใช้สิ่งที่ได้เรียนรู้มา โดยบทที่ 2, 12 และ 20 จะเป็นบทที่เราจะ
ทำโปรเจ็คด้วยกัน และบทที่เหลือจะเป็นเรื่องแนวคิด

บทที่ 1 จะอธิบายวิธีการติดตั้ง Rust วิธีเขียนโปรแกรม Hello, world! และการใช้ Cargo รวมไป
ถึงการใช้ package manager และเครื่องมือสำหรับ build โปรแกรม
บทที่ 2 จะเป็นกิจกรรมเบื้องต้นให้รู้จักภาษา Rust โดยจะครอบคลุมแนวคิดเรื่อง high level และจะ
ไปอธิบายรายละเอียดเพิ่มเติมในบทท้ายๆ ซึ่งถ้าคุณคันไม้คันมือแล้วตอนนี้ก็ไปเริ่มที่บทที่ 2 ได้เลย

Chapter 1 explains how to install Rust, how to write a Hello, world! program,
and how to use Cargo, Rust’s package manager and build tool. Chapter 2 is a
hands-on introduction to the Rust language. Here we cover concepts at a high
level, and later chapters will provide additional detail. If you want to get
your hands dirty right away, Chapter 2 is the place for that. At first, you
might even want to skip Chapter 3, which covers Rust features similar to those
of other programming languages, and head straight to Chapter 4 to learn about
Rust’s ownership system. However, if you’re a particularly meticulous learner
who prefers to learn every detail before moving on to the next, you might want
to skip Chapter 2 and go straight to Chapter 3, returning to Chapter 2 when
you’d like to work on a project applying the details you’ve learned.

Chapter 5 discusses structs and methods, and Chapter 6 covers enums, `match`
expressions, and the `if let` control flow construct. You’ll use structs and
enums to make custom types in Rust.

In Chapter 7, you’ll learn about Rust’s module system and about privacy rules
for organizing your code and its public Application Programming Interface
(API). Chapter 8 discusses some common collection data structures that the
standard library provides, such as vectors, strings, and hash maps. Chapter 9
explores Rust’s error-handling philosophy and techniques.

Chapter 10 digs into generics, traits, and lifetimes, which give you the power
to define code that applies to multiple types. Chapter 11 is all about testing,
which even with Rust’s safety guarantees is necessary to ensure your program’s
logic is correct. In Chapter 12, we’ll build our own implementation of a subset
of functionality from the `grep` command line tool that searches for text
within files. For this, we’ll use many of the concepts we discussed in the
previous chapters.

Chapter 13 explores closures and iterators: features of Rust that come from
functional programming languages. In Chapter 14, we’ll examine Cargo in more
depth and talk about best practices for sharing your libraries with others.
Chapter 15 discusses smart pointers that the standard library provides and the
traits that enable their functionality.

In Chapter 16, we’ll walk through different models of concurrent programming
and talk about how Rust helps you to program in multiple threads fearlessly.
Chapter 17 looks at how Rust idioms compare to object-oriented programming
principles you might be familiar with.

Chapter 18 is a reference on patterns and pattern matching, which are powerful
ways of expressing ideas throughout Rust programs. Chapter 19 contains a
smorgasbord of advanced topics of interest, including unsafe Rust, macros, and
more about lifetimes, traits, types, functions, and closures.

In Chapter 20, we’ll complete a project in which we’ll implement a low-level
multithreaded web server!

Finally, some appendixes contain useful information about the language in a
more reference-like format. Appendix A covers Rust’s keywords, Appendix B
covers Rust’s operators and symbols, Appendix C covers derivable traits
provided by the standard library, Appendix D covers some useful development
tools, and Appendix E explains Rust editions.

There is no wrong way to read this book: if you want to skip ahead, go for it!
You might have to jump back to earlier chapters if you experience any
confusion. But do whatever works for you.

<span id="ferris"></span>

An important part of the process of learning Rust is learning how to read the
error messages the compiler displays: these will guide you toward working code.
As such, we’ll provide many examples that don’t compile along with the error
message the compiler will show you in each situation. Know that if you enter
and run a random example, it may not compile! Make sure you read the
surrounding text to see whether the example you’re trying to run is meant to
error. Ferris will also help you distinguish code that isn’t meant to work:

| Ferris                                                                 | Meaning                                          |
|------------------------------------------------------------------------|--------------------------------------------------|
| <img src="img/ferris/does_not_compile.svg" class="ferris-explain"/>    | This code does not compile!                      |
| <img src="img/ferris/panics.svg" class="ferris-explain"/>              | This code panics!                                |
| <img src="img/ferris/unsafe.svg" class="ferris-explain"/>              | This code block contains unsafe code.            |
| <img src="img/ferris/not_desired_behavior.svg" class="ferris-explain"/>| This code does not produce the desired behavior. |

In most situations, we’ll lead you to the correct version of any code that
doesn’t compile.

## Source Code

The source files from which this book is generated can be found on
[GitHub][book].

[book]: https://github.com/rust-lang/book/tree/master/src
