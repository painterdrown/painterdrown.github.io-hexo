---
title: Minimum Path Sum (via Floyd)
date: 2017-12-25 14:09:58
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 16 周的作业
- 题目来源：[LeetCode #64](https://leetcode.com/problems/minimum-path-sum)
- 题目类型：Dynamic Programming
- 编程语言：C
- 运行结果：![](images/result.png)

## 问题描述

![](images/description.png)

## 问题分析

最短路径问题其实是一个很常见的算法问题，路径是否为有向对应的解法也是不同的。
  + 对于有向路径，可以通过 dist(u, v) = dist(u, k) + length(k, v) 的动态规划方法来求解
  + 对于无向路径，可以通过：
    + dist(u, v) = min{ dist(u, v), dist(u, k) + dist(k, v)} 的动态规划方法来求解（Floyd 算法）
    + Dijkstra 的贪心算法来求解，详见：[Minimum Path Sum (via Dijkstra)](https://painterdrown.github.io/algorithm/minimum-path-sum-via-dijkstra)

这次要讨论的解决方法是 Floyd 算法：Floyd 算法是一个经典的动态规划算法。从任意节点 u 到任意节点 v 的最短路径不外乎 2 种可能：
  + 直接从 u 到 v
  + 从 u 经过某个中间节点 k 再到 v

所以，我们假设 dist(u, v) 为节点 u 到节点 v 的最短路径的距离。对于每一个其他的中间节点 k，我们检查：<br/>
`dist(u, v) > dist(u, k) + dist(k, v)`<br/>
是否成立。如果成立，证明从 u 到 k 再到 v 的路径比 u 直接到 v 的路径短，我们便更新：<br/>
`dist(u, v) = dist(u, k) + dist(k, v)`<br/>
这样一来，当我们遍历完所有节点k，dist(u, v) 便是 u 到 v 的最短路径的距离。

但是注意一下题目到描述：
> You can only move either down or right at any point in time.

因此，可以将本题目视为一个有向的最短路径题目，假设题目中输入的矩阵是 matrix。dist(i, j) 表示从 matrix[0, 0] 到达 matrix[i, j] 到最短路径之和，则有：
  + dist(i, j) = dist(i, j - 1) + matrix[i, j]，当 dist[i, j - 1] 较小时
  + dist(i, j) = dist(i - 1, j) + matrix[i, j]，当 dist[i - 1, j] 较小时

最终，dist(m - 1, n - 1) 为所求最短路径之和（m, n 分别代表 matrix 的行数和列数）。

## 源代码

```C
// 非递归版本

int min(int a, int b) {
    return a < b ? a : b;
}

int minPathSum(int** matrix, int m, int n) {
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i == 0 && j == 0) continue;
            else if (i == 0) matrix[0][j] = matrix[0][j-1] + matrix[0][j];
            else if (j == 0) matrix[i][0] = matrix[i-1][0] + matrix[i][0];
            else matrix[i][j] = min(matrix[i][j-1], matrix[i-1][j]) + matrix[i][j];
        }
    }
    return matrix[m-1][n-1];
}
```

```C
// 递归版本

int min(int a, int b) {
    return a < b ? a : b;
}

int dist(int** matrix, int i, int j) {
    if (i == 0 && j == 0) return matrix[0][0];
    else if (i == 0) return dist(matrix, 0, j - 1) + matrix[0][j];
    else if (j == 0) return dist(matrix, i - 1, 0) + matrix[i][0];
    else return min(dist(matrix, i - 1, j), dist(matrix, i, j - 1)) + matrix[i][j];
}

int minPathSum(int** matrix, int m, int n) {
    return dist(matrix, m - 1, n - 1);
}
```