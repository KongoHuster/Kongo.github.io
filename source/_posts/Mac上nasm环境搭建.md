---
title: Mac上汇编环境搭建
date: 2020-06-29 12:12:26
categories: Mac
tags:
- Mac
- Nasm
- DOSBox
---

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s1.ax1x.com/2020/06/29/NWb44U.md.jpg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">星路</div>
</center>

> 此文为学习汇编语言程序设计写代码时搭建的环境笔记

<!-- more -->

### 一、下载nasm
```
$ brew install nasm
```
```
vi HelloWord.asm
```
```
SECTION .data
 
msg: db "HelloWorld!", 0x0a
len: equ $-msg
 
SECTION .text
global _main
 
_main:
    mov rax,0x2000004  ;0x2000004 表示 syscall 调用号 write
    mov rdi,1          ;表示控制台输出
    mov rsi,msg        ;syscall 调用会到 rsi 来获取字符
    mov rdx,len        ;字符串长度
    syscall
 
    mov rax,0x2000001  ;0x2000001 表示退出 syscall
    mov rdi,0
    syscall
```
编译
```
$ nasm -f macho64 HelloWorld.asm -o HelloWorld.o
```
链接
```
$ ld -o HelloWorld -e _main HelloWorld.o -macosx_version_min 10.15 -static
```
运行
```
./HelloWorld
```

### 二、Mac 通过DOSBox搭建汇编环境

> 该教程主要参考[此博客](https://www.e-learn.cn/topic/3366973)，学习汇编的资料在https://github.com/honghong1234/assembly-exercise

#### 1.下载安装相关工具
选择Mac版的DOSBox
```
https://www.dosbox.com/download.php?main=1
```
下载汇编常用工具
```
链接: https://pan.baidu.com/s/1p5jMRxx0ISPCdOy1cIqR3Q  密码: u97w
```

#### 2.在用户目录下新建DOSBox目录

制作目录
```
mkdir ~/DOSBox/
```
修改配置文件
```
vi /Users/yzh/Library/Preferences/DOSBox\ 0.74-3-1\ Preferences
```
文件最后添加
```
# 挂载~/DOSBox目录为C盘
mount C ~/DOSBox
# 进入C盘（~/DOSBox目录）
C:
```
启动BOSBox

#### 3.在DOSBox目录下编写helloworld测试DOSBox环境
在桌面新建hello.asm文件，用记事本打开
```
DATAS SEGMENT
    ;此处输入数据段代码  
    ;13、10都是十进制，分别表示垂直制表符、退格，'$'表示字符串结尾
    STRING  DB  'Hello World!',13,10,'$'
DATAS ENDS

STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS,SS:STACKS
START:
    MOV AX,DATAS
    MOV DS,AX
    ;此处输入代码段代码
    
    LEA  DX,STRING
    MOV  AH,9
    INT  21H
    
    MOV AH,4CH
    INT 21H
CODES ENDS
    END START
```
汇编hello.asm文件
```
MASM HELLO.ASM #回车三次
```
链接HELLO.OBJ文件
```
LINK HELLO.OBJ
```
HELLO.EXE文件
```
HELLO.EXE
```

#### 4.DOSBox调试功能

- R命令查看、改变CPU寄存器的内容；
- D命令查看内存中的内容；
- E命令改写内存中的内容；
- U命令将内存中的机器指令翻译成汇编指令；
- T命令执行一条机器指令；
- G命令跳转到偏移地址；
- P命令结束循环或者是int 21H时是退出程序；
- A命令是以汇编指令的格式在内存中写入一条机器指令。
