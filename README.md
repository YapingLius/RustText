# RustText
# 第一阶段：学习rust
## 7月5日
把项目clone到clion上面，看了一下rust相关的书籍
## 7月6日
### 了解rust语法：
```rust

fn main(){} Rust程序入口函数，目前无返回值
let a = 10; 使用let声明变量，进行绑定，a不可变，
            没有指定a的类型，编译器会默认根据a的值为a推断a的类型为i32,
            结尾用分号结束
let b:i32 = 20;  主动指定b的类型为i32(有符号32位整数)
let mut c =  30i32;   值是32，类型是查i32
                       c 是可变的，mutable
let  d = 30_i32; 可读性会更好
let  e = add(add(a,b),add(c,d));使用一个函数的返回值，作为另一个函数的参数；
println!("(a+b)+(c+d)={}",e);    println!是宏调用，返回的是宏定义的代码块
                                该函数将指定的格式化字符串输出到标准输出中（控制台）
                                {}是占位符，执行过程中，会把e的值带入进来。
定义函数
fn add(i:i32,j:i32)->i32{
i+j//可以省略return
}
字符串使用双引号 "" 而不是单引号 ''，Rust 中单引号是留给单个字符类型（char）使用的
Rust 使用 {} 来作为格式化输出占位符，其它语言可能使用的是 %s，%d，%p 等，由于 println! 会自动推导出具体的类型，因此无需手动指定
### 变量命名
和其他语言一样，不要使用关键字，后面会依次补充关键字
### 变量绑定
let a = "hello world";这个过程就叫变量绑定（涉及到rust核心原则之一----所有权。任何内存对象都是有主人的，在一般情况下，是完全属于它的主人，绑定就是把这个对象绑定给一个变量，让这个变量成为它的主人。）
### 使用下划线开头忽略未使用的变量。
比如你刚开始一个项目，无法确定此变量会不会使用，可以用下划线作为变量名的开头，告诉rust不要警告未使用的变量
let _x = 5;
### 变量解构
let 不仅用于变量的绑定，还能进行复杂的解构。
```rust
fn main(){
let (a,mut b):(bool,bool) = (ture,false);//a = true,不可变，b = false ，可变
println!("a ={:?},b={:?}",a,b);
b = true;
assert_eq!(a,b);
运行结果：a = true, b = false
}
### 解构式赋值
```rust
struct Struct{
e :i32
}
fn main(){
 let (a, b, c, d, e);
    (a, b) = (1, 2);
    // _ 代表匹配一个值，但是我们不关心具体的值是什么，因此没有是一个变量名而是使用了 _
    [c, .., d, _] = [1, 2, 3, 4, 5];
    Struct { e, .. } = Struct { e: 5 };

    assert_eq!([1, 2, 1, 4, 5], [a, b, c, d, e]);
}
### 常量和变量：
常量不允许使用 mut。常量不仅仅默认不可变，而且自始至终不可变，因为常量在编译完成后，已经确定它的值。
常量使用 const 关键字而不是 let 关键字来声明，并且值的类型必须标注。

常量名为 MAX_POINTS，值设置为 100,000。（Rust 常量的命名约定是全部字母都使用大写，并使用下划线分隔单词，另外对数字字面量可插入下划线以提高可读性）：const MAX_POINTS: u32 = 100_000;

常量可以在任意作用域内声明，包括全局作用域，在声明的作用域内，常量在程序运行的整个过程中都有效。对于需要在多处代码共享一个不可变的值时非常有用

### 变量遮蔽
Rust 允许声明相同的变量名，在后面声明的变量会遮蔽掉前面声明的，
```rust
fn main() {
    let x = 5;
    // 在main函数的作用域内对之前的x进行遮蔽
    let x = x + 1;

    {
        // 在当前的花括号作用域内，对之前的x进行遮蔽
        let x = x * 2;
        println!("The value of x in the inner scope is: {}", x);
    }

    println!("The value of x is: {}", x);
}
这个程序首先将数值 5 绑定到 x，然后通过重复使用 let x = 来遮蔽之前的 x，并取原来的值加上 1，所以 x 的值变成了 6。第三个 let 语句同样遮蔽前面的 x，取之前的值并乘上 2，得到的 x 最终值为 12。
这和 mut 变量的使用是不同的，第二个 let 生成了完全不同的新变量，两个变量只是恰好拥有同样的名称，涉及一次内存对象的再分配 ，而 mut 声明的变量，可以修改同一个内存地址上的值，并不会发生内存对象的再分配，性能要更好。

## 基本类型
分为两个类型：基本类型和复合类型
基本类型：
数值类型: 有符号整数 (i8, i16, i32, i64, isize)、 无符号整数 (u8, u16, u32, u64, usize) 、浮点数 (f32, f64)、以及有理数、复数
字符串：字符串字面量和字符串切片 &str
布尔类型： true和false
字符类型: 表示单个 Unicode 字符，存储为 4 个字节
单元类型: 即 () ，其唯一的值也是 ()
### 数值类型
类型定义的形式统一为：有无符号 + 类型大小(位数)。无符号数表示数字只能取正数，而有符号则表示数字既可以取正数又可以取负数。就像在纸上写数字一样：当要强调符号时，数字前面可以带上正号或负号；然而，当很明显确定数字为正数时，就不需要加上正号了。有符号数字以补码形式存储。
每个有符号类型规定的数字范围是 -(2n - 1) ~ 2n - 1 - 1，其中 n 是该定义形式的位长度。因此 i8 可存储数字范围是 -(27) ~ 27 - 1，即 -128 ~ 127。无符号类型可以存储的数字范围是 0 ~ 2n - 1，所以 u8 能够存储的数字为 0 ~ 28 - 1，即 0 ~ 255。
isize 和 usize 类型取决于程序运行的计算机 CPU 类型： 若 CPU 是 32 位的，则这两个类型是 32 位的，同理，若 CPU 是 64 位，那么它们则是 64 位。
### 整形溢出
假设有一个 u8 ，它可以存放从 0 到 255 的值。那么当你将其修改为范围之外的值，比如 256，则会发生整型溢出。
在当使用 --release 参数进行 release 模式构建时，Rust 不检测溢出。相反，当检测到整型溢出时，Rust 会按照补码循环溢出（two’s complement wrapping）的规则处理。简而言之，大于该类型最大值的数值会被补码转换成该类型能够支持的对应数字的最小值。比如在 u8 的情况下，256 变成 0，257 变成 1，
要显式处理可能的溢出，可以使用标准库针对原始数字类型提供的这些方法：
使用 wrapping_* 方法在所有模式下都按照补码循环溢出规则处理，例如 wrapping_add
如果使用 checked_* 方法时发生溢出，则返回 None 值
使用 overflowing_* 方法返回该值和一个指示是否存在溢出的布尔值
使用 saturating_* 方法使值达到最小值或最大值

