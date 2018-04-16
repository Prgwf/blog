---
title: C++ Primer chap2
date: 2017-07-21 22:26:29
categories:
  - learning
tags:
  - C++
---
### 显示访问全局变量
``::变量名``

### 类型别名(type alias)
``typedef long long ll;``  
``using ll = long long;``

### const 指针 引用
这个几个东西真的非常搞。  
~~我觉得我要掉进C++的语法坑了~~

"不能定义指向引用的指针，但存在对指针的引用"  
其实是不能出现``int &*p``，但``int *&p``是可以的。  
``int *&p``是对指针的引用，这个在学习Treap的时候已经碰到了。  

``const int &p``对常量的引用，右侧不是一个常量是不要紧的，但是不能通地过``p = 0``去改变其绑定对象的值。  

指向常量的指针(pointer to const)  
``const double pi = 3.14;``  
``const double *cptr = pi;``  
**指针所指内容不可变**

常量指针(const pointer)  
``double pi = 3.14;``  
``double *const cstp = pi;``  
**指针的指向地址不可变**

顶层const：指针本身是个常量
底层const：指针所指的对象是个常量

### 常量表达式
指值不会改变并且在编译过程中就能得到计算结果的表达式。

``constexpr``是C++11新标准规定的玩意，如果认定变量是一个常量表达式，就把它声明成``constexpr``
### auto
auto让编译器通过初始值来推算变量的类型。
- auto定义变量必须有初始值。
- auto能在一条语句中声明多个变量，该语句中所有变量的初始基本数据类型必须一样。

### decltype
``decltype``选择并返回操作数的数据类型。  
``decltype(f()) sum = x;``

```
int i = 1, *p = &i;
decltype(*p) j; // 错误，j是int&，因为*p是解引用。
```
