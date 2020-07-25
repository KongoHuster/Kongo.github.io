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
### 第一章 Linux系统入门
#### 1.阅读服务器系统信息
cat /proc/cpuinfo
```
vendor_id　：CPU制造商     
cpu family　：CPU产品系列代号
model　　　：CPU属于其系列中的哪一代的代号
model name：CPU属于的名字及其编号、标称主频
stepping　  ：CPU属于制作更新版本
cpu MHz　  ：CPU的实际使用主频
cache size   ：CPU二级缓存大小
physical id   ：单个CPU的标号
siblings       ：单个CPU逻辑物理核数
core id        ：当前物理核在其所处CPU中的编号，这个编号不一定连续
cpu cores    ：该逻辑核所处CPU的物理核数
apicid          ：用来区分不同逻辑核的编号，系统中每个逻辑核的此编号必然不同，此编号不一定连续
fpu             ：是否具有浮点运算单元（Floating Point Unit）
fpu_exception  ：是否支持浮点计算异常
cpuid level   ：执行cpuid指令前，eax寄存器中的值，根据不同的值cpuid指令会返回不同的内容
wp             ：表明当前CPU是否在内核态支持对用户空间的写保护（Write Protection）
flags          ：当前CPU支持的功能
bogomips   ：在系统内核启动时粗略测算的CPU速度（Million Instructions Per Second）
clflush size  ：每次刷新缓存的大小单位
cache_alignment ：缓存地址对齐单位
address sizes     ：可访问地址空间位数
power management ：对能源管理的支持，有以下几个可选支持功能
```

### 第二章 Linux内核基础知识


#### 一、C文件编译

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

#### 二、把Vim打造成一个强大的IDE编辑器

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

#### 三、在服务器上建立git仓库
服务器
```
mkdir KongoBlog.git
cd KongoBlog.git
git --bare init 
```
客户端
```
mkdir KongoBlog
cd KongBlog
git init 
git remote add origin ssh://root@ip:/root/KongoBlog.git
```
推送方法
```
git push --set-upstream origin master
```
拉取方法
```
git clone ssh://root@ip:/root/KongoBlog.git
```

### 第三章 内核编译和调试

#### 一、通过QEMU调试ARM Linux内核

##### 1.下载软件

```
sudo apt-get install gdb-multiarch
```
##### 2.内核配置
```
cd runninglinuxkernel_4.0
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabi-
make vexpress_defconfig
make menuconfig
```
记得保证编译的内核包含调试信息
```
Kernel Hacking  --->
Compile-time checks and compiler options --->
  [*] Compile the kernel with debug info
```
重新编译内核
```
make bzImage -j4 
make dtbs
```
在终端输入
```
qemu-system-arm -nographic -M vexpress-a9 -m 100M -kernel arch/arm/boot/zImage -append "rdinit=/linuxrc console=ttyAMA0 loglevel=8" -dtb arch/arm/boot/dts/vexpress-v2p-ca9.dtb -S -s
```
-S 表示QEMU虚拟机会冻结CPU，直到远程GDB输入相应的控制指令为止
-s 表示在1234端口接受GDB的调试连接

在另一中断输入
```
cd runninglinuxkernel_4.0
./run.sh arm32 debug
```
再在另一中断输入
```
cd runninglinuxkernel_4.0
gdb-multiarch --tui vmlinux
(gdb) set architecture arm 
(gdb) target remote localhost:1234
(gdb) b start_kernel
(gdb) c
```

### 第四章 内核模块

#### 1.编写一个简单的内核模块
编译内核
```
cd runninglinuxkernel_4.0
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabi-
make vexpress_defconfig
make 
make bzImage -j4
make dtbs
```
建立文件夹，写好Makefile和my_test.c
```
mkdir simple_module
export BASEINCLUDE=/root/runninglinuxkernel_4.0
vi Makefile
```
```
BASEINCLUDE ?= /root/runninglinuxkernel_4.0

mytest-objs := my_test.o
obj-m :=  my_test.o
path := /root/simple_module

all : 
	$(MAKE) -C $(BASEINCLUDE) M=$(path) modules;

clean :
	$(MAKE) -C $(BASEINCLUDE) SUBDIR=$(PWD) clean;
	rm -f *.ko;
```
my_test.c
```
#include <linux/module.h>
#include <linux/init.h>

static int __init my_test_init(void){
    printk("My first kernel module init.\n");
    return 0;
}

static void __exit my_test_exit(void){
    printk("Goodbye!\n");
}

module_init(my_test_init);
module_exit(my_test_exit);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Kongo");
MODULE_DESCRIPTION("Kongo's first kernel module!");
MODULE_ALIAS("mytest");
```
编译拷贝ko文件
```
make 
cp my_test.ko ../runninglinuxkernel_4.0/kmodule
```
启动qemu
```
./run.sh arm32 
```
装载ko
```
insmod /mnt/my_test.ko
```
卸载ko
```
rmmod my_test.ko
```

