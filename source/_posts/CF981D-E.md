---
title: CF981D/E补题
date: 2018-05-28 16:28:55
mathjax: true
categories:
  - algorithm
  - Codeforces
tags:
  - DP
  - bitset
  - 线段树
---
世界上有10种人，一种懂二进制，一种不懂(我)。
### 981D
感谢蔡队抬一手。

题意：长度为$n$的数组，分成连续的$k$段，段内加法，段间按位与，最后求最大值。
首先二进制按位贪心，即从高位开始贪心当前位$1$是否可行；
判断可行性就需要dp，这里需要满足在前面贪心出来的答案也可行；
状态定义：$dp[i][k]$ 表示第k段以i结尾时可行。
转移方程：$dp[j][k+1] = ((sum[j]-sum[i-1]) \& mask)==mask$，枚举第$k+1$段右端点$j$时，条件是$dp[i-1][k]=true$(第$k$段右端点是$i-1$时可行)
复杂度是$O(log(S) \times n \times n \times k)$

### 981E
这个题就有点厉害了。

题意：有一段区间$[1, n]$初值都是$0$，$k$个操作，每次把一段区间$[l, r]$加上一个$x$，问$[1, n]$中所有可能出现的最值。
解法是：线段树+背包
区间赋值想到的肯定是线段树打``lazy标记``；
$dp[i]$定义成$i$是最大值是否可行；
看大佬们很骚的操作用``bitset<N> b``来加速背包；
``b |= b << x``整体左移x位（补零），因为用了$|=$那么相当于枚举了操作的子集，而且这样就直接完成了上层状态加/不加$x$后能转移到的位置；
最后算一下bitset在$[1, n]$中为$1$的位置。
```cpp
int n, k;
vector<int> tr[N * 4];
bitset<N> b, ans;
void update(int ll, int rr, int x, int l=1, int r=n, int o=1) {
  if (ll <= l && r <= rr) {
    tr[o].push_back(x);
    return;
  }
  int m = (l + r) / 2;
  if (ll <= m) update(ll, rr, x, lson);
  if (rr > m) update(ll, rr, x, rson);
}
void query(int l, int r, int o, bitset<N> b) {
  for (int x : tr[o]) {
    b |= (b << x);
  }
  if (l == r) {
    ans |= b;
    return;
  }
  int m = (l + r) / 2;
  query(lson, b);
  query(rson, b);
}
int main(int args, char const *argv[]) {
  // freopen("data.in", "r", stdin);
  // freopen("data.out", "w", stdout);

  scanf("%d %d", &n, &k);
  for (int i = 0; i < k; ++i) {
    int l, r, x;
    scanf("%d %d %d", &l, &r, &x);
    update(l, r, x);
  }
  b.set(0);
  query(1, n, 1, b);
  int cnt = 0;
  for (int i = 1; i <= n; ++i) if (ans[i]) cnt++;
  printf("%d\n", cnt);
  for (int i = 1; i <= n; ++i) if (ans[i]) {
    printf("%d ", i);
  }
  return 0;
}
```