### 浮点类型
```rust

fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}

fn main() {
    let abc: (f32, f32, f32) = (0.1, 0.2, 0.3);
    let xyz: (f64, f64, f64) = (0.1, 0.2, 0.3);

    println!("abc (f32)");
    println!("   0.1 + 0.2: {:x}", (abc.0 + abc.1).to_bits());
    println!("         0.3: {:x}", (abc.2).to_bits());
    println!();

    println!("xyz (f64)");
    println!("   0.1 + 0.2: {:x}", (xyz.0 + xyz.1).to_bits());
    println!("         0.3: {:x}", (xyz.2).to_bits());
    println!();

    assert!(abc.0 + abc.1 == abc.2);
    assert!(xyz.0 + xyz.1 == xyz.2);
}
结果：
abc (f32)
   0.1 + 0.2: 3e99999a
         0.3: 3e99999a

xyz (f64)
   0.1 + 0.2: 3fd3333333333334
         0.3: 3fd3333333333333

对 f32 类型做加法时，0.1 + 0.2 的结果是 3e99999a，0.3 也是 3e99999a，因此 f32 下的 0.1 + 0.2 == 0.3 通过测试，但是到了 f64 类型时，结果就不一样了，因为 f64 精度高很多，因此在小数点非常后面发生了一点微小的变化，0.1 + 0.2 以 4 结尾，但是 0.3 以3结尾，这个细微区别导致 f64 下的测试失败了，并且抛出了异常。

出于防御性编程的考虑，可以使用 is_nan() 等方法，可以用来判断一个数值是否是 NaN ：
fn main() {
    let x = (-42.0_f32).sqrt();
    if x.is_nan() {
        println!("未定义的数学行为")
    }
}
```rust
fn main() {
  // 编译器会进行自动推导，给予twenty i32的类型
  let twenty = 20;
  // 类型标注
  let twenty_one: i32 = 21;
  // 通过类型后缀的方式进行类型标注：22是i32类型
  let twenty_two = 22i32;

  // 只有同样类型，才能运算
  let addition = twenty + twenty_one + twenty_two;
  println!("{} + {} + {} = {}", twenty, twenty_one, twenty_two, addition);

  // 对于较长的数字，可以用_进行分割，提升可读性
  let one_million: i64 = 1_000_000;
  println!("{}", one_million.pow(2));

  // 定义一个f32数组，其中42.0会自动被推导为f32类型
  let forty_twos = [
    42.0,
    42f32,
    42.0_f32,
  ];

  // 打印数组中第一个值，并控制小数位为2位
  println!("{:.2}", forty_twos[0]);
}
20 + 21 + 22 = 63
1000000000000
42.00
### 位运算
运算符	说明
& 位与	相同位置均为1时则为1，否则为0
| 位或	相同位置只要有1时则为1，否则为0
^ 异或	相同位置不相同则为1，相同则为0
! 位非	把位中的0和1相互取反，即0置为1，1置为0
<< 左移	所有位向左移动指定位数，右位补零
>> 右移	所有位向右移动指定位数，左位补零
```rust
fn main() {
    // 二进制为00000010
    let a:i32 = 2;
    // 二进制为00000011
    let b:i32 = 3;

    println!("(a & b) value is {}", a & b);

    println!("(a | b) value is {}", a | b);

    println!("(a ^ b) value is {}", a ^ b);

    println!("(!b) value is {} ", !b);

    println!("(a << b) value is {}", a << b);

    println!("(a >> b) value is {}", a >> b);

    let mut a = a;
    // 注意这些计算符除了!之外都可以加上=进行赋值 (因为!=要用来判断不等于)
    a <<= b;
    println!("(a << b) value is {}", a);
}
(a & b) value is 2
(a | b) value is 3
(a ^ b) value is 1
(!b) value is -4 
(a << b) value is 16
(a >> b) value is 0
(a << b) value is 16

###  序列Range
例如 1..5，生成从 1 到 4 的连续数字，不包含 5 ；1..=5，生成从 1 到 5 的连续数字，包含 5

for i in 1..=5 {
    println!("{}",i);
}

1
2
3
4
5
序列只允许用于数字或字符类型，原因是：它们可以连续
for i in 'a'..='z' {
    println!("{}",i);
}

### 有理数和复数
社区已经开发出高质量的 Rust 数值库：num。
按照以下步骤来引入 num 库：
创建新工程 cargo new complex-num && cd complex-num
在 Cargo.toml 中的 [dependencies] 下添加一行 num = "0.4.0"
将 src/main.rs 文件中的 main 函数替换为下面的代码
运行 cargo run
use num::complex::Complex;
 fn main() {
   let a = Complex { re: 2.1, im: -1.2 };
   let b = Complex::new(11.1, 22.2);
   let result = a + b;

   println!("{} + {}i", result.re, result.im)
 }
13.2 + 21i
Rust 拥有相当多的数值类型. 因此你需要熟悉这些类型所占用的字节数，这样就知道该类型允许的大小范围以及你选择的类型是否能表达负数
类型转换必须是显式的. Rust 永远也不会偷偷把你的 16bit 整数转换成 32bit 整数
Rust 的数值上可以使用方法. 例如你可以用以下方法来将 13.14 取整：13.14_f32.round()，在这里我们使用了类型后缀，因为编译器需要知道 13.14 的具体类型
## 7月8日
被问及一个问题：我们知道rust可以使用相同的变量名
  let x = "world";
    let x = "hello";
    println!("Hello {}!", x);
    结果：此时的x的值，hello覆盖了world，所以显示结果为Hello hello!
        let x = "world";
    let x = "hell";
    println!("Hello {}!", x);  结果为：Hello hell!
### 字符、布尔、单元类型
### 字符类型char
```rust
fn main() {
    let c = 'z';
    let z = 'ℤ';
    let g = '国';
    let heart_eyed_cat = '😻';
}
在 Rust 语言中这些都是字符，Rust 的字符不仅仅是 ASCII，所有的 Unicode 值都可以作为 Rust 字符，包括单个的中文、日文、韩文、emoji 表情符号等等，都是合法的字符类型。Unicode 值的范围从 U+0000 ~ U+D7FF 和 U+E000 ~ U+10FFFF。
由于 Unicode 都是 4 个字节编码，因此字符类型也是占用 4 个字节：
fn main() {
    let x = '中';
    println!("字符'中'占用了{}字节的内存大小",std::mem::size_of_val(&x));
    //字符'中'占用了4字节的内存大小
}
### bool
Rust 中的布尔类型有两个可能的值：true 和 false，布尔值占用内存的大小为 1 个字节：
```rust
fn main() {
    let t = true;

    let f: bool = false; // 使用类型标注,显式指定f的类型

    if f {
        println!("这是段毫无意义的代码");
    }
}
###单元类型
单元类型就是 () ，唯一的值也是 () ，没错， main 函数就返回这个单元类型 ()，
没有返回值的函数在 Rust 中是有单独的定义的：发散函数( diverge function )
println!() 的返回值也是单元类型 ()。
### 语句和表达式
Rust 的函数体是由一系列语句组成，最后由一个表达式来返回值，
```rust
fn add_with_extra(x: i32, y: i32) -> i32 {
    let x = x + 1; // 语句
    let y = y + 5; // 语句
    x + y // 表达式
}
语句会执行一些操作但是不会返回一个值，而表达式会在求值后返回一个值，因此在上述函数体的三行代码中，前两行是语句，最后一行是表达式。
### 语句
```rust
let a = 8;
let b: Vec<f64> = Vec::new();
let (a, c) = ("hi", false);
它们完成了一个具体的操作，但是并没有返回值，因此是语句。
###  表达式
表达式会进行求值，然后返回一个值。例如 5 + 6，在求值后，返回值 11，因此它就是一条表达式。
表达式可以成为语句的一部分，例如 let y = 6 中，6 就是一个表达式，它在求值后返回一个值 6（有些反直觉，但是确实是表达式）。
调用一个函数是表达式，因为会返回一个值，调用宏也是表达式，用花括号包裹最终返回一个值的语句块也是表达式，总之，能返回值，它就是表达式:
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {}", y);
}
该语句块是表达式的原因是：它的最后一行是表达式，返回了 x + 1 的值，注意 x + 1 不能以分号结尾，否则就会从表达式变成语句， 表达式不能包含分号。这一点非常重要，一旦你在表达式后加上分号，它就会变成一条语句，再也不会返回一个值，请牢记！

最后，表达式如果不返回任何值，会隐式地返回一个 () 。
fn main() {
    assert_eq!(ret_unit_type(), ())
}

fn ret_unit_type() {
    let x = 1;
    // if 语句块也是一个表达式，因此可以用于赋值，也可以直接返回
    if (x > 1) {

    }
}
### 函数
```rust
fn main() {
fn add(i: i32, j: i32) -> i32 {
   i + j
 }
}
声明函数的关键字 fn ,函数名 add()，参数 i 和 j，参数类型和返回值类型都是 i32，
函数要点
函数名和变量名使用蛇形命名法(snake case)，例如 fn add_two() -> {}
函数的位置可以随便放，Rust 不关心我们在哪里定义了函数，只要有定义即可
每个函数参数都需要标注类型
函数参数
Rust 是强类型语言，因此需要你为每一个函数参数都标识出它的具体类型，例如：
```rust
fn main() {
    another_function(5, 6.1);
}

fn another_function(x: i32, y: f32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
    The value of x is: 5
    The value of y is: 6.1
}
another_function 函数有两个参数，其中 x 是 i32 类型，y 是 f32 类型，然后在该函数内部，打印出这两个值。这里去掉 x 或者 y 的任何一个的类型，都会报错：
函数
another_function 函数有两个参数，其中 x 是 i32 类型，y 是 f32 类型，然后在该函数内部，打印出这两个值。这里去掉 x 或者 y 的任何一个的类型，都会报错：f机会
another_function 函数有两个参数，其中 x 是 i32 类型，y 是 f32 类型，然后在该函数内部，打印出这两个值。这里去掉 x 或者 y 的任何一个的类型，都会报错
函数返回
在 Rust 中函数就是表达式，因此我们可以把函数的返回值直接赋给调用者。
函数的返回值就是函数体最后一条表达式的返回值，当然我们也可以使用 return 提前返回，下面的函数使用最后一条表达式来返回一个值：
fn plus_five(x:i32) -> i32 {
    x + 5
}
fn main() {
    let x = plus_five(5);

    println!("The value of x is: {}", x);
    The value of x is: 10
}
x + 5 是一条表达式，求值后，返回一个值，因为它是函数的最后一行，因此该表达式的值也是函数的返回值。
1.let x = plus_five(5)，说明我们用一个函数的返回值来初始化 x 变量，因此侧面说明了在 Rust 中函数也是表达式，这种写法等同于 let x = 5 + 5;
2.x + 5 没有分号，因为它是一条表达式，这个在上一节中我们也有详细介绍
fn plus_or_minus(x:i32) -> i32 {
    if x > 5 {
        return x - 5
    }

    x + 5
}

fn main() {
    let x = plus_or_minus(5);

    println!("The value of x is: {}", x);
    The value of x is: 10
}
plus_or_minus 函数根据传入 x 的大小来决定是做加法还是减法，若 x > 5 则通过 return 提前返回 x - 5 的值,否则返回 x + 5 的值。
### rust中的特殊返回值
1.无返回值()
例如单元类型 ()，是一个零长度的元组。它没啥作用，但是可以用来表达一个函数没有返回值：
函数没有返回值，那么返回一个 ()
通过 ; 结尾的表达式返回一个 ()
report 函数会隐式返回一个 ()：
#![allow(unused)]
use std::fmt::Debug;

fn report<T: Debug>(item: T) {
  println!("{:?}", item);
}
与上面的函数返回值相同，但是下面的函数显式的返回了 ()：
#![allow(unused)]
fn clear(text: &mut String) -> () {
  *text = String::from("");
}
在实际编程中，你会经常在错误提示中看到该 () 的身影出没，假如你的函数需要返回一个 u32 值，但是如果你不幸的以 表达式; 的方式作为函数的最后一行代码，就会报错：
fn add(x:u32,y:u32) -> u32 {
    x + y;
}
2.永不返回的发散函数 !
当用 ! 作函数返回类型的时候，表示该函数永不返回( diverge function )，特别的，这种语法往往用做会导致程序崩溃的函数：

#![allow(unused)]
fn dead_end() -> ! {
  panic!("你已经到了穷途末路，崩溃吧！");
}
下面的函数创建了一个无限循环，该循环永不跳出，因此函数也永不返回：
fn forever() -> ! {
  loop {
    //...
  };
}
### 所有权和借用
Rust 之所以能成为万众瞩目的语言，就是因为其内存安全性，在以往，内存安全几乎都是通过 GC 的方式实现，但是 GC 会引来性能、内存占用以及 Stop the world 等问题，在高性能场景和系统编程上是不可接受的，因此 Rust 采用了与(错)众(误)不(之)同(源)的方式：所有权系统。
所有的程序都必须和计算机内存打交道，如何从内存中申请空间来存放程序的运行内容，如何在不需要的时候释放这些空间，成了重中之重，
在计算机语言不断演变过程中，出现了三种流派：
1.垃圾回收机制(GC)，在程序运行时不断寻找不再使用的内存，典型代表：Java、Go
2.手动管理内存的分配和释放, 在程序中，通过函数调用的方式来申请和释放内存，典型代表：C++
3.通过所有权来管理内存，编译器在编译时会根据一系列规则进行检查
其中 Rust 选择了第三种，最妙的是，这种检查只发生在编译期，因此对于程序运行期，不会有任何性能上的损失。

先来看看一段来自 C 语言的糟糕代码：
int* foo() {
    int a;          // 变量a的作用域开始
    a = 100;
    char *c = "xyz";   // 变量c的作用域开始
    return &a;
}                   // 变量a和c的作用域结束
这段代码虽然可以编译通过，但是其实非常糟糕，变量 a 和 c 都是局部变量，函数结束后将局部变量 a 的地址返回，但局部变量 a 存在栈中，在离开作用域后，a 所申请的栈上内存都会被系统回收，从而造成了 悬空指针(Dangling Pointer) 的问题。这是一个非常典型的内存安全问题，虽然编译可以通过，但是运行的时候会出现错误, 很多编程语言都存在。
栈(Stack)与堆(Heap)
栈和堆的核心目标就是为程序在运行时提供可供使用的内存空间。
1.栈
栈按照顺序存储值并以相反顺序取出值，这也被称作后进先出。想象一下一叠盘子：当增加更多盘子时，把它们放在盘子堆的顶部，当需要盘子时，再从顶部拿走。不能从中间也不能从底部增加或拿走盘子！
增加数据叫做进栈，移出数据则叫做出栈。
因为上述的实现方式，栈中的所有数据都必须占用已知且固定大小的内存空间，假设数据大小是未知的，那么在取出数据时，你将无法取到你想要的数据。
2.堆
与栈不同，对于大小未知或者可能变化的数据，我们需要将它存储在堆上。
当向堆上放入数据时，需要请求一定大小的内存空间。操作系统在堆的某处找到一块足够大的空位，把它标记为已使用，并返回一个表示该位置地址的指针, 该过程被称为在堆上分配内存，有时简称为 “分配”(allocating)。

接着，该指针会被推入栈中，因为指针的大小是已知且固定的，在后续使用过程中，你将通过栈中的指针，来获取数据在堆上的实际内存位置，进而访问该数据。

由上可知，堆是一种缺乏组织的数据结构。想象一下去餐馆就座吃饭: 进入餐馆，告知服务员有几个人，然后服务员找到一个够大的空桌子（堆上分配的内存空间）并领你们过去。如果有人来迟了，他们也可以通过桌号（栈上的指针）来找到你们坐在哪。
性能区别
写入方面：入栈比在堆上分配内存要快，因为入栈时操作系统无需分配新的空间，只需要将新数据放入栈顶即可。相比之下，在堆上分配内存则需要更多的工作，这是因为操作系统必须首先找到一块足够存放数据的内存空间，接着做一些记录为下一次分配做准备。

读取方面：得益于 CPU 高速缓存，使得处理器可以减少对内存的访问，高速缓存和内存的访问速度差异在 10 倍以上！栈数据往往可以直接存储在 CPU 高速缓存中，而堆数据只能存储在内存中。访问堆上的数据比访问栈上的数据慢，因为必须先访问栈再通过栈上的指针来访问内存。

因此，处理器处理和分配在栈上数据会比在堆上的数据更加高效。

所有权与堆栈
当你的代码调用一个函数时，传递给函数的参数（包括可能指向堆上数据的指针和函数的局部变量）依次被压入栈中，当函数调用结束时，这些值将被从栈中按照相反的顺序依次移除。
因为堆上的数据缺乏组织，因此跟踪这些数据何时分配和释放是非常重要的，否则堆上的数据将产生内存泄漏 —— 这些数据将永远无法被回收。这就是 Rust 所有权系统为我们提供的强大保障。

对于其他很多编程语言，你确实无需理解堆栈的原理，但是在 Rust 中，明白堆栈的原理，对于我们理解所有权的工作原理会有很大的帮助。
### 所有权原则
1.Rust 中每一个值都被一个变量所拥有，该变量被称为值的所有者
2.一个值同时只能被一个变量所拥有，或者说一个值只能拥有一个所有者
3.当所有者(变量)离开作用域范围时，这个值将被丢弃(drop)
变量作用域
作用域是一个变量在程序中有效的范围, 假如有这样一个变量：let s = "hello"
变量 s 绑定到了一个字符串字面值，该字符串字面值是硬编码到程序代码中的。s 变量从声明的点开始直到当前作用域的结束都是有效的：

{                      // s 在这里无效，它尚未声明
    let s = "hello";   // 从此处起，s 是有效的

    // 使用 s
}    
### 简单介绍 String 类型
我们已经见过字符串字面值 let s ="hello"，s 是被硬编码进程序里的字符串值（类型为 &str ）。字符串字面值是很方便的，但是它并不适用于所有场景。原因有二：
1.字符串字面值是不可变的，因为被硬编码到程序代码中
2.并非所有字符串的值都能在编写代码时得知
字符串是需要程序运行时，通过用户动态输入然后存储在内存中的，这种情况，字符串字面值就完全无用武之地。 为此，Rust 为我们提供动态字符串类型: String, 该类型被分配到堆上，因此可以动态伸缩，也就能存储在编译时大小未知的文本。

可以使用下面的方法基于字符串字面量来创建 String 类型：let s = String::from("hello");
:: 是一种调用操作符，这里表示调用 String 中的 from 方法，因为 String 存储在堆上是动态的，你可以这样修改它：
fn main() {
let mut s = String::from("hello");
s.push_str(", world!"); // push_str() 在字符串后追加字面值
println!("{}", s); // 将打印 `hello, world!`
}

### 变量绑定背后的数据交互
转移所有权

#![allow(unused)]
fn main() {
let x = 5;
let y = x;
}
将 5 绑定到变量 x；接着拷贝 x 的值赋给 y，最终 x 和 y 都等于 5，因为整数是 Rust 基本数据类型，是固定大小的简单值，因此这两个值都是通过自动拷贝的方式来赋值的，都被存在栈中，完全无需在堆上分配内存

#![allow(unused)]
fn main() {
let s1 = String::from("hello");
let s2 = s1;
}
对于基本类型（存储在栈上），Rust 会自动拷贝，但是 String 不是基本类型，而且是存储在堆上的，因此不能自动拷贝。
String 类型是一个复杂类型，由存储在栈中的堆指针、字符串长度、字符串容量共同组成，其中堆指针是最重要的，它指向了真实存储字符串内容的堆内存，至于长度和容量，如果你有 Go 语言的经验，这里就很好理解：容量是堆内存分配空间的大小，长度是目前已经使用的大小。
String 类型指向了一个堆上的空间，这里存储着它的真实数据, 下面对上面代码中的 let s2 = s1 分成两种情况讨论：

1.拷贝 String 和存储在堆上的字节数组 如果该语句是拷贝所有数据(深拷贝)，那么无论是 String 本身还是底层的堆上数据，都会被全部拷贝，这对于性能而言会造成非常大的影响

2.只拷贝 String 本身 这样的拷贝非常快，因为在 64 位机器上就拷贝了 8字节的指针、8字节的长度、8字节的容量，总计 24 字节，但是带来了新的问题，还记得我们之前提到的所有权规则吧？其中有一条就是：一个值只允许有一个所有者，而现在这个值（堆上的真实字符串数据）有了两个所有者：s1 和 s2。
，就假定一个值可以拥有两个所有者，会发生什么呢？

当变量离开作用域后，Rust 会自动调用 drop 函数并清理变量的堆内存。不过由于两个 String 变量指向了同一位置。这就有了一个问题：当 s1 和 s2 离开作用域，它们都会尝试释放相同的内存。这是一个叫做 二次释放（double free） 的错误，也是之前提到过的内存安全性 BUG 之一。两次释放（相同）内存会导致内存污染，它可能会导致潜在的安全漏洞。
因此，Rust 这样解决问题：当 s1 赋予 s2 后，Rust 认为 s1 不再有效，因此也无需在 s1 离开作用域后 drop 任何东西，这就是把所有权从 s1 转移给了 s2，s1 在被赋予 s2 后就马上失效了。
现在再回头看看之前的规则，相信大家已经有了更深刻的理解：

1.Rust 中每一个值都被一个变量所拥有，该变量被称为值的所有者
2.一个值同时只能被一个变量所拥有，或者说一个值只能拥有一个所有者
3.当所有者(变量)离开作用域范围时，这个值将被丢弃(drop)
在其他语言中听说过术语 浅拷贝(shallow copy) 和 深拷贝(deep copy)，那么拷贝指针、长度和容量而不拷贝数据听起来就像浅拷贝，但是又因为 Rust 同时使第一个变量 s1 无效了，因此这个操作被称为 移动(move)，而不是浅拷贝
这样就解决了我们之前的问题，s1 不再指向任何数据，只有 s2 是有效的，当 s2 离开作用域，它就会释放内存。 相信此刻，你应该明白了，为什么 Rust 称呼 let a = b 为变量绑定了吧？

再来看一段代码:
fn main() {
    let x: &str = "hello, world";
    let y = x;
    println!("{},{}",x,y);
}
这段代码和之前的 String 有一个本质上的区别：在 String 的例子中 s1 持有了通过String::from("hello") 创建的值的所有权，而这个例子中，x 只是引用了存储在二进制中的字符串 "hello, world"，并没有持有所有权。
因此 let y = x 中，仅仅是对该引用进行了拷贝，此时 y 和 x 都引用了同一个字符串。如果还不理解也没关系，当学习了下一章节 "引用与借用" 后，大家自然而言就会理解。
### 克隆(深拷贝)
首先，Rust 永远也不会自动创建数据的 “深拷贝”。因此，任何自动的复制都不是深拷贝，可以被认为对运行时性能影响较小。
如果我们确实需要深度复制 String 中堆上的数据，而不仅仅是栈上的数据，可以使用一个叫做 clone 的方法。
#![allow(unused)]
fn main() {
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {}, s2 = {}", s1, s2);
}
这段代码能够正常运行，因此说明 s2 确实完整的复制了 s1 的数据。
拷贝(浅拷贝)
浅拷贝只发生在栈上，因此性能很高，在日常编程中，浅拷贝无处不在。

#![allow(unused)]
fn main() {
let x = 5;
let y = x;
println!("x = {}, y = {}", x, y);
}
原因是像整型这样的基本类型在编译时是已知大小的，会被存储在栈上，所以拷贝其实际的值是快速的。这意味着没有理由在创建变量 y 后使 x 无效（x、y 都仍然有效）。换句话说，这里没有深浅拷贝的区别，因此这里调用 clone 并不会与通常的浅拷贝有什么不同，我们可以不用管它（可以理解成在栈上做了深拷贝）。

Rust 有一个叫做 Copy 的特征，可以用在类似整型这样在栈中存储的类型。如果一个类型拥有 Copy 特征，一个旧的变量在被赋值给其他变量后仍然可用。
规则： 任何基本类型的组合可以 Copy ，不需要分配内存或某种形式资源的类型是可以 Copy 的。如下是一些 Copy 的类型：
* 所有整数类型，比如 u32。
* 布尔类型，bool，它的值是 true 和 false。
* 所有浮点数类型，比如 f64。
* 字符类型，char。
* 元组，当且仅当其包含的类型也都是 Copy 的时候。比如，(i32, i32) 是 Copy 的，但 (i32, String) 就不是。
* 不可变引用 &T ，例如转移所有权中的最后一个例子，但是注意: 可变引用 &mut T 是不可以 Copy的
### 函数传值与返回
将值传递给函数，一样会发生 移动 或者 复制，就跟 let 语句一样，下面的代码展示了所有权、作用域的规则：
```rust
fn main() {
    let s = String::from("hello");  // s 进入作用域

    takes_ownership(s);             // s 的值移动到函数里 ...
                                    // ... 所以到这里不再有效

    let x = 5;                      // x 进入作用域

    makes_copy(x);                  // x 应该移动函数里，
                                    // 但 i32 是 Copy 的，所以在后面可继续使用 x

} // 这里, x 先移出了作用域，然后是 s。但因为 s 的值已被移走，
  // 所以不会有特殊操作

