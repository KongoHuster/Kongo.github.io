---
title: MyTinySTL源码阅读笔记
date: 2020-06-25 09:45:26
categories: 源码阅读
tags:
- 源码阅读
- MyTinySTL
---

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s1.ax1x.com/2020/06/25/N08yid.md.jpg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px;">走向生</div>
</center>

> 此文为阅读MyTinySTL做的笔记，学习代码的风格与设计，以及实现算法

<!-- more -->

### 算法总结

#### 红黑树

##### 1.红黑树产生
平衡二叉树真的很不错，在查找时既有着二叉查找树的优越性，在插入时还能通过调整继续保持着。那么为什么还要使用到红黑树呢？我觉得可以从以下两个方面来考虑：

（1）删除：对于平衡二叉树来说，在最坏情况下，需要维护从被删节点到根节点这条路径上所有节点的平衡性，旋转的量级是O(logN)。但是红黑树就不一样了，最多只需3次旋转就会重新平衡，旋转的量级是O(1)。

（2）保持平衡：平衡二叉树高度平衡，这也就意味着在大量插入和删除节点的场景下，平衡二叉树为了保持平衡需要调整的频率会更高。

注意：在大量查找的情况下，平衡二叉树的效率更高，也是首要选择。在大量增删的情况下，红黑树是首选。

鉴于以上原因，因此我们才使用到了红黑树这种更好的结构。上面提了这么多次红黑树，相信你已经迫不及待的想要认识一下了。下面就正式拉开红黑树的序幕。

##### 2.红黑树的应用场景
- STL中的Map和Set的数据结构
- TreeMap in Java
- HashMap in Java
- 计算机科学中的关联数组
- Linux内核中完全公平调度
- 实时计算
- 计算几何

##### 3.搜索

树的遍历
- 广度优先（BFS）：先访问离根节点最近的点
- 深度优先（DFS）：前序遍历、中序遍历、后续遍历

##### 3.红黑树的性质
- 根节点必须黑色
- 父子不能同为红色
- 从任何一个节点出发，到达叶子节点经过的黑色节点数量必须一致

### 关于C++的积累

#### 1.C/C++/中宏特殊字符的含义及用法总结

在C／C＋＋中，宏定义是由define完成的，宏定义中有几种常见的特殊字符需要我们了解，常用的特殊字符有以下几种：

```
#：在宏展开的时候会将#后面的参数替换成字符串； 字符串化
##:将前后两个的单词拼接在一起； 连接化
#@:将值序列变为一个字符；   字符化
\:将两行连接起来。行连接化
```

#### 2.typedef typename的使用

```
typedef typename std::vector<T>::size_type size_type;
```

语句的意思是：
typedef创建了存在类型的别名，而typename告诉编译器std::vector<T>::size_type是一个类型而不是一个成员。

#### 3.C++中的 =default和=delete
每当我们声明一个有参构造函数时，编译器就不会创建默认构造函数。在这种情况下，我们可以使用default说明符来创建默认说明符。

```
// use of defaulted functions
#include <iostream>
using namespace std;

class A {
public:
    // A user-defined
    A(int x){
        cout << "This is a parameterized constructor";
    }

    // Using the default specifier to instruct
    // the compiler to create the default implementation of the constructor.
    A() = default;
};

int main(){
    A a;          //call A()
    A x(1);       //call A(int x)
    cout<<endl;
    return 0;
} 
```

在C ++ 11之前，操作符delete 只有一个目的，即释放已动态分配的内存。而C ++ 11标准引入了此操作符的另一种用法，即：禁用成员函数的使用。这是通过附加= delete来完成的; 说明符到该函数声明的结尾。

```
// copy-constructor using delete operator 
#include <iostream> 
using namespace std; 
  
class A { 
public: 
    A(int x): m(x) { } 
      
    // Delete the copy constructor 
    A(const A&) = delete;      
    // Delete the copy assignment operator 
    A& operator=(const A&) = delete;  
    int m; 
}; 
  
int main() { 
    A a1(1), a2(2), a3(3); 
    // Error, the usage of the copy assignment operator is disabled 
    a1 = a2;   
    // Error, the usage of the copy constructor is disabled 
    a3 = A(a2);  
    return 0; 
} 
```