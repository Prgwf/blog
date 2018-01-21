---
title: spfa_check
date: 2018-01-11 13:11:23
mathjax: true
categories:
    - algorithm
tags:
    - 最短路
---

一般的做法是``bfs``的``spfa``判断一下入队次数是否大于n次。
一个更好的想法是，用``dfs``版本的``spfa``，判断存在一点在一条路径上出现多次。
把距离数组初始化为0，这样初始化后第一次只会加负边权松弛，枚举每个点为起点，找到一个负环就停止。
```cpp
void dfs(int x) {
  vis[x] = true;
  for (auto pii : g[x]) {
    int v = pii.first;
    int d = pii.second;
    if (dist[v] > dist[x] + d) {
      dist[v] = dist[x] + d;
      if (vis[v]) {
        flag = true;
        return;
      }
      dfs(v);
    }
  }
  vis[x] = false;    
}
```