fn takes_ownership(some_string: String) { // some_string 进入作用域
    println!("{}", some_string);
} // 这里，some_string 移出作用域并调用 `drop` 方法。占用的内存被释放

fn makes_copy(some_integer: i32) { // some_integer 进入作用域
    println!("{}", some_integer);
} // 这里，some_integer 移出作用域。不会有特殊操作
hello
5

同样的，函数返回值也有所有权，
fn main() {
    let s1 = gives_ownership();         // gives_ownership 将返回值
                                        // 移给 s1

    let s2 = String::from("hello");     // s2 进入作用域

    let s3 = takes_and_gives_back(s2);  // s2 被移动到
                                        // takes_and_gives_back 中,
                                        // 它也将返回值移给 s3
} // 这里, s3 移出作用域并被丢弃。s2 也移出作用域，但已被移走，
  // 所以什么也不会发生。s1 移出作用域并被丢弃

fn gives_ownership() -> String {             // gives_ownership 将返回值移动给
                                             // 调用它的函数

    let some_string = String::from("hello"); // some_string 进入作用域.

    some_string                              // 返回 some_string 并移出给调用的函数
}

// takes_and_gives_back 将传入字符串并返回该值
fn takes_and_gives_back(a_string: String) -> String { // a_string 进入作用域

    a_string  // 返回 a_string 并移出给调用的函数
}
所有权很强大，避免了内存的不安全性，但是也带来了一个新麻烦： 总是把一个值传来传去来使用它。 传入一个函数，很可能还要从该函数传出去，结果就是语言表达变得非常啰嗦，幸运的是，Rust 提供了新功能解决这个问题。
### 引用和借用
Rust 通过 借用(Borrowing) 这个概念来达成上述的目的，获取变量的引用，称之为借用(borrowing)。正如现实生活中，如果一个人拥有某样东西，你可以从他那里借来，当使用完毕后，也必须要物归原主。