#### 2.向内核模块传递参数
实验步骤和实验1相同，源代码如下
```
#include <linux/module.h>
#include <linux/init.h>

static int debug = 1;
module_param(debug, int, 0644);
MODULE_PARM_DESC(debug, "enable debugging information!");

#define dprintk(args...) \
    if(debug){ \
        printk(KERN_DEBUG args); \
    }

static int mytest = 100;
module_param(mytest, int, 0644);
MODULE_PARM_DESC(mytest, "test for module paramter");

static int __init my_test_init(void){
    dprintk("My first kernel module init.\n");
    dprintk("moduel parameter=%d\n",mytest);
    return 0;
}

static void __exit my_test_exit(void){
    printk("Goodbye!\n");
}

module_init(my_test_init);
module_exit(my_test_exit);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Kongo");
MODULE_DESCRIPTION("Kongo's first kernel module!");
MODULE_ALIAS("mytest");
```

###  第五章

#### 1.简单的字符设备
my_demodev.c
```
#include <linux/module.h>
#include <linux/init.h>
#include <linux/fs.h>
#include <linux/uaccess.h>
#include <linux/cdev.h>

#define DEMO_NAME "my_demo_dev"
static dev_t dev;
static struct cdev *demo_cdev;
static signed count = 1;

static int demodrv_open(struct inode *inode, struct file *file){

    int major = MAJOR(inode->i_rdev);
    int minor = MINOR(inode->i_rdev);
    printk("%s: major=%%d, minor=%d\n", __func__, major, minor);
    return 0;
}

static int demodrv_release(struct inode *inode, struct file *file){
    printk("Release the dev.\n");
    return 0;
}


static int demodrv_read(struct file *file, char __user *buf, size_t lbuf, loff_t *ppos){
    printk("%s enter\n", __func__);
    return 0;
}

static int demodrv_write(struct file *file, char __user *buf, size_t count, loff_t *f_pos){
    printk("%s enter\n", __func__);
    return 0;
}

static const struct file_operations demodrv_fops = {
    .owner = THIS_MODULE,
    .open = demodrv_open,
    .release = demodrv_release,
    .read = demodrv_read,
    .write = demodrv_write
};

static int __init simple_char_init(void){
    int ret;
    ret = alloc_chrdev_region(&dev, 0, count, DEMO_NAME);
    if (ret){
        printk("Failed to allocate cha device region.\n");
        return ret;
    }

    demo_cdev = cdev_alloc();
    if (!demo_cdev){
        printk("Cdev_alloc failed.\n");
        goto unregister_chrdev;
    }

    cdev_init(demo_cdev, &demodrv_fops);
    ret = cdev_add(demo_cdev, dev, count);
    if(ret){
        printk("Cdev_add failed\n");
        goto cdev_fail;
    }

    printk("Succeeed register char device: %s\n", DEMO_NAME);
    printk("Major number = %d, minor number = %d\n", MAJOR(dev), MINOR(dev));

cdev_fail:
    cdev_del(demo_cdev);

unregister_chrdev:
    unregister_chrdev_region(dev, count);

    return 0;
}

static void __exit simple_char_exit(void){
    printk("Removing device\n");

    if (demo_cdev)
        cdev_del(demo_cdev);
    
    unregister_chrdev_region(dev, count);
}

module_init(simple_char_init);
module_exit(simple_char_exit);

MODULE_LICENSE("GPL v2");
MODULE_AUTHOR("Kongo");
MODULE_DESCRIPTION("Simple charater device!");
```

test.c
```
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>

#define DEMO_DEV_NAME "/dev/demo_drv"

int main(){

    char buffer[64];
    int fd = open(DEMO_DEV_NAME, O_RDONLY);
    if (fd<0){
        printf("Open devices failed.");
        return -1;
    }
    
    read(fd, buffer, 64);
    close(fd);

    return 0;
}
```

### 第六章 系统调用

#### 1.在ARM32上新增一个系统调用

##### (1)添加补丁
```
cd runninglinuxkernel_4.0/
git am rlk_lab/rlk_basic/chapter_6/lab1/0001-arm32-add-a-new-syscall-which-called-getpuid.patch
```
* 这里我们新添加的系统调用名称为`getpuid`
* `getpuid`使用的系统调用号为`388`

此命令在arch/arm/include/uapi/unisid.h头文件中的417行添加了
```
#define __NR_getpuid (__NR_SYSCALL_BASE+388)
```
在arch/arm/include/unistd.h头文件中添加了
```
#define __NR_syscalls (392)
```

##### (2)编译内核

```
# export ARCH=arm
# export CROSS_COMPILE=arm-linux-gnueabi-
# make vexpress_defconfig
# make menuconfig
# make bzImage -j4
# make dtbs
```

##### (3)编写应用程序并编译

```
# cp rlk_lab/rlk_basic/chapter_6/lab1/test_getpuid_syscall.c kmodules/test_getpuid_syscall.c
# cd kmodules/
# arm-linux-gnueabi-gcc --static -o test_getpuid_syscall test_getpuid_syscall.c 
# cd ..
```

##### (4)启动内核

```
# ./run.sh arm32
```

##### (5)编写并运行应用程序

```
# cd /mnt
/mnt # ./test_getpuid_syscall
call getpuid success, return pid = 809, uid = 0
/mnt # ./test_getpuid_syscall
call getpuid success, return pid = 810, uid = 0
/mnt # 

### 第七章 内存管理

- vmalloc和kmalloc的区别
- slab缓存的创建