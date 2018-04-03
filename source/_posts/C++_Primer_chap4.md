---
title: C++ Primer chap 4
date: 2017-07-30 01:39:11
categories:
  - learning
tags:
  - C++
---
### 取模运算
如果``m % n != 0``，则其结果符号与m的相同。

### 短路求值
对于逻辑与运算：当且仅当左侧运算对象为真时才对右侧运算对象求值
对于逻辑或运算：当且仅当左侧运算对象为假时才对右侧运算对象求值

### 建议：除非必须，否则不用递增递减运算符的后置版本

### 强制类型转换
``cast-nmae<type>(expr)``
- static_cast
- const_cast
- dynamic_cast
- reinterpret_cast