### 引用和解引用
常规引用是一个指针类型，指向了对象存储的内存地址。在下面代码中，我们创建一个 i32 值的引用 y，然后使用解引用运算符来解出 y 所使用的值:
```rust
fn main() {
    let x = 5;
    let y = &x;
    assert_eq!(5, x);
    assert_eq!(5, *y);
}
量 x 存放了一个 i32 值 5。y 是 x 的一个引用。可以断言 x 等于 5。然而，如果希望对 y 的值做出断言，必须使用 *y 来解出引用所指向的值（也就是解引用）。一旦解引用了 y，就可以访问 y 所指向的整型值并可以与 5 做比较。

### 不可变引用
我们用 s1 的引用作为参数传递给 calculate_length 函数，而不是把 s1 的所有权转移给该函数：
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}

The length of 'hello' is 5.

能注意到两点：

1.无需像上章一样：先通过函数参数传入所有权，然后再通过函数返回来传出所有权，代码更加简洁
2.calculate_length 的参数 s 类型从 String 变为 &String
这里，& 符号即是引用，它们允许你使用值，但是不获取所有权，如图所示：
通过 &s1 语法，我们创建了一个指向 s1 的引用，但是并不拥有它。因为并不拥有这个值，当引用离开作用域后，其指向的值也不会被丢弃。
同理，函数 calculate_length 使用 & 来表明参数 s 的类型是一个引用：
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world");
}
会有bug
### 可变引用
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
首先，声明 s 是可变类型，其次创建一个可变的引用 &mut s 和接受可变引用参数 some_string: &mut String 的函数。
可变引用同时只能存在一个
不过可变引用并不是随心所欲、想用就用的，它有一个很大的限制： 同一作用域，特定数据只能有一个可变引用：

#![allow(unused)]
fn main() {
let mut s = String::from("hello");
let r1 = &mut s;
let r2 = &mut s;
println!("{}, {}", r1, r2);
}报错
这段代码出错的原因在于，第一个可变借用 r1 必须要持续到最后一次使用的位置 println!，在 r1 创建和最后一次使用之间，我们又尝试创建第二个可变引用 r2。

很多时候，大括号可以帮我们解决一些编译不通过的问题，通过手动限制变量的作用域：

#![allow(unused)]
fn main() {
let mut s = String::from("hello");

{
    let r1 = &mut s;

} // r1 在这里离开了作用域，所以我们完全可以创建一个新的引用

let r2 = &mut s;
}
可变引用与不可变引用不能同时存在
下面的代码会导致一个错误：

#![allow(unused)]
fn main() {
let mut s = String::from("hello");
let r1 = &s; // 没问题
let r2 = &s; // 没问题
let r3 = &mut s; // 大问题

println!("{}, {}, and {}", r1, r2, r3);
}

Rust 的编译器一直在优化，早期的时候，引用的作用域跟变量作用域是一致的，这对日常使用带来了很大的困扰，你必须非常小心的去安排可变、不可变变量的借用，免得无法通过编译，例如以下代码：
fn main() {
   let mut s = String::from("hello");

    let r1 = &s;
    let r2 = &s;
    println!("{} and {}", r1, r2);
    // 新编译器中，r1,r2作用域在这里结束

    let r3 = &mut s;
    println!("{}", r3);
} // 老编译器中，r1、r2、r3作用域在这里结束
  // 新编译器中，r3作用域在这里结束
在老版本的编译器中（Rust 1.31 前），将会报错，因为 r1 和 r2 的作用域在花括号 } 处结束，那么 r3 的借用就会触发 无法同时借用可变和不可变的规则。
但是在新的编译器中，该代码将顺利通过，因为 引用作用域的结束位置从花括号变成最后一次使用的位置，因此 r1 借用和 r2 借用在 println! 后，就结束了，此时 r3 可以顺利借用到可变引用。
### NLL
对于这种编译器优化行为，Rust 专门起了一个名字 —— Non-Lexical Lifetimes(NLL)，专门用于找到某个引用在作用域(})结束前就不再被使用的代码位置。
### 悬垂引用(Dangling References)
悬垂引用也叫做悬垂指针，意思为指针指向某个值后，这个值被释放掉了，而指针仍然存在，其指向的内存可能不存在任何值或已被其它变量重新使用。
让我们尝试创建一个悬垂引用，Rust 会抛出一个编译时错误：
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s
}
错误信息引用了一个我们还未介绍的功能：生命周期(lifetimes)。不过，即使你不理解生命周期，也可以通过错误信息知道这段代码错误的关键信息：
仔细看看 dangle 代码的每一步到底发生了什么：

fn dangle() -> &String { // dangle 返回一个字符串的引用

    let s = String::from("hello"); // s 是一个新字符串

    &s // 返回字符串 s 的引用
} // 这里 s 离开作用域并被丢弃。其内存被释放。
  // 危险！
  因为 s 是在 dangle 函数内创建的，当 dangle 的代码执行完毕后，s 将被释放, 但是此时我们又尝试去返回它的引用。这意味着这个引用会指向一个无效的 String，这可不对！
  
  其中一个很好的解决方法是直接返回 String：
fn no_dangle() -> String {
    let s = String::from("hello");

    s
}
  这样就没有任何错误了，最终 String 的 所有权被转移给外面的调用者。
  ### 复合类型
  复合类型是由其它类型组合而成的，最典型的就是结构体 struct 和枚举 enum。
  来看一段代码，它使用我们之前学过的内容来构建文件操作：


#![allow(unused_variables)]
type File = String;

fn open(f: &mut File) -> bool {
    true
}
fn close(f: &mut File) -> bool {
    true
}

#[allow(dead_code)]
fn read(f: &mut File, save_to: &mut Vec<u8>) -> ! {
    unimplemented!()
}

fn main() {
    let mut f1 = File::from("f1.txt");
    open(&mut f1);
    //read(&mut f1, &mut vec![]);
    close(&mut f1);
}
有的方法只提供 API 接口，但是不提供具体实现。此外，有的变量在声明之后并未使用，因此在这个阶段我们需要排除一些编译器噪音（Rust 在编译的时候会扫描代码，变量声明后未使用会以 warning 警告的形式进行提示），引入 #![allow(unused_variables)] 属性标记，该标记会告诉编译器忽略未使用的变量，不要抛出 warning 警告,具体的常见编译器属性你可以在这里查阅：编译器属性标记。
  read 函数也非常有趣，它返回一个 ! 类型，这个表明该函数是一个发散函数，不会返回任何值，包括 ()。unimplemented!() 告诉编译器该函数尚未实现，unimplemented!() 标记通常意味着我们期望快速完成主要代码，回头再通过搜索这些标记来完成次要代码，类似的标记还有 todo!()，当代码执行到这种未实现的地方时，程序会直接报错。你可以反注释 read(&mut f1, &mut vec![]); 这行，然后再观察下结果。
  ### 字符串
  ```rust
  fn main() {
  let my_name = "Pascal";
  greet(my_name);
}

fn greet(name: String) {
  println!("Hello, {}!", name);
}
Bingo，果然报错了，编译器提示 greet 函数需要一个 String 类型的字符串，却传入了一个 &str 类型的字符串，相信读者心中现在一定有几头草泥马呼啸而过，怎么字符串也能整出这么多花活？
### slice
切片并不是 Rust 独有的概念，在 Go 语言中就非常流行，它允许你引用集合中部分连续的元素序列，而不是引用整个集合。
对于字符串而言，切片就是对 String 类型中某一部分的引用，它看起来像这样：
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
hello 没有引用整个 String s，而是引用了 s 的一部分内容，通过 [0..5] 的方式来指定。 
 这就是创建切片的语法，使用方括号包括的一个序列：[开始索引..终止索引]，其中开始索引是切片中第一个元素的索引位置，而终止索引是最后一个元素后面的索引位置，也就是这是一个 右半开区间。在切片数据结构内部会保存开始的位置和切片的长度，其中长度是通过 终止索引 - 开始索引 的方式计算得来的。
对于 let world = &s[6..11]; 来说，world 是一个切片，该切片的指针指向 s 的第 7 个字节(索引从 0 开始, 6 是第 7 个字节)，且该切片的长度是 5 个字节。
在使用 Rust 的 .. range 序列语法时，如果你想从索引 0 开始，可以使用如下的方式，这两个是等效的：
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];
同样的，如果你的切片想要包含 String 的最后一个字节，则可以这样使用:
let s = String::from("hello");
let len = s.len();
let slice = &s[4..len];
let slice = &s[4..];
 你也可以截取完整的 String 切片：
let s = String::from("hello");
let len = s.len();

let slice = &s[0..len];
let slice = &s[..];
 在对字符串使用切片语法时需要格外小心，切片的索引必须落在字符之间的边界位置，也就是 UTF-8 字符的边界，例如中文在 UTF-8 中占用三个字节,下面的代码就会崩溃:

 let s = "中国人";
 let a = &s[0..2];
 println!("{}",a);
因为我们只取 s 字符串的前两个字节，但是本例中每个汉字占用三个字节，因此没有落在边界处，也就是连 中 字都取不完整，此时程序会直接崩溃退出，如果改成 &s[0..3]，则可以正常通过编译。 因此，当你需要对字符串做切片索引操作时，需要格外小心这一点, 关于该如何操作 UTF-8 字符串，参见这里 
  
  字符串切片的类型标识是 &str，因此我们可以这样声明一个函数，输入 String 类型，返回它的切片: fn first_word(s: &String) -> &str 。
有了切片就可以写出这样的代码：
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);

    s.clear(); // error!

    println!("the first word is: {}", word);
}
fn first_word(s: &String) -> &str {
    &s[..1]
}编译器报错
回忆一下借用的规则：当我们已经有了可变借用时，就无法再拥有不可变的借用。因为 clear 需要清空改变 String，因此它需要一个可变借用（利用 VSCode 可以看到该方法的声明是 pub fn clear(&mut self) ，参数是对自身的可变借用 ）；而之后的 println! 又使用了不可变借用，也就是在 s.clear() 处可变借用与不可变借用试图同时生效，因此编译无法通过。
### 其他切片
因为切片是对集合的部分引用，因此不仅仅字符串有切片，其它集合类型也有，例如数组：
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
该数组切片的类型是 &[i32]，数组切片和字符串切片的工作方式是一样的，例如持有一个引用指向原始数组的某个元素和长度。

### 字符串字面量是切片
之前提到过字符串字面量,但是没有提到它的类型：let s = "Hello, world!";
实际上，s 的类型是 &str，因此你也可以这样声明：let s: &str = "Hello, world!";
该切片指向了程序可执行文件中的某个点，这也是为什么字符串字面量是不可变的，因为 &str 是一个不可变引用。
### 什么是字符串?
字符串是由字符组成的连续集合，但是在上一节中我们提到过，Rust 中的字符是 Unicode 类型，因此每个字符占据 4 个字节内存空间，但是在字符串中不一样，字符串是 UTF-8 编码，也就是字符串中的字符所占的字节数是变化的(1 - 4)，这样有助于大幅降低字符串所占用的内存空间。
Rust 在语言级别，只有一种字符串类型： str，它通常是以引用类型出现 &str，也就是上文提到的字符串切片。虽然语言级别只有上述的 str 类型，但是在标准库里，还有多种不同用途的字符串类型，其中使用最广的即是 String 类型。

