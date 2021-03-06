---
<!-- layout: c++ -->
title: C++ Primer chap5&6
date: 2018-04-05 14:31:37
categories:
  - learning
tags:
  - C++
---
异常处理机制：
- throw表达式。
- try语句块，抛出的异常被某个catch子句处理。

局部对象：
形参和函数体内部定义的变量统称为局部变量，仅在函数的作用域内可见；
并且局部变量会隐藏在外层作用域中同名的其他所有声明。
- 自动对象(automatic object)：仅存在于块执行期间的对象。
- 局部静态对象(local static object)：定义语句第一次被执行到时初始化，直到程序终止才被销毁。

函数声明无须函数体，形参名字可以省略。

**如果函数无须改变引用形参的值，最好将其声明为常量引用**

```cpp
void fcn(const int i) {}
void fcn(int i) {/* 重复定义fcn(int)，上面函数的顶层const被忽略。*/}
```

省略符形参 ...
大多数类类型的对象在传递给省略符形参时无法正确拷贝。
