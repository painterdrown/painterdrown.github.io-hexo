---
title: Regular Expression Matching (via DP)
date: 2017-11-27 10:54:28
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 11 周的作业
- 题目来源：[LeetCode #10](https://leetcode.com/problems/regular-expression-matching)
- 题目类型：Dynamic Programming
- 编程语言：C++
- 运行结果：![](images/result.png)

## 问题描述

![](images/description.png)

## 问题分析

> 参考：[Regular Expression Matching (via Recursion)](https://painterdrown.github.com/algorithm/median-of-two-sorted-arrays/)

我们也可以用 DP 来解，定义一个二维的 dp 数组，其中 dp[i][j] 表示 s[0,i) 和 p[0,j) 是否 match，然后有下面三种情况：

+ P[i][j] = P[i - 1][j - 1], if p[j - 1] != '*' && (s[i - 1] == p[j - 1] || p[j - 1] == '.')
+ P[i][j] = P[i][j - 2], if p[j - 1] == '*' and the pattern repeats for 0 times
+ P[i][j] = P[i - 1][j] && (s[i - 1] == p[j - 2] || p[j - 2] == '.'), if p[j - 1] == '*' and the pattern repeats for at least 1 times

## 源代码

```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();
        vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
        dp[0][0] = true;
        for (int i = 0; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (j > 1 && p[j - 1] == '*') {
                    dp[i][j] = dp[i][j - 2] || (i > 0 && (s[i - 1] == p[j - 2] || p[j - 2] == '.') && dp[i - 1][j]);
                } else {
                    dp[i][j] = i > 0 && dp[i - 1][j - 1] && (s[i - 1] == p[j - 1] || p[j - 1] == '.');
                }
            }
        }
        return dp[m][n];
    }
};
```