str 类型是硬编码进可执行文件，也无法被修改，但是 String 则是一个可增长、可改变且具有所有权的 UTF-8 编码字符串，当 Rust 用户提到字符串时，往往指的就是 String 类型和 &str 字符串切片类型，这两个类型都是 UTF-8 编码。
除了 String 类型的字符串，Rust 的标准库还提供了其他类型的字符串，例如 OsString， OsStr， CsString 和 CsStr 等，注意到这些名字都以 String 或者 Str 结尾了吗？它们分别对应的是具有所有权和被借用的变量。
### String 与 &str 的转换
* 已经见到好几种从 &str 类型生成 String 类型的操作：
String::from("hello,world")
"hello,world".to_string()
* 将 String 类型转为 &str 类型
fn main() {
    let s = String::from("hello,world!");
    say_hello(&s);
    say_hello(&s[..]);
    say_hello(s.as_str());
}
fn say_hello(s: &str) {
    println!("{}",s);
}
hello,world!
hello,world!
hello,world!
实际上这种灵活用法是因为 deref 隐式强制转换，具体我们会在 Deref 特征进行详细讲解。
### 字符串索引
在其它语言中，使用索引的方式访问字符串的某个字符或者子串是很正常的行为，但是在 Rust 中就会报错:
   let s1 = String::from("hello");
   let h = s1[0];
* 深入字符串内部
字符串的底层的数据存储格式实际上是[ u8 ]，一个字节数组。对于 let hello = String::from("Hola"); 这行代码来说，Hola 的长度是 4 个字节，因为 "Hola" 中的每个字母在 UTF-8 编码中仅占用 1 个字节，但是对于下面的代码呢？
let hello = String::from("中国人");
如果问你该字符串多长，你可能会说 3，但是实际上是 9 个字节的长度，因为大部分常用汉字在 UTF-8 中的长度是 3 个字节，因此这种情况下对 hello 进行索引，访问 &hello[0] 没有任何意义，因为你取不到 中 这个字符，而是取到了这个字符三个字节中的第一个字节，这是一个非常奇怪而且难以理解的返回值。

* 字符串的不同表现形式
现在看一下用梵文写的字符串 “नमस्ते”, 它底层的字节数组如下形式：
#![allow(unused)]
fn main() {
[224, 164, 168, 224, 164, 174, 224, 164, 184, 224, 165, 141, 224, 164, 164,
224, 165, 135]
}
长度是 18 个字节，这也是计算机最终存储该字符串的形式。如果从字符的形式去看，则是：['न', 'म', 'स', '्', 'त', 'े']
但是这种形式下，第四和六两个字母根本就不存在，没有任何意义，接着再从字母串的形式去看：["न", "म", "स्", "ते"]
还有一个原因导致了 Rust 不允许去索引字符串：因为索引操作，我们总是期望它的性能表现是 O(1)，然而对于 String 类型来说，无法保证这一点，因为 Rust 可能需要从 0 开始去遍历字符串来定位合法的字符。
### 字符串切片
字符串切片是非常危险的操作，因为切片的索引是通过字节来进行，但是字符串又是 UTF-8 编码，因此你无法保证索引的字节刚好落在字符的边界上，例如：
let hello = "中国人";

let s = &hello[0..2];
运行上面的程序，会直接造成崩溃：这里提示的很清楚，我们索引的字节落在了 中 字符的内部，这种返回没有任何意义。
因此在通过索引区间来访问字符串时，需要格外的小心，一不注意，就会导致你程序的崩溃！
### 操作字符串
由于 String 是可变字符串，下面介绍 Rust 字符串的修改，添加，删除等常用方法：
* 追加 (Push)
在字符串尾部可以使用 push() 方法追加字符 char，也可以使用 push_str() 方法追加字符串字面量。这两个方法都是在原有的字符串上追加，并不会返回新的字符串。由于字符串追加操作要修改原来的字符串，则该字符串必须是可变的，即字符串变量必须由 mut 关键字修饰。
fn main() {
    let mut s = String::from("Hello ");
    s.push('r');
    println!("追加字符 push() -> {}", s);

    s.push_str("ust!");
    println!("追加字符串 push_str() -> {}", s);
}
追加字符 push() -> Hello r
追加字符串 push_str() -> Hello rust!
* 插入 (Insert)
可以使用 insert() 方法插入单个字符 char，也可以使用 insert_str() 方法插入字符串字面量，与 push() 方法不同，这俩方法需要传入两个参数，第一个参数是字符（串）插入位置的索引，第二个参数是要插入的字符（串），索引从 0 开始计数，如果越界则会发生错误。由于字符串插入操作要修改原来的字符串，则该字符串必须是可变的，即字符串变量必须由 mut 关键字修饰。
fn main() {
    let mut s = String::from("Hello rust!");
    s.insert(5, ',');
    println!("插入字符 insert() -> {}", s);
    s.insert_str(6, " I like");
    println!("插入字符串 insert_str() -> {}", s);
}
插入字符 insert() -> Hello, rust!
插入字符串 insert_str() -> Hello, I like rust!
* 替换 (Replace)
如果想要把字符串中的某个字符串替换成其它的字符串，那可以使用 replace() 方法。与替换有关的方法有三个。

1、replace

该方法可适用于 String 和 &str 类型。replace() 方法接收两个参数，第一个参数是要被替换的字符串，第二个参数是新的字符串。该方法会替换所有匹配到的字符串。该方法是返回一个新的字符串，而不是操作原来的字符串。
fn main() {
    let string_replace = String::from("I like rust. Learning rust is my favorite!");
    let new_string_replace = string_replace.replace("rust", "RUST");
    dbg!(new_string_replace);
}
new_string_replace = "I like RUST. Learning RUST is my favorite!"
2、replacen

该方法可适用于 String 和 &str 类型。replacen() 方法接收三个参数，前两个参数与 replace() 方法一样，第三个参数则表示替换的个数。该方法是返回一个新的字符串，而不是操作原来的字符串。
fn main() {
    let string_replace = "I like rust. Learning rust is my favorite!";
    let new_string_replacen = string_replace.replacen("rust", "RUST", 1);
    dbg!(new_string_replacen);
}
new_string_replacen = "I like RUST. Learning rust is my favorite!"

3、replace_range

该方法仅适用于 String 类型。replace_range 接收两个参数，第一个参数是要替换字符串的范围（Range），第二个参数是新的字符串。该方法是直接操作原来的字符串，不会返回新的字符串。该方法需要使用 mut 关键字修饰。
fn main() {
    let mut string_replace_range = String::from("I like rust!");
    string_replace_range.replace_range(7..8, "R");
    dbg!(string_replace_range);
}
string_replace_range = "I like Rust!"
* 删除 (Delete)
与字符串删除相关的方法有 4 个，他们分别是 pop()，remove()，truncate()，clear()。这四个方法仅适用于 String 类型。

1、 pop —— 删除并返回字符串的最后一个字符

该方法是直接操作原来的字符串。但是存在返回值，其返回值是一个 Option 类型，如果字符串为空，则返回 None。 示例代码如下：
fn main() {
    let mut string_pop = String::from("rust pop 中文!");
    let p1 = string_pop.pop();
    let p2 = string_pop.pop();
    dbg!(p1);
    dbg!(p2);
    dbg!(string_pop);
}
p1 = Some(
   '!',
)
p2 = Some(
   '文',
)
string_pop = "rust pop 中"
2、 remove —— 删除并返回字符串中指定位置的字符

该方法是直接操作原来的字符串。但是存在返回值，其返回值是删除位置的字符串，只接收一个参数，表示该字符起始索引位置。remove() 方法是按照字节来处理字符串的，如果参数所给的位置不是合法的字符边界，则会发生错误。
fn main() {
    let mut string_remove = String::from("测试remove方法");
    println!(
        "string_remove 占 {} 个字节",
        std::mem::size_of_val(string_remove.as_str())
    );
    // 删除第一个汉字
    string_remove.remove(0);
    // 下面代码会发生错误
    // string_remove.remove(1);
    // 直接删除第二个汉字
    // string_remove.remove(3);
    dbg!(string_remove);
}
string_remove 占 18 个字节
string_remove = "试remove方法"
3、truncate —— 删除字符串中从指定位置开始到结尾的全部字符

该方法是直接操作原来的字符串。无返回值。该方法 truncate() 方法是按照字节来处理字符串的，如果参数所给的位置不是合法的字符边界，则会发生错误。
fn main() {
    let mut string_truncate = String::from("测试truncate");
    string_truncate.truncate(3);
    dbg!(string_truncate);
}
代码运行结果：string_truncate = "测"
4、clear —— 清空字符串
该方法是直接操作原来的字符串。调用后，删除字符串中的所有字符，相当于 truncate() 方法参数为 0 的时候。
fn main() {
    let mut string_clear = String::from("string clear");
    string_clear.clear();
    dbg!(string_clear);
}
代码运行结果：string_clear = ""
连接 (Catenate)
1、使用 + 或者 += 连接字符串

使用 + 或者 += 连接字符串，要求右边的参数必须为字符串的切片引用（Slice)类型。其实当调用 + 的操作符时，相当于调用了 std::string 标准库中的 add() 方法，这里 add() 方法的第二个参数是一个引用的类型。因此我们在使用 +， 必须传递切片引用类型。不能直接传递 String 类型。+ 和 += 都是返回一个新的字符串。所以变量声明可以不需要 mut 关键字修饰。
fn main() {
    let string_append = String::from("hello ");
    let string_rust = String::from("rust");
    // &string_rust会自动解引用为&str
    let result = string_append + &string_rust;
    let mut result = result + "!";
    result += "!!!";

    println!("连接字符串 + -> {}", result);
}
连接字符串 + -> hello rust!!!!
add() 方法的定义：



fn add(self, s: &str) -> String
因为该方法涉及到更复杂的特征功能，因此我们这里简单说明下：


fn main() {
    let s1 = String::from("hello,");
    let s2 = String::from("world!");
    // 在下句中，s1的所有权被转移走了，因此后面不能再使用s1
    let s3 = s1 + &s2;
    assert_eq!(s3,"hello,world!");
    // 下面的语句如果去掉注释，就会报错
    // println!("{}",s1);
}
self 是 String 类型的字符串 s1，该函数说明，只能将 &str 类型的字符串切片添加到 String 类型的 s1 上，然后返回一个新的 String 类型，所以 let s3 = s1 + &s2; 就很好解释了，将 String 类型的 s1 与 &str 类型的 s2 进行相加，最终得到 String 类型的 s3。
由此可推，以下代码也是合法的：



let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

// String = String + &str + &str + &str + &str
let s = s1 + "-" + &s2 + "-" + &s3;
String + &str返回一个 String，然后再继续跟一个 &str 进行 + 操作，返回一个 String 类型，不断循环，最终生成一个 s，也是 String 类型。
s1 这个变量通过调用 add() 方法后，所有权被转移到 add() 方法里面， add() 方法调用后就被释放了，同时 s1 也被释放了。再使用 s1 就会发生错误。这里涉及到所有权转移（Move）的相关知识。
2、使用 format! 连接字符串

