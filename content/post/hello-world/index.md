---
title: Rust
description: rust语言的一些东西
slug: hello-world
date: 2026-04-07 00:00:00+0000
image: back.jpg
categories:
    - Rust Category
tags:
    - Rust Tag
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
我会在这里写一些rust语言相关的东西
## rust基础配置与输出helloworld
- rust的安装
    https://rust-lang.org/zh-CN/：进入rust官网，下载官网提供的安装器
    ![Image 1](image/1-1.png)
    打开之后，无特殊要求直接回车即可，安装会自动安装rust并配置所需要的环境变量
    ![Image 1](image/1-2.png)
    安装成功之后，打开cmd输入rustc -V,检查是否成功安装并查看rust版本
    ![Image 1](image/1-3.png)
- ide的环境配置
    win环境想要是应用ide编辑rust程序可以使用vscode搭配rust相关的插件：
    ![Image 1](image/1-4.png)
- 使用cargo创建rust项目并运行
    在想要创建项目的目录中打开cmd并输入cargo new 项目名，可以自动创建一个rust的初始项目
    ![Image 1](image/1-5.png)
    用vscode打开，项目的入口main函数在src下
    ![Image 1](image/1-7.png)
    在项目中输入cargo run来执行该项目
    ![Image 1](image/1-8.png)
    Rust是一门静态类型、编译型、系统级编程语言，核心特点是“内存安全且无需垃圾回收（GC）”，编译后会生成可执行的二进制文件
    
    如果不使用cargo的话还可以使用rustc来执行rust程序，官网的教学书里已经给出了具体步骤
    
    但需要注意的是，在书写rust时，每当你更改了你的代码，那么你就需要重新编译出新的exe文件之后才能运行
    但使用cargo就不需要考虑这些，书写完之后保存直接cargo run就行
    ![Image 1](image/1-9.png)

## 使用Rust读取json文件
- 步骤思路
    先读取file -> read_to_string将读取出的file以string的类型存下来 -> 将取出来的string转为JsonValue对象 -> 取值
    ```rust
    //使用的依赖
    use std::{fs::File, io::Read};
    use json;
    ```
    1. 第一步
        先获取读取权限，此时获取到的是二进制数据
        ```rust
        let mut file = File::open("json文件位置").expect("文件读取错误")
        ```
    2. 第二步
        将二进制转为string
        ```rust
        let mut json_conten = String::new();
        file.read_to_string(&mut json_content).expect("文件应该能被读")
        ```
    3. 第三步
        将读取到的文件存到json上下文中
        ```rust
        let json_analysis: Result<json::JsonValue, json::Error> = json::parse(json_content.as_str());
        //把上面转为string的json文件转为Result<json::JsonValue, json::Error> 
        ```
    4. 第四步
        解构,拿值
        ```rust
        match json_analysis{
            OK(parsed) => { //如果解析成功则为JsonValue对象
            let pretty = json::stringify_pretty(parsed.clone(),4);
            println!("{}",pretty);

            //剩下的就按照读的json的k v关系取值就行

            }
            Err(e) => { //json::Error
                println!("错误")
            }
        }
        ```
## 开发文件读取模块，支持解析指定格式json文件，并处理异常提取关键业务字段
