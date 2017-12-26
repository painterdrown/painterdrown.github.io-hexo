---
title: Minimum Path Sum (via Dijkstra)
date: 2017-12-18 15:48:35
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 16 周的作业
- 题目来源：[LeetCode #64](https://leetcode.com/problems/minimum-path-sum)
- 题目类型：Greedy
- 编程语言：C++

## 问题描述

![](images/description.png)

## 问题分析

Dijkstra 算法是典型的单源最短路径算法，用于计算一个节点到其他所有节点的最短路径。算法的基本思想是贪心算法，从源节点开始，不断地以最小代价向其他未到达的节点拓展，直到覆盖目标节点为止。

在这个问题中，假设输入的矩阵是 matrix（m 行 n 列），算法的具体过程如下：
1. 初始化：
  + 集合 A = { v0 }。v0 起点，A 代表代表已经到达的节点
  + bool last[m][n] 用于记录达到该点的上一个点，true 表示上方的点，false 表示左边的点
2. 从 A 中任意点开始走一步，假设最近到达的节点为 v，则将 v 加到集合 A 中
3. 重复步骤 2，直到终点也在 A 中
4. 最后根据 last 矩阵从终点追溯到起点，即可获取该最短路径，将路径上的值求和即为所求最短路径之和

## 源代码

这次算法比较简单因此不再赘述，参考下面的数据结构与上面的 4 个步骤即可。

```C++
# include <vector>
using namespace std;

struct Node {
    int row;
    int col;
};

// 用于记录已经达到的 Node
vector<struct Node> A;

// 用于记录达到该 Node 的上一个 Node 的相对方向
bool **last = new bool*[m];
for (int i = 0; i < m; ++i) {
  last[i] = new bool[n];
}
```