format! 这种方式适用于 String 和 &str 。format! 的用法与 print! 的用法类似，详见格式化输出。

示例代码如下：


fn main() {
    let s1 = "hello";
    let s2 = String::from("rust");
    let s = format!("{} {}!", s1, s2);
    println!("{}", s);
}

代码运行结果：hello rust!
* 字符串转义
我们可以通过转义的方式 \ 输出 ASCII 和 Unicode 字符。
fn main() {
    // 通过 \ + 字符的十六进制表示，转义输出一个字符
    let byte_escape = "I'm writing \x52\x75\x73\x74!";
    println!("What are you doing\x3F (\\x3F means ?) {}", byte_escape);

    // \u 可以输出一个 unicode 字符
    let unicode_codepoint = "\u{211D}";
    let character_name = "\"DOUBLE-STRUCK CAPITAL R\"";

    println!(
        "Unicode character {} (U+211D) is called {}",
        unicode_codepoint, character_name
    );

    // 换行了也会保持之前的字符串格式
    let long_string = "String literals
                        can span multiple lines.
                        The linebreak and indentation here ->\
                        <- can be escaped too!";
    println!("{}", long_string);
}
当然，在某些情况下，可能你会希望保持字符串的原样，不要转义:


fn main() {
    println!("{}", "hello \\x52\\x75\\x73\\x74");
    let raw_str = r"Escapes don't work here: \x3F \u{211D}";
    println!("{}", raw_str);

    // 如果字符串包含双引号，可以在开头和结尾加 #
    let quotes = r#"And then I said: "There is no escape!""#;
    println!("{}", quotes);
    // 如果还是有歧义，可以继续增加，没有限制
    let longer_delimiter = r###"A string with "# in it. And even "##!"###;
    println!("{}", longer_delimiter);
}
* 操作 UTF-8 字符串
前文提到了几种使用 UTF-8 字符串的方式，下面来一一说明。
字符
如果你想要以 Unicode 字符的方式遍历字符串，最好的办法是使用 chars 方法，例如：



for c in "中国人".chars() {
    println!("{}", c);
}
输出如下
中
国
人
* 字节
这种方式是返回字符串的底层字节数组表现形式：
for b in "中国人".bytes() {
    println!("{}", b);
}
228
184
173
229
155
189
228
186
186
获取子串
想要准确的从 UTF-8 字符串中获取子串是较为复杂的事情，例如想要从 holla中国人नमस्ते 这种变长的字符串中取出某一个子串，使用标准库你是做不到的。 你需要在 crates.io 上搜索 utf8 来寻找想要的功能。

可以考虑尝试下这个库:utf8_slice。
字符串深度剖析
那么问题来了，为啥 String 可变，而字符串字面值 str 却不可以？
就字符串字面值来说，我们在编译时就知道其内容，最终字面值文本被直接硬编码进可执行文件中，这使得字符串字面值快速且高效，这主要得益于字符串字面值的不可变性。不幸的是，我们不能为了获得这种性能，而把每一个在编译时大小未知的文本都放进内存中（你也做不到！），因为有的字符串是在程序运行得过程中动态生成的。

对于 String 类型，为了支持一个可变、可增长的文本片段，需要在堆上分配一块在编译时未知大小的内存来存放内容，这些都是在程序运行时完成的：
1.首先向操作系统请求内存来存放 String 对象
2.在使用完成后，将内存释放，归还给操作系统
其中第一部分由 String::from 完成，它创建了一个全新的 String。
重点来了，到了第二部分，就是百家齐放的环节，在有垃圾回收 GC 的语言中，GC 来负责标记并清除这些不再使用的内存对象，这个过程都是自动完成，无需开发者关心，非常简单好用；但是在无 GC 的语言中，需要开发者手动去释放这些内存对象，就像创建对象需要通过编写代码来完成一样，未能正确释放对象造成的后果简直不可估量。

对于 Rust 而言，安全和性能是写到骨子里的核心特性，如果使用 GC，那么会牺牲性能；如果使用手动管理内存，那么会牺牲安全，这该怎么办？为此，Rust 的开发者想出了一个无比惊艳的办法：变量在离开作用域后，就自动释放其占用的内存：

#![allow(unused)]
fn main() {
{
    let s = String::from("hello"); // 从此处起，s 是有效的

    // 使用 s
}                                  // 此作用域已结束，
                                   // s 不再有效，内存被释放
}
与其它系统编程语言的 free 函数相同，Rust 也提供了一个释放内存的函数： drop，但是不同的是，其它语言要手动调用 free 来释放每一个变量占用的内存，而 Rust 则在变量离开作用域时，自动调用 drop 函数: 上面代码中，Rust 在结尾的 } 处自动调用 drop。

### 元组
元组是由多种类型组合到一起形成的，因此它是复合类型，元组的长度是固定的，元组中元素的顺序也是固定的。
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
    //变量 tup 被绑定了一个元组值 (500, 6.4, 1)，该元组的类型是 (i32, f64, u8)，
}
可以使用模式匹配或者 . 操作符来获取元组中的值。
用模式匹配解构元组
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}
上述代码首先创建一个元组，然后将其绑定到 tup 上，接着使用 let (x, y, z) = tup; 来完成一次模式匹配，因为元组是 (n1, n2, n3) 形式的，因此我们用一模一样的 (x, y, z) 形式来进行匹配，元组中对应的值会绑定到变量 x， y， z上。这就是解构：用同样的形式把一个复杂对象中的值匹配出来。
用 . 来访问元组
模式匹配可以让我们一次性把元组中的值全部或者部分获取出来，如果只想要访问某个特定元素，那模式匹配就略显繁琐，对此，Rust 提供了 . 的访问方式：
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}和其它语言的数组、字符串一样，元组的索引从 0 开始。
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}
fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() 返回字符串的长度

    (s, length)
}
The length of 'hello' is 5.
calculate_length 函数接收 s1 字符串的所有权，然后计算字符串的长度，接着把字符串所有权和字符串长度再返回给 s2 和 len 变量。

在其他语言中，可以用结构体来声明一个三维空间中的点，例如 Point(10, 20, 30)，虽然使用 Rust 元组也可以做到：(10, 20, 30)，但是这样写有个非常重大的缺陷：不具备任何清晰的含义，在下一章节中，会提到一种与元组类似的结构体，元组结构体，可以解决这个问题。
### Struct结构体
结构体 struct 恰恰就是这样的复合数据结构，它是由其它数据类型组合而来。 其它语言也有类似的数据结构，不过可能有不同的名称，例如 object、 record 等。
### 语法：
一个结构体由几部分组成：
1.通过关键字 struct 定义
2.一个清晰明确的结构体 名称
3.几个有名字的结构体 字段

#![allow(unused)]
fn main() {
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
}

该结构体名称是 User，拥有 4 个字段，且每个字段都有对应的字段名及类型声明，例如 username 代表了用户名，是一个可变的 String 类型。
## 7月11日
### 创建结构体实例
为了使用上述结构体，我们需要创建 User 结构体的实例：
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };
    
有几点值得注意:
1.初始化实例时，每个字段都需要进行初始化
2.初始化时的字段顺序不需要和结构体定义时的顺序一致
访问结构体
fn main() {
fn build_user(email: String, username: String) -> User {
    User {
        email: email,
        username: username,
        active: true,
        sign_in_count: 1,
    }
}
它接收两个字符串参数： email 和 username，然后使用它们来创建一个 User 结构体，并且返回。可以注意到这两行： email: email 和 username: username，非常的扎眼，因为实在有些啰嗦，如果你从 TypeScript 过来，肯定会鄙视 Rust 一番，不过好在，它也不是无可救药：

fn build_user(email: String, username: String) -> User {
    User {
        email,
        username,
        active: true,
        sign_in_count: 1,
    }
}
如上所示，当函数参数和结构体字段同名时，可以直接使用缩略的方式进行初始化，跟 TypeScript 中一模一样。
### 结构体更新语法
#![allow(unused)]
fn main() {
  let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    };
}
好在 Rust 为我们提供了 结构体更新语法：
  let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
因为 user2 仅仅在 email 上与 user1 不同，因此我们只需要对 email 进行赋值，剩下的通过结构体更新语法 ..user1 即可完成。
结构体更新语法跟赋值语句 = 非常相像，因此在上面代码中，user1 的部分字段所有权被转移到 user2 中：username 字段发生了所有权转移，作为结果，user1 无法再被使用。

聪明的读者肯定要发问了：明明有三个字段进行了自动赋值，为何只有 username 发生了所有权转移？

仔细回想一下所有权那一节的内容，我们提到了 Copy 特征：实现了 Copy 特征的类型无需所有权转移，可以直接在赋值时进行 数据拷贝，其中 bool 和 u64 类型就实现了 Copy 特征，因此 active 和 sign_in_count 字段在赋值给 user2 时，仅仅发生了拷贝，而不是所有权转移。

值得注意的是：username 所有权被转移给了 user2，导致了 user1 无法再被使用，但是并不代表 user1 内部的其它字段不能被继续使用，例如：
#[derive(Debug)]
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
fn main() {
let user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};
let user2 = User {
    active: user1.active,
    username: user1.username,
    email: String::from("another@example.com"),
    sign_in_count: user1.sign_in_count,
};
println!("{}", user1.active);
// 下面这行会报错
println!("{:?}", user1);
}
结构体的内存排列
先来看以下代码：


#[derive(Debug)]
 struct File {
   name: String,
   data: Vec<u8>,
 }

 fn main() {
   let f1 = File {
     name: String::from("f1.txt"),
     data: Vec::new(),
   };

   let f1_name = &f1.name;
   let f1_length = &f1.data.len();

   println!("{:?}", f1);
   println!("{} is {} bytes long", f1_name, f1_length);
 }
 元组结构体(Tuple Struct)
结构体必须要有名称，但是结构体的字段可以没有名称，这种结构体长得很像元组，因此被称为元组结构体，例如：
    struct Color(i32, i32, i32);
    struct Point(i32, i32, i32);

    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
单元结构体(Unit-like Struct)
还记得之前讲过的基本没啥用的单元类型吧？单元结构体就跟它很像，没有任何字段和属性，但是好在，它还挺有用。

如果你定义一个类型，但是不关心该类型的内容, 只关心它的行为时，就可以使用 单元结构体：



struct AlwaysEqual;

let subject = AlwaysEqual;

// 我们不关心 AlwaysEqual 的字段数据，只关心它的行为，因此将它声明为单元结构体，然后再为它实现某个特征
impl SomeTrait for AlwaysEqual {

}
结构体数据的所有权
在之前的 User 结构体的定义中，有一处细节：我们使用了自身拥有所有权的 String 类型而不是基于引用的 &str 字符串切片类型。这是一个有意而为之的选择：因为我们想要这个结构体拥有它所有的数据，而不是从其它地方借用数据。

你也可以让 User 结构体从其它对象借用数据，不过这么做，就需要引入生命周期(lifetimes)这个新概念（也是一个复杂的概念），简而言之，生命周期能确保结构体的作用范围要比它所借用的数据的作用范围要小。

