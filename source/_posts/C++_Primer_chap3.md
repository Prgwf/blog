---
title: C++ Primer chap3
date: 2017-07-21 22:26:34
categories:
  - learning
tags:
  - C++
---
### getline
使用getline读取一整行，其参数是一个输入流和一个string对象，但是最后的换行符被丢掉了。
```cpp
istream &is = cin;
string line;
while (getline(is, line)) {
  cout << line << endl;
}
```
### string
'+'运算：
两个``string``对象相加得到一个新的``string``对象，必须确保``+``两侧的的运算对象至少有一个是``string``
也就是说``string s = "hello" + "world";`` 是错的
```
改成下面的形式是可以的
string s1 = "hello";
string s2 = s1 + "world";
```

<cctype\>中实用的函数：
```cpp
isdigit(c)
isalpha(c)
islower(c)
isupper(c)
tolower(c)
toupper(c)
```

C风格字符串：字符串存放在字符数组中并以空字符结束，也就是在字符串的最后一个字符后面跟一个空字符``'\0'``

### 数组
声明：  
``int (*parray)[10] = &arr;``parray指向一个含有10个整数的数组  
``int (&arrRef)[10] = arr;``arrRef引用一个含有10个整数的数组  
``int *(&array)[10] = ptrs;``array是数组的引用，该数组含有10个指针

初始化：  
``a[10] = {};``10个都是0。

```cpp
int a[] = {0, 1, 2, 3, 4};
auto pa(a); // pa是一个整型指针，指向pa的第一个元素
decltype(a) b = {2, 3, 4, 5, 6}; //返回类型是由5个整数构成的数组
```  

<iterator>里定义了两个的函数
``begin(a)``返回指向a首元素的指针  
``end(a)``返回指向a尾元素下一个位置的指针

```cpp
int a[] = {0, 2, 4, 6, 8};
int *p = &a[2];
int k = p[-2]; //p[-2]是a[0]表示的那个元素
```
也就是说内置下标运算可以处理负值，但string和vector虽然可以执行下标运算但是标准库类型限定下标必须是无符号类型。
