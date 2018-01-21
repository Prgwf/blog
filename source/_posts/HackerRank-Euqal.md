---
title: HackerRank-Euqal
date: 2018-01-05 11:22:25
mathjax: true
categories:
  - algorithm
  - HackerRank
tags:
  - 思维
---

#### 题意：
给一些数字，每次操作可以选一个数不变，其余所有数都加上1、2、5。
要求最少的次数使得所有的数都相同。

#### 思路:
逆向想一下，每次操作一个数，让这个数减1、2、5, 最后使得每个数相同。
自然就想到了所有数都减到``min{a[i]}``。
每个数的操作次数是 $k=(x-min)/5+((x-m)\%5)/2+((x-m)\%5)\&1$
答案就是每个数操作次数之和

#### trick:
但是并不一定是减到``min{a[i]}``是最优的，也有可能是``min{a[i]}-1``, ``min{a[i]}-2``, ``min{a[i]}-3``, ``min{a[i]}-4``,但不可能比最小值小5,这说明每个数都需要减一个5。

#### 代码：
```cpp
/*
教练我想打ACM～
*/
void work(istream &in, ostream &out) {
  int T;
  in >> T;

  while (T--) {
    int n;
    in >> n;
    vector<int> a;
    a.resize(n);

    for (int i = 0; i < n; ++i) {
      in >> a[i];
    }

    int Min = *min_element(a.begin(), a.end());
    int ans = inf;
    for (int i = 0; i <= 5; ++i) {
      int temp = 0;
      for (int j = 0; j < n; ++j) {
        int k = (a[j] - (Min - i));
        temp += k / 5;
        k %= 5;
        temp += k / 2;
        temp += k & 1;
      }
      ans = min(ans, temp);
    }
    out << ans << endl;
  }
}
```
