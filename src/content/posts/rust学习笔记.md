---
title: rust学习笔记
published: 2025-07-03
description: 'rust学习笔记'
image: ''
tags: [study]
category: 'study'
draft: false 
lang: ''
---
### 说在前面
&ensp;&ensp;做了一个长期学习Rust的计划，每天的学习都以博客的形式记录，同时也作为给自己查询的笔记。
任何错误欢迎斧正,爱来自a@likaix.in  
&ensp;&ensp;***7月3日内容修正***
## Rust 环境搭建（Linux）
### 开发环境
&ensp;&ensp;毋庸置疑，我选择`Visual Studio Code`作为我的开发环境（`code`）。如果是`Ubuntu`有图形化页面，进入软件商店搜code下载即可。如果没有就去[`Visual Studio Code`官网](https://code.visualstudio.com/)下载对应的deb包安装即可。

### 安装Rust编译工具
&ensp;&ensp;Rust的编译工具依赖于C的编译工具，所以在linux下你首先可以安装GCC,在`Ubuntu`中使用`sudo apt install gcc`安装。安装后输入`g++ --version`出现相应版本号即表示安装成功。做完这些前置步骤，终端运行`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`自动安装。下载完毕后会出现安装脚本页面，选择默认回车即可安装。**注意**在安装后一定要重启终端，否则通过`rustc -V`也不会正确输出版本号。只有重启终端后，输入`rustc -V`出现版本号即表示安装成功。

## Rust 基础学习
### 输出命令行与创建项目
&ensp;&ensp;`cargo new project-name`即可快速创建一个`Rust`项目，在`src`文件夹下找到对应的`main.rs`文件即可开始进行`hello world`。终端`cd`进入`src`文件夹，输入`rustc main.rs`会在`src`文件夹下生成`main可执行文件`,`./main`就会发现终端顺利的输输出了`hello world`。

```rust
fn main() { 
    println!("hello world"); 
}
```
## Rust 的基础语法
&ensp;&ensp;Rust作为一直强类型语言，但是又同时具有上下文自动去推测变量类型的功能。默认情况下Rust声明的变量类型是不可变变量。除非使用`mut`关键字声明为可变变量。
```rust
let a = 111; //不可变变量
let mut b = 111; //可变变量
```
&ensp;&ensp;听起来有点反人类，不可变变量，既然是变量为何又说其不可变？那不是和常量没有区别吗？所谓常量简单来说就是程序运行期间都不会改变的变量。听起来似乎和不可变变量一致。其实不然，看下面的代码。
```rust
let a = 111;
let a = 222;
```
&ensp;&ensp;编译器可能会有警告，但是没有语法错误，但是使用以下代码是不合法的。
```rust
const a: i32 = 111; //必须显示声明类型
let a = 222;
```
&ensp;&ensp;二者的作用域也不同，`let`声明的不可变变量在块级作用域内(例如函数内)，`const`声明的常量在全局/模块级作用域内。
&ensp;&ensp;为什么要设计这样的默认不可变变量呢:
 - 安全性（Memory Safety）避免意外的修改
 - 并发友好（Concurrency）多个线程也可以安全的共享，无需锁机制
 - 编译器优化 静态分析 不可变性让编译器更容易优化代码

### Rust数据类型
#### 整数(Integer)
|位长度 |有符号|无符号|
|:---: |:---:|:---:|
|8-bit |i8	 |u8   |
|16-bit|i16  |u16  |
|32-bit|i32  |u32  |
|64-bit|i64  |u64  |
|128-bit|i128|u128 |
|arch  |isize|usize|

#### 浮点数型(Floating-Point)
&ensp;&ensp;`Rust`与其它语言一样支持32位浮点数(f32)和64位浮点数(f64)。默认情况下，64.0 将表示 64 位浮点数，因为现代计算机处理器对两种浮点数计算的速度几乎相同，但 64 位浮点数精度更高。

```rust
fn main() {
    let x = 2.0; // f64
    let y: f32 = 3.0; // f32
}
```

### 布尔型(bool)

&ensp;&ensp;值**只能**为 `true` 或 `false`。这个**只能**就表示{-1,0,1}并不会被隐式的转化为布尔值。所以类似于C语言中的`while(1)`之类的是不合法的。

### 字符型(char)
&ensp;&ensp;Rust的`char`类型大小为4个字节，代表`Unicode标`量值，这意味着它可以支持中文，日文和韩文字符等非英文字符甚至表情符号和零宽度空格在`Rust`中都是有效的`char`值。

&ensp;&ensp;注意：由于中文文字编码有两种（`GBK 和 UTF-8`），所以编程中使用中文字符串有可能导致乱码的出现，这是因为源程序与命令行的文字编码不一致，所以在 `Rust` 中字符串和字符都必须使用 `UTF-8` 编码，否则编译器会报错。

### 复合类型
&ensp;&ensp;元组是用一对 ( ) 包括的一组数据，可以包含不同种类的数据：

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
// tup.0 等于 500
// tup.1 等于 6.4
// tup.2 等于 1
let (x, y, z) = tup;
// y 等于 6.4
```

&ensp;&ensp;数组是用一对 [ ] 包括同类型数据：

```rust
let a = [1,2,3,4,5]

```

### Rust 数学运算
&ensp;&ensp;简单的加减乘除和其他语言基本上一致，但是`Rust`没有自加(++)和自减(--)运算。也就是说i++可能要写为`i=i+1`。

## Rust注释
&ensp;&ensp;和C语言的单多行数据基本上一致，rust把双`//`后的第一个`/`也会算作注释，所以`///`也是合法的。
```rust
// 这是第一种注释方式

/* 这是第二种注释方式 */ 

/* 
 * 多行注释
 * 多行注释
 * 多行注释
 */

 /// 也是可以的
```

## Rust 函数
&ensp;&ensp;不是`function`也不用说明返回值,Rust函数的基本形式：
```rust
fn <函数名> ( <参数> ) <函数体>
```
&ensp;&ensp;`Rust` 函数名称的命名风格是小写字母以下划线分割：
```rust
fn main(){
    hello_world();
}
fn hello_world()
{
    println!("Hell0 world!");
}
```
&ensp;&ensp;`Rust`不在乎在何处定义函数，只需在某个地方定义它们即可。

### 函数参数

&ensp;&ensp;Rust中定义函数如果需要具备参数必须声明参数名称和类型：
```rust
fn main() {
    another_function(5, 6);
}

fn another_function(x: i32, y: i32) {
    println!("x 的值为 : {}", x);
    println!("y 的值为 : {}", y);
}
```
运行结果:

    x 的值为 : 5
    y 的值为 : 6

### 函数体的语句和表达式
`Rust`函数体由一系列可以以表达式（Expression）结尾的语句（Statement）组成。到目前为止，我们仅见到了没有以表达式结尾的函数，但已经将表达式用作语句的一部分。

&ensp;&ensp;语句是执行某些操作且没有返回值的步骤。

```rust
let a = 6;
```

```rust
let a = (let b = 2); //不正确的语句
```


在Rust中
- 表达式（Expression）：会计算并返回一个值(如 1 + 2、foo())。
- 语句（Statement）：执行操作但不返回值(如 let b = 2;、return x;)。
  
&ensp;&ensp;let b = 2 是一个语句，它没有返回值（不像 b = 2 在 C 中会返回赋值后的值）。因此，它不能嵌套在另一个 let 赋值中。

&ensp;&ensp;但是以下是合理：
```rust
fn main() {
    let x = 5;

    let y = {
        let x = 3;
        x + 1
    };

    println!("x 的值为 : {}", x);
    println!("y 的值为 : {}", y);
}
```
&ensp;&ensp;可以在{}中编写一个较为复杂的表达式，其中的`let x = 3`是语句，不返回任何值。但是`x + 1`是表达式，返回了`x + 1`的值，所以是合法的。`x + 1`后面不能加分号，不然就会变成一条语句。

### 函数返回值

&ensp;&ensp;在参数声明之后用 -> 来声明函数返回值的类型（不是 : ）。在函数体中，随时都可以以 return 关键字结束函数运行并返回一个类型合适的值。
```rust
fn add(a: i32, b: i32) -> i32 {
    return a + b;
}
```
&ensp;&ensp;如果说没有声明返回值的类型，就不能使用`return`，同时函数表达式不能使用`return`。举个例子：
```rust
fn main() {
    let result = {
        let x = 3;
        let y = 5;
        return x + y; // 错误！`return` 会直接退出 `main()`
    };
    
    println!("Result: {}", result); // 这行永远不会执行
}
```
```rust
fn main() {
    let value = if true {
        return 42; // 错误！`return` 会退出 `main()`
    } else {
        0
    };
    
    println!("Value: {}", value); // 这行不会执行
}

```

## Rust 条件语句

&ensp;&ensp;Rust中的条件语句如下所示：
```rust
fn main() {
    let a = 1;
    if a < 5 {
        println!(1);
    }
    else
    {
        println!(2);
    }
}

```
&ensp;&ensp;和C语言相比有几点区别：
 - 不需要用()去包裹条件语句`a<5`，但是也可以用()去包裹。
 - 不存在单语句可以不用{}包裹的语法。
 - 表达式必须是bool类型。
  
&ensp;&ensp;Rust中没有三元表达式，但是可以通过以下语句达成同样的效果：
```rust
if <condition> { block 1 } else { block 2 } 
```

## Rust 循环

### while 循环

```rust
fn main() {
    let mut number = 1; 
    while number != 4 { 
        println!("{}", number); 
        number += 1; 
    } 
    println!("EXIT"); 
}
```
&ensp;&ensp;没有`do-while`的语法,但是`do`依旧作为保留字存在。

&ensp;&ensp;`Rust`中不存在类似C语言中的，`for(int i = 1;i<n;i++)`使用三元语句控制循环。需要使用`while`循环

```rust
let mut i = 0; 
while i < 10 { 
    // 循环体 
    i += 1; 
}
```

### for 循环
&ensp;&ensp;这里的.iter是迭代器，后文会详细说明
```rust
fn main() { 
    let a = [10, 20, 30, 40, 50]; 
    for i in a.iter() { 
        println!("值为 : {}", i); 
    } 
}
```
&ensp;&ensp;也可以通过下标来访问数组
```rust
fn main() { 
let a = [10, 20, 30, 40, 50]; 
    for i in 0..5 { 
        println!("a[{}] = {}", i, a[i]); 
    } 
}
```

### loop 循环

&ensp;&ensp;`Rust`语言具有原生的无限循环结构loop：
```rust
fn main() { 
    let s = ['1', '2', '3', '4', '5', '6']; 
    let mut i = 0; 
    loop { 
        let ch = s[i]; 
        if ch == 'O' { 
            break; 
        } 
        println!("\'{}\'", ch);
        i += 1; 
    } 
}
```
&ensp;&ensp;`Rust`支持使用`break i;`的样式，也就是说可以在结束循环的时候返回一个值，如下所示：
```rust
fn main() { 
    let s = ['R', 'U', 'N', 'O', 'O', 'B']; 
    let mut i = 0; 
    let location = loop { 
        let ch = s[i];
        if ch == 'O' { 
            break i; 
        } 
        i += 1; 
    }; 
    println!(" \'O\' 的索引为 {}", location); 
}
```
&ensp;&ensp;当`loop`循环结束的时候返回了`i`的值，此时`location`就获得了`i`的值.

## Rust 迭代器

&ensp;&ensp;Rust 中的迭代器（Iterator）是一个强大且灵活的工具，用于对集合(如数组、向量、链表等)进行逐步访问和操作。
迭代器通过实现`Iterator trait`来定义。最基本的`trait`方法是`next`，用于逐一返回迭代器中的下一个元素。

&ensp;&ensp;迭代器有这么几种原则：

#### 惰性求值（Lazy Evaluation）
&ensp;&ensp;迭代器不应预先计算所有值，而应在调用`next()`时按需生成。
```rust
let v = vec![1, 2, 3];
let iter = v.iter().map(|x| {
    println!("Processing {}", x);
    x * 2
}); // 此时不会执行 map 中的逻辑

for x in iter { // 调用 next() 时才会实际计算
    println!("Got {}", x);
}

```
#### 线性消费（Linear Consumption）
&ensp;&ensp;迭代器通常是一次性的，调用`next()`会消耗当前元素，无法回退或重置。某些迭代器(如 `std::iter::Rev`)可能支持双向遍历，但需明确实现 DoubleEndedIterator。

#### 所有权清晰（Ownership Semantics）(后文会详细解释所有权)
三种变体：
 - 不可变借用迭代器：`iter() → Iterator<Item = &T>`
 - 可变借用迭代器：`iter_mut() → Iterator<Item = &mut T>`
 - 所有权迭代器：`into_iter() → Iterator<Item = T>`（消费集合）
  
#### 无副作用（Side-Effect Free next()）
&ensp;&ensp;next() 的逻辑应尽量纯粹，避免修改外部状态（除非是迭代器自身的必要状态）。
反例：
```rust
// 不良设计：next() 修改外部变量
let mut count = 0;
let bad_iter = std::iter::from_fn(|| {
    count += 1; // 副作用！
    Some(count)
});

```

#### 终止性（Termination）
&ensp;&ensp;迭代器必须能在有限次 next() 后返回 None（除非明确设计为无限迭代器）。
&ensp;&ensp;无限迭代器需明确文档说明（如 std::iter::repeat）。
示例：
```rust
// 有限迭代器
let finite = (0..3).into_iter(); // 最终返回 None

// 无限迭代器需配合 take 等适配器使用
let infinite = std::iter::repeat(1).take(5);

```
#### 方法链式调用（Method Chaining）
&ensp;&ensp;迭代器适配器（如 map/filter）应返回新的迭代器，而非修改原有迭代器。
示例：
```rust
let result = (1..5)
    .map(|x| x * 2)    // 返回新迭代器
    .filter(|x| x > 3) // 再返回新迭代器
    .collect::<Vec<_>>();

```

#### 性能保证（Performance Guarantees）
&ensp;&ensp;零成本抽象：迭代器应编译为与手写循环相近的高效代码。短路操作：如 find/any 应在满足条件时立即停止迭代。

#### 遵循 Iterator Trait 约定
必须实现：
- `type Item`
- `fn next(&mut self) ->Option<Self::Item>`

一个良好设计的迭代器应该是这样的：
```rust
// 良好设计的迭代器
struct Countdown(u32);

impl Iterator for Countdown {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
        if self.0 == 0 {
            None
        } else {
            let val = self.0;
            self.0 -= 1;
            Some(val)
        }
    }

    // 优化：覆盖默认的 size_hint
    fn size_hint(&self) -> (usize, Option<usize>) {
        (self.0 as usize, Some(self.0 as usize))
    }
}

fn main() {
    let countdown = Countdown(3);
    assert_eq!(countdown.collect::<Vec<_>>(), vec![3, 2, 1]);
}

```