总之，如果你想在结构体中使用一个引用，就必须加上生命周期，否则就会报错：
struct User {
    username: &str,
    email: &str,
    sign_in_count: u64,
    active: bool,
}

fn main() {
    let user1 = User {
        email: "someone@example.com",
        username: "someusername123",
        active: true,
        sign_in_count: 1,
    };
}
使用 #[derive(Debug)] 来打印结构体的信息
在前面的代码中我们使用 #[derive(Debug)] 对结构体进行了标记，这样才能使用 println!("{:?}", s); 的方式对其进行打印输出，如果不加，看看会发生什么:
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 is {}", rect1);
}
fn main() {
    let v = 1;
    let b = true;

    println!("{}, {}", v, b);
}
上面代码不会报错，那么结构体为什么不默认实现 Display 特征呢？原因在于结构体较为复杂，例如考虑以下问题：你想要逗号对字段进行分割吗？需要括号吗？加在什么地方？所有的字段都应该显示？类似的还有很多，由于这种复杂性，Rust 不希望猜测我们想要的是什么，而是把选择权交给我们自己来实现：如果要用 {} 的方式打印结构体，那就自己实现 Display 特征。
让我们实现 Debug 特征，Oh No，就是不想实现 Display 特征，才用的 {:?}，怎么又要实现 Debug，但是仔细看，提示中有一行： add #[derive(Debug)] to Rectangle， 哦？这不就是我们前文一直在使用的吗？
首先，Rust 默认不会为我们实现 Debug，为了实现，有两种方式可以选择：

1.手动实现
2.使用 derive 派生实现
后者简单的多，但是也有限制，具体见附录 D，这里我们就不再深入讲解，来看看该如何使用:
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 is {:?}", rect1);
}
rect1 is Rectangle { width: 30, height: 50 }

