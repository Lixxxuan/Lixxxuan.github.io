---
title: rust学习笔记-1
published: 2025-07-02
description: 'rust学习笔记'
image: ''
tags: [study]
category: ''
draft: false 
lang: ''
---
### 说在前面
&ensp;&ensp;做了一个长期学习nest的计划，每天的学习都以博客的形式记录，同时也作为给自己查询的笔记。
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
