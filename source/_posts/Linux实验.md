---
title: Linux实验
date: 2020-07-06 16:53:11
categories: Linux
tags:
- Linux内核
- Linux
---


<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s1.ax1x.com/2020/07/06/UPgae1.md.png">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">白发少女</div>
</center>


> 此文为学习《奔跑吧Linux内核》所做实验做的笔记

<!-- more -->
## 第一章 Linux系统入门

## 第二章 Linux内核基础知识

### 一、系统内核基础

#### 1.C文件编译

vi test.c添加一下内容
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define PAGE_SIZE 4096
#define MAX_SIZE 100*PAGE_SIZE

int main()
{

        char *buf = (char *)malloc(MAX_SIZE);
        memset(buf, 0, MAX_SIZE);
        printf("buffer adress = 0x%p\n", buf);
        free(buf);
        return 0;

}
```
预处理 GCC的“-E”选项可以让预处理阶段就结束，-o指定输出文件
```
arm-linux-gnueabi-gcc -E test.c -o test.i
```
编译 “-S”
```
arm-linux-gnueabi-gcc test.i -o test.s
```
汇编 “-c”
```
arm-linux-gnueabi-gcc -c test.s -o test.o
```
链接 “--static”
```
arm-linux-gnueabi-gcc  test.o -o test --static
```
写个Makefile来编译
```
cc = arm-linux-gnueabi-gcc
prom = test 
obj = test.o
CFLAGS = -static

$(prom) : $(obj)
        $(cc) -o $(prom) $(obj) $(CFLAGS)

%.o: %.c
        $(cc) -c $< -o $@

clean:
        rm -rf $(obj) $(prom)
```

#### 2.把Vim打造成一个强大的IDE编辑器

##### 下载Vundle
（1）下载插件管理器Vundle
```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
（2）配置Vundle，编辑Vim的配置文件vimrc
```
sudo vim .vimrc
```
```
set nocompatible              " be iMproved, required
filetype off                  " required

" 启用vundle来管理vim插件
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" 安装插件写在这之后

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" 安装插件写在这之前
call vundle#end()            " required
filetype plugin on    " required

" 常用命令
" :PluginList       - 查看已经安装的插件
" :PluginInstall    - 安装插件
" :PluginUpdate     - 更新插件
" :PluginSearch     - 搜索插件，例如 :PluginSearch xml就能搜到xml相关的插件
" :PluginClean      - 删除插件，把安装插件对应行删除，然后执行这个命令即可
```
（3）安装插件
```
sudo vim
```
```
:PluginInstall
```

##### ctags工具
>ctags的功能：扫描指定的源文件，找出其中所包含的语法元素，并将找到的相关内容记录下来

下载ctags
```
sudo apt-get install ctags
```
递归扫描源代码根目录和子目录的文件并且生成索引文件
```
ctags -R
```
进入vim加载这个tags文件
```
:set tags=tags
```

##### Tagbar插件
```
vim ~/.vimrc
```
```
Plugin 'majutsushi/tagbar'
```
```
sudo vim
```
```
:PluginInstall
```

##### NerdTree插件
```
vim ~/.vimrc
```
```
Plugin 'scrooloose/nerdtree'
```
```
sudo vim
```
```
:PluginInstall
```

##### YouCompleteMe插件
```
vim ~/.vimrc
```
```
Plugin 'valloric/YouCompleteMe'
```
```
sudo vim
```
```
:PluginInstall
```
插件安装完成以后需要重新编译它，编译之前需要下载如下软件包
```
sudo apt-get install build-essential cmake python-dev python3-dev 
```
进入 YouCompleteMe进行编译
```
cd ~/.vim/bundle/YouCompleteMe
./install.py --clang-comleter
```
拷贝配置文件
```
cp ~/.vim/bundle/YouCompleteMe/third_party/ycmd/examples/.ycm_extra_conf.py ~/.vim
```
再在.vimrc配置文件中还需要添加如下配置
```
let g:ycm_server_python_interpreter='/usr/bin/python'
let g:ycm_global_ycm_extra_conf='~/.vim/.ycm_extra_conf.py'
```

#### 3.将本地的博客和服务器上的博客同步