当结构体较大时，我们可能希望能够有更好的输出表现，此时可以使用 {:#?} 来替代 {:?}，输出如下:
rect1 is Rectangle {
    width: 30,
    height: 50,
}
下面的例子中清晰的展示了 dbg! 如何在打印出信息的同时，还把表达式的值赋给了 width:


#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let scale = 2;
    let rect1 = Rectangle {
        width: dbg!(30 * scale),
        height: 50,
    };

    dbg!(&rect1);
}
$ cargo run
[src/main.rs:10] 30 * scale = 60
[src/main.rs:14] &rect1 = Rectangle {
    width: 60,
    height: 50,
}
### 枚举
枚举(enum 或 enumeration)允许你通过列举可能的成员来定义一个枚举类型，例如扑克牌花色：
enum PokerSuit {
  Clubs,
  Spades,
  Diamonds,
  Hearts,
}
枚举值
现在来创建 PokerSuit 枚举类型的两个成员实例：
let heart = PokerSuit::Hearts;
let diamond = PokerSuit::Diamonds;
我们通过 :: 操作符来访问 PokerSuit 下的具体成员，从代码可以清晰看出，heart 和 diamond 都是 PokerSuit 枚举类型的，接着可以定义一个函数来使用它们：
```rust
fn main() {
    let heart = PokerSuit::Hearts;
    let diamond = PokerSuit::Diamonds;

    print_suit(heart);
    print_suit(diamond);
}
print_suit 函数的参数类型是 PokerSuit，因此我们可以把 heart 和 diamond 传给它，虽然 heart 是基于 PokerSuit 下的 Hearts 成员实例化的，但是它是货真价实的 PokerSuit 枚举类型。

enum PokerSuit {
    Clubs,
    Spades,
    Diamonds,
    Hearts,
}

struct PokerCard {
    suit: PokerSuit,
    value: u8
}

fn main() {
   let c1 = PokerCard {
       suit: PokerSuit::Clubs,
       value: 1,
   };
   let c2 = PokerCard {
       suit: PokerSuit::Diamonds,
       value: 12,
   };
}
这段代码很好的完成了它的使命，通过结构体 PokerCard 来代表一张牌，结构体的 suit 字段表示牌的花色，类型是 PokerSuit 枚举类型，value 字段代表扑克牌的数值。

可以吗？可以！好吗？说实话，不咋地，因为还有简洁得多的方式来实现：


enum PokerCard {
    Clubs(u8),
    Spades(u8),
    Diamonds(u8),
    Hearts(u8),
}

fn main() {
   let c1 = PokerCard::Spades(5);
   let c2 = PokerCard::Diamonds(13);
}
不仅如此，同一个枚举类型下的不同成员还能持有不同的数据类型，例如让某些花色打印 1-13 的字样，另外的花色打印上 A-K 的字样：
enum PokerCard {
    Clubs(u8),
    Spades(u8),
    Diamonds(char),
    Hearts(char),
}
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}


fn main() {
    let m1 = Message::Quit;
    let m2 = Message::Move{x:1,y:1};
    let m3 = Message::ChangeColor(255,255,0);
}
该枚举类型代表一条消息，它包含四个不同的成员：
Quit 没有任何关联数据
Move 包含一个匿名结构体
Write 包含一个 String 字符串
ChangeColor 包含三个 i32

fn main() {
   let c1 = PokerCard::Spades(5);
   let c2 = PokerCard::Diamonds('A');
}
fn print_suit(card: PokerSuit) {
    println!("{:?}",card);
}
当然，我们也可以用结构体的方式来定义这些消息：
struct QuitMessage; // 单元结构体
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // 元组结构体
struct ChangeColorMessage(i32, i32, i32); // 元组结构体
同一化类型
最后，再用一个实际项目中的简化片段，来结束枚举类型的语法学习。

例如我们有一个 WEB 服务，需要接受用户的长连接，假设连接有两种：TcpStream 和 TlsStream，但是我们希望对这两个连接的处理流程相同，也就是用同一个函数来处理这两个连接，代码如下：
fn new (stream: TcpStream) {
  let mut s = stream;
  if tls {
    s = negotiate_tls(stream)
  }

  // websocket是一个WebSocket<TcpStream>或者
  //   WebSocket<native_tls::TlsStream<TcpStream>>类型
  websocket = WebSocket::from_raw_socket(
    stream, ......)
}
此时，枚举类型就能帮上大忙：



enum Websocket {
  Tcp(Websocket<TcpStream>),
  Tls(Websocket<native_tls::TlsStream<TcpStream>>),
}
Option 枚举用于处理空值
在其它编程语言中，往往都有一个 null 关键字，该关键字用于表明一个变量当前的值为空（不是零值，例如整形的零值是 0），也就是不存在值。当你对这些 null 进行操作时，例如调用一个方法，就会直接抛出null 异常，导致程序的崩溃，因此我们在编程时需要格外的小心去处理这些 null 空值。
Rust 吸取了众多教训，决定抛弃 null，而改为使用 Option 枚举变量来表述这种结果。

Option 枚举包含两个成员，一个成员表示含有值：Some(T), 另一个表示没有值：None，定义如下：
enum Option<T> {
    Some(T),
    None,
}

let some_number = Some(5);
let some_string = Some("a string");

let absent_number: Option<i32> = None;
如果使用 None 而不是 Some，需要告诉 Rust Option<T> 是什么类型的，因为编译器只通过 None 值无法推断出 Some 成员保存的值的类型。
当有一个 Some 值时，我们就知道存在一个值，而这个值保存在 Some 中。当有个 None 值时，在某种意义上，它跟空值具有相同的意义：并没有一个有效的值。那么，Option<T> 为什么就比空值要好呢？

let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y;

fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
let five = Some(5);
let six = plus_one(five);
let none = plus_one(None);
### 数组
在日常开发中，使用最广的数据结构之一就是数组，在 Rust 中，最常用的数组有两种，第一种是速度很快但是长度固定的 array，第二种是可动态增长的但是有性能损耗的 Vector，在本书中，我们称 array 为数组，Vector 为动态数组。
这两个数组的关系跟 &str 与 String 的关系很像，前者是长度固定的字符串切片，后者是可动态增长的字符串。其实，在 Rust 中无论是 String 还是 Vector，它们都是 Rust 的高级类型：集合类型，

数组的三要素：

1.长度固定
2.元素必须有相同的类型
3.依次线性排列
创建数组
在 Rust 中，数组是这样定义的：
fn main() {
    let a = [1, 2, 3, 4, 5];
}
因此数组 array 是存储在栈上，性能也会非常优秀。与此对应，动态数组 Vector 是存储在堆上，因此长度可以动态改变。当你不确定是使用数组还是动态数组时，那就应该使用后者，

#![allow(unused)]
fn main() {
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
}
在一些时候，还需要为数组声明类型，如下所示：let a: [i32; 5] = [1, 2, 3, 4, 5];
访问数组元素
因为数组是连续存放元素的，因此可以通过索引的方式来访问存放其中的元素：
fn main() {
    let a = [9, 8, 7, 6, 5];

    let first = a[0]; // 获取a数组第一个元素
    let second = a[1]; // 获取第二个元素
}
越界访问
如果使用超出数组范围的索引访问数组元素，会怎么样？下面是一个接收用户的控制台输入，然后将其作为索引访问数组元素的例子：
use std::io;

fn main() {
    let a = [1, 2, 3, 4, 5];

    println!("Please enter an array index.");

    let mut index = String::new();
    // 读取控制台的输出
    io::stdin()
        .read_line(&mut index)
        .expect("Failed to read line");

    let index: usize = index
        .trim()
        .parse()
        .expect("Index entered was not a number");

    let element = a[index];

    println!(
        "The value of the element at index {} is: {}",
        index, element
    );
}
数组切片
在之前的章节，我们有讲到 切片 这个概念，它允许你引用集合中的部分连续片段，而不是整个集合，对于数组也是，数组切片允许我们引用数组的一部分：



let a: [i32; 5] = [1, 2, 3, 4, 5];
let slice: &[i32] = &a[1..3];
assert_eq!(slice, &[2, 3]);
上面的数组切片 slice 的类型是&[i32]，与之对比，数组的类型是[i32;5]，简单总结下切片的特点：
1.切片的长度可以与数组不同，并不是固定的，而是取决于你使用时指定的起始和结束位置
2.创建切片的代价非常小，因为切片只是针对底层数组的一个引用
3.切片类型[T]拥有不固定的大小，而切片引用类型&[T]则具有固定的大小，因为 Rust 很多时候都需要固定大小数据类型，因此&[T]更有用,&str字符串切片也同理
fn main() {
  // 编译器自动推导出one的类型
  let one             = [1, 2, 3];
  // 显式类型标注
  let two: [u8; 3]    = [1, 2, 3];
  let blank1          = [0; 3];
  let blank2: [u8; 3] = [0; 3];

  // arrays是一个二维数组，其中每一个元素都是一个数组，元素类型是[u8; 3]
  let arrays: [[u8; 3]; 4]  = [one, two, blank1, blank2];

  // 借用arrays的元素用作循环中
  for a in &arrays {
    print!("{:?}: ", a);
    // 将a变成一个迭代器，用于循环
    // 你也可以直接用for n in a {}来进行循环
    for n in a.iter() {
      print!("\t{} + 10 = {}", n, n+10);
    }

    let mut sum = 0;
    // 0..a.len,是一个 Rust 的语法糖，其实就等于一个数组，元素是从0,1,2一直增加到到a.len-1
    for i in 0..a.len() {
      sum += a[i];
    }
    println!("\t({:?} = {})", a, sum);
  }
}
数组类型容易跟数组切片混淆，[T;n]描述了一个数组的类型，而[T]描述了切片的类型， 因为切片是运行期的数据结构，它的长度无法在编译期得知，因此不能用[T;n]的形式去描述
[u8; 3]和[u8; 4]是不同的类型，数组的长度也是类型的一部分
在实际开发中，使用最多的是数组切片[T]，我们往往通过引用的方式去使用&[T]，因为后者有固定的类型大小
### 7月12日
### 模式匹配
在 Rust 中，模式匹配最常用的就是 match 和 if let，
先来看一个关于 match 的简单例子：
enum Direction {
    East,
    West,
    North,
    South,
}

fn main() {
    let dire = Direction::South;
    match dire {
        Direction::East => println!("East"),
        Direction::North | Direction::South => {
            println!("South or North");
        },
        _ => println!("West"),
    };
}
这里我们想去匹配 dire 对应的枚举类型，因此在 match 中用三个匹配分支来完全覆盖枚举变量 Direction 的所有成员类型，有以下几点值得注意：
match 的匹配必须要穷举出所有可能，因此这里用 _ 来代表未列出的所有可能性
match 的每一个分支都必须是一个表达式，且所有分支的表达式最终返回值的类型必须相同
X | Y，类似逻辑运算符 或，代表该分支可以匹配 X 也可以匹配 Y，只要满足一个即可
其实 match 跟其他语言中的 switch 非常像，_ 类似于 switch 中的 default。
match 匹配
首先来看看 match 的通用形式：

match target {
    模式1 => 表达式1,
    模式2 => {
        语句1;
        语句2;
        表达式2
    },
    _ => 表达式3
}
该形式清晰的说明了何为模式，何为模式匹配：将模式与 target 进行匹配，即为模式匹配，而模式匹配不仅仅局限于 match，后面我们会详细阐述。
match 允许我们将一个值与一系列的模式相比较，并根据相匹配的模式执行对应的代码，下面让我们来一一详解，先看一个例子：
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny =>  {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
match 后紧跟着的是一个表达式，跟 if 很像，但是 if 后的表达式必须是一个布尔值，而 match 后的表达式返回值可以是任意类型，只要能跟后面的分支中的模式匹配起来即可，这里的 coin 是枚举 Coin 类型。
接下来是 match 的分支。一个分支有两个部分：一个模式和针对该模式的处理代码。第一个分支的模式是 Coin::Penny，其后的 => 运算符将模式和将要运行的代码分开。这里的代码就仅仅是表达式 1，不同分支之间使用逗号分隔。
当 match 表达式执行时，它将目标值 coin 按顺序依次与每一个分支的模式相比较，如果模式匹配了这个值，那么模式之后的代码将被执行。如果模式并不匹配这个值，将继续执行下一个分支。
每个分支相关联的代码是一个表达式，而表达式的结果值将作为整个 match 表达式的返回值。如果分支有多行代码，那么需要用 {} 包裹，同时最后一行代码需要是一个表达式。
使用 match 表达式赋值
还有一点很重要，match 本身也是一个表达式，因此可以用它来赋值：


enum IpAddr {
   Ipv4,
   Ipv6
}

fn main() {
    let ip1 = IpAddr::Ipv6;
    let ip_str = match ip1 {
        IpAddr::Ipv4 => "127.0.0.1",
        _ => "::1",
    };

    println!("{}", ip_str);
}
模式绑定
模式匹配的另外一个重要功能是从模式中取出绑定的值，例如：



#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState), // 25美分硬币
}
其中 Coin::Quarter 成员还存放了一个值：美国的某个州（因为在 1999 年到 2008 年间，美国在 25 美分(Quarter)硬币的背后为 50 个州印刷了不同的标记，其它硬币都没有这样的设计）。
例如有一个印了阿拉斯加州标记的 25 分硬币：Coin::Quarter(UsState::Alaska), 它在匹配时，state 变量将被绑定 UsState::Alaska 的枚举值。
接下来，我们希望在模式匹配中，获取到 25 美分硬币上刻印的州的名称：
例如有一个印了阿拉斯加州标记的 25 分硬币：Coin::Quarter(UsState::Alaska), 它在匹配时，state 变量将被绑定 UsState::Alaska 的枚举值。
enum Action {
    Say(String),
    MoveTo(i32, i32),
    ChangeColorRGB(u16, u16, u16),
}

fn main() {
    let actions = [
        Action::Say("Hello Rust".to_string()),
        Action::MoveTo(1,2),
        Action::ChangeColorRGB(255,255,0),
    ];
    for action in actions {
        match action {
            Action::Say(s) => {
                println!("{}", s);
            },
            Action::MoveTo(x, y) => {
                println!("point from (0, 0) move to ({}, {})", x, y);
            },
            Action::ChangeColorRGB(r, g, _) => {
                println!("change color into '(r:{}, g:{}, b:0)', 'b' has been ignored",
                    r, g,
                );
            }
        }
    }
Hello Rust
point from (0, 0) move to (1, 2)
change color into '(r:255, g:255, b:0)', 'b' has been ignored
}
穷尽匹配
在文章的开头，我们简单总结过 match 的匹配必须穷尽所有情况，下面来举例说明，
enum Direction {
    East,
    West,
    North,
    South,
}

fn main() {
    let dire = Direction::South;
    match dire {
        Direction::East => println!("East"),
        Direction::North | Direction::South => {
            println!("South or North");
        },
    };
}
我们没有处理 Direction::West 的情况，因此会报错：
_ 通配符
当我们不想在匹配的时候列出所有值的时候，可以使用 Rust 提供的一个特殊模式，例如，u8 可以拥有 0 到 255 的有效的值，但是我们只关心 1、3、5 和 7 这几个值，不想列出其它的 0、2、4、6、8、9 一直到 255 的值。那么, 我们不必一个一个列出所有值, 因为可以使用特殊的模式 _ 替代：

#![allow(unused)]
fn main() {
let some_u8_value = 0u8;
match some_u8_value {
    1 => println!("one"),
    3 => println!("three"),
    5 => println!("five"),
    7 => println!("seven"),
    _ => (),
}
}
if let 匹配
有时会遇到只有一个模式的值需要被处理，其它值直接忽略的场景，如果用 match 来处理就要写成下面这样：

#![allow(unused)]
fn main() {
    let v = Some(3u8);
    match v {
        Some(3) => println!("three"),
        _ => (),
    }
}
three
我们只想要对 Some(3) 模式进行匹配, 不想处理任何其他 Some<u8> 值或 None 值。但是为了满足 match 表达式（穷尽性）的要求，写代码时必须在处理完这唯一的成员后加上 _ => ()，这样会增加不少无用的代码。

杀鸡焉用牛刀，可以用 if let 的方式来实现：
if let Some(3) = v {
    println!("three");
}
这两种匹配对于新手来说，可能有些难以抉择，但是只要记住一点就好：当你只要匹配一个条件，且忽略其他条件时就用 if let ，否则都用 match。
matches!宏
Rust 标准库中提供了一个非常实用的宏：matches!，它可以将一个表达式跟模式进行匹配，然后返回匹配的结果 true or false。

例如，有一个动态数组，里面存有以下枚举：
enum MyEnum {
    Foo,
    Bar
}

fn main() {
    let v = vec![MyEnum::Foo,MyEnum::Bar,MyEnum::Foo];
}
现在如果想对 v 进行过滤，只保留类型是 MyEnum::Foo 的元素，你可能想这么写：v.iter().filter(|x| x == MyEnum::Foo);
但是，实际上这行代码会报错，因为你无法将 x 直接跟一个枚举成员进行比较。好在，你可以使用 match 来完成，但是会导致代码更为啰嗦，是否有更简洁的方式？答案是使用 matches!：
v.iter().filter(|x| matches!(x, MyEnum::Foo));

let foo = 'f';
assert!(matches!(foo, 'A'..='Z' | 'a'..='z'));
let bar = Some(4);
assert!(matches!(bar, Some(x) if x > 2));
变量覆盖
无论是 match 还是 if let，他们都可以在模式匹配时覆盖掉老的值，绑定新的值:


fn main() {
   let age = Some(30);
   println!("在匹配前，age是{:?}",age);
   if let Some(age) = age {
       println!("匹配出来的age是{}",age);
   }
   println!("在匹配后，age是{:?}",age);
}
在匹配前，age是Some(30)
匹配出来的age是30
在匹配后，age是Some(30)
可以看出在 if let 中，= 右边 Some(i32) 类型的 age 被左边 i32 类型的新 age 覆盖了，该覆盖一直持续到 if let 语句块的结束。因此第三个 println! 输出的 age 依然是 Some(i32) 类型。
对于 match 类型也是如此:


fn main() {
   let age = Some(30);
   println!("在匹配前，age是{:?}",age);
   match age {
       Some(age) =>  println!("匹配出来的age是{}",age),
       _ => ()
   }
   println!("在匹配后，age是{:?}",age);
}
解构 Option
在枚举那章，提到过 Option 枚举，它用来解决 Rust 中变量是否有值的问题，定义如下：



enum Option<T> {
    Some(T),
    None,
}
简单解释就是：一个变量要么有值：Some(T), 要么为空：None。
因为 Option，Some，None 都包含在 prelude 中，因此你可以直接通过名称来使用它们，而无需以 Option::Some 这种形式去使用，总之，千万不要因为调用路径变短了，就忘记 Some 和 None 也是 Option 底下的枚举成员！
匹配 Option<T>
使用 Option<T>，是为了从 Some 中取出其内部的 T 值以及处理没有值的情况，为了演示这一点，下面一起来编写一个函数，它获取一个 Option<i32>，如果其中含有一个值，将其加一；如果其中没有值，则函数返回 None 值：



fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
let five = Some(5);
let six = plus_one(five);
let none = plus_one(None);
plus_one 接受一个 Option<i32> 类型的参数，同时返回一个 Option<i32> 类型的值(这种形式的函数在标准库内随处所见)，在该函数的内部处理中，如果传入的是一个 None ，则返回一个 None 且不做任何处理；如果传入的是一个 Some(i32)，则通过模式绑定，把其中的值绑定到变量 i 上，然后返回 i+1 的值，同时用 Some 进行包裹。

为了进一步说明，假设 plus_one 函数接受的参数值 x 是 Some(5)，来看看具体的分支匹配情况：
传入参数 Some(5)

None => None,
首先是匹配 None 分支，因为值 Some(5) 并不匹配模式 None，所以继续匹配下一个分支。
Some(i) => Some(i + 1),
Some(5) 与 Some(i) 匹配吗？当然匹配！它们是相同的成员。i 绑定了 Some 中包含的值，因此 i 的值是 5。接着匹配分支的代码被执行，最后将 i 的值加一并返回一个含有值 6 的新 Some。
传入参数 None
接着考虑下 plus_one 的第二个调用，这次传入的 x 是 None， 我们进入 match 并与第一个分支相比较。
None => None,
匹配上了！接着程序继续执行该分支后的代码：返回表达式 None 的值，也就是返回一个 None，因为第一个分支就匹配到了，其他的分支将不再比较。
### 7月13日
范性学习
