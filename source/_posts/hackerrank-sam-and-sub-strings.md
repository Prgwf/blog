---
title: hackerrank-sam-and-sub-strings
mathjax: true
date: 2018-01-05 12:38:27
  - algorithm
  - HackerRank
tags:
  - 思维
  - DP
---

#### 题意：
给一个很长的数字字符串，求这个串所有子串加起来的值。

#### 思路：
状态表示： $dp[i]$以i结尾字符串的贡献
转移方程： $dp[i]=dp[i-1] * 10+(i+1) * s[i]$

#### 代码：
```cpp
void work(istream &in, ostream &out) {
  string x;
  in >> x;

  int n = x.size();
  ll ans = 0;
  ll t = 0;
  for (int i = 0; i < n; ++i) {
    t = (t * 10LL + 1LL * (i + 1) * (x[i] - '0')) % moder;
    ans = (t + ans) % moder;
  }
  out << ans << endl;
}
```
