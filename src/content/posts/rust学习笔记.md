---
title: rust学习笔记
published: 2025-07-02
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
## Rust环境搭建（Linux）
### 开发环境
&ensp;&ensp;毋庸置疑，我选择`Visual Studio Code`作为我的开发环境（`code`）。如果是`Ubuntu`有图形化页面，进入软件商店搜code下载即可。如果没有就去[`Visual Studio Code`官网](https://code.visualstudio.com/)下载对应的deb包安装即可。

### 安装Rust编译工具
&ensp;&ensp;Rust的编译工具依赖于C的编译工具，所以在linux下你首先可以安装GCC,在`Ubuntu`中使用`sudo apt install gcc`安装。安装后输入`g++ --version`出现相应版本号即表示安装成功。做完这些前置步骤，终端运行`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`自动安装。下载完毕后会出现安装脚本页面，选择默认回车即可安装。**注意**在安装后一定要重启终端，否则通过`rustc -V`也不会正确输出版本号。只有重启终端后，输入`rustc -V`出现版本号即表示安装成功。


## Rust基础学习
### 输出命令行与创建项目
&ensp;&ensp;`cargo new project-name`即可快速创建一个`Rust`项目，在`src`文件夹下找到对应的`main.rs`文件即可开始进行`hello world`。终端`cd`进入`src`文件夹，输入`rustc main.rs`会在`src`文件夹下生成`main可执行文件`,`./main`就会发现终端顺利的输输出了`hello world`。

```rust
fn main() { 
    println!("hello world"); 
}
```
## Rust的基础语法
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
Rust与其它语言一样支持32位浮点数(f32)和64位浮点数(f64)。默认情况下，64.0 将表示 64 位浮点数，因为现代计算机处理器对两种浮点数计算的速度几乎相同，但 64 位浮点数精度更高。

```rust
