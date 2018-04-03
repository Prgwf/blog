---
title: Wannafly 挑战赛1 and 2听课笔记
date: 2017-11-12 21:33:12
mathjax: true
categories:
  - algorithm
  - Wannafly
tags:
---
第一场
#### TreePath
``x->y``的长度奇偶性只与x到根的长度的奇偶性以及y到根的长度的奇偶性有关。
所以``dfs``染色下就好。

#### Xorto
我的想法是预处理出每个区间的异或和，并把产生某个异或和的区间用一个``vector``存下来。
然后枚举每个异或和，排序一下区间，然后对每个区间去二分找一下，统计它右端点左边有相同异或和的区间个数。

#### MMSet2
$diameter(T)$表示树$T$的直径。
答案是$\lceil {diameter(T_{\lbrace v \rbrace}) / 2} \rceil$，因为直径中点的性质就是满足题意的，树心？
每次需要把给出的点集重新构树，这个方法叫虚树。
然后求直径的方法：
- 两遍``bfs``或``dfs``
- 如果$diameter(T)=(a, b)$，那么加入一个点$u$后，$diameter(T \bigcup \{u\}) \in \lbrace (a, b), (a, u), (b, u)\rbrace$

#### Color
答案是最大度数。开小差了，没听具体怎么搞。
比赛的时候和佛祖一起做没搞出来，赛后贪心搞定，每次先染度数大的点。

#### Cut
听不懂。

第二场
#### Cut
这个题也叫Cut...
哈夫曼树。

#### Travel
补题的时候枚举了所有特殊点，其实只要枚举离$(u,v)$最近的两个特殊点就好了。

#### Butterfly
这个题的数据很难搞，可以$O(n^3)$搞搞过去。
补题的时候看到一个题解用了``bitset``，非常方便的搞完了。
具体的思路：
要维护两个形状：
```
|\          /|
| \        / |
|  \      /  |
|   \    /   |
|    \  /    |
```
- $ld[i][j]$表示$(i,j)$点向左下连续X的个数。
- $rd[i][j]$表示$(i,j)$点向右下连续X的个数。
- $dd[i][j]$表示$(i,j)$点向下连续X的个数。

$O(n^2)$处理，那么以$(i,j)$为左上角的左翅膀最大是$min(ld[i][j], dd[i][j])$，右侧同理。
``vector< pair<int, int> > pl[maxn], pr[maxn];``存某一大小翅膀的左右上角坐标。
``bitset<maxn> bl[maxn], br[maxn];``的作用就是用来枚举左右上角的纵坐标，一开始把所有左翅膀的左上角坐标和右翅膀的右上角坐标都置1。
最后枚举答案长度，每次把小于当前枚举长度的那些坐标都置0,然后再枚举一下坐标就可以了，用``(bl[k] & br[k + i - 1]).any()``来判断有没有解。
关键实现
```cpp
  for (int i = n; i >= 1; --i) {
    for (int j = 1; j <= m; ++j) {
      if (grid[i][j] != 'X') continue;
      ld[i][j] = ld[i + 1][j - 1] + 1;
      rd[i][j] = rd[i + 1][j + 1] + 1;
      dd[i][j] = dd[i + 1][j] + 1;

      pl[min(dd[i][j], rd[i][j])].push_back({i, j});
      pr[min(dd[i][j], ld[i][j])].push_back({i, j});
    }
  }

  for (int i = 1; i <= min(n, m); ++i) {
    for (pss & p : pl[i]) {
      bl[p.second][p.first] = 1;
    }
    for (pss & p : pr[i]) {
      br[p.second][p.first] = 1;
    }
  }

  int ans = 0;
  for (int i = 1, j = 0; i <= m; i += 2) {
    while (j < i) {
      for (pss & p : pl[j]) {
        bl[p.second][p.first] = 0;
      }
      for (pss & p : pr[j]) {
        br[p.second][p.first] = 0;
      }
      ++j;
    }
    for (int k = 1; k + i - 1 <= m; ++k) {
      if ((bl[k] & br[k + i - 1]).any()) {
        ans = i;
        break;
      }
    }
  }
```

#### Delete
用到了DAG的性质，然后线段树搞搞就好。等补完题再来水。

#### Mod
暂时还没有人过这个题。
