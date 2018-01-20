---
title: Best Time to Buy and Sell Stock (via DP)
date: 2017-12-11 15:37:05
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 14 周的作业
- 题目来源：[LeetCode #121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock)
- 题目类型：Dynamic Programming
- 编程语言：C++
- 运行结果：![](images/result.png)

## 问题描述

![](images/description.png)

## 问题分析

乍一看，这道题的做法应该是找出最大的 prices[j] - prices[i]，其中 i < j。
最容易想到的做法就是穷举，但是时间复杂度看不下去🙈。

转化一下思路，如果我们由 prices 得到相邻两台的股票差价数组 differences。
则原问题就转化为求 differences 的最大连续子数组问题了！

最大子数组问题有比较多的解法：
  + 穷举
    ```C
    // C

    int maxProfit(int* differences, int n) {
      int result = 0
      for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
          int sum = 0;
          for (int k = i; k <= j; ++k) {
            sum += differences[k];
          }
          result = max(result, sum);
        }
      }
      return result;
    }
    ```
  + 分治
    > 参考：[Best Time to Buy and Sell Stock (via DC)](https://painterdrown.github.io/algorithm/best-time-to-buy-and-sell-stock-via-dc)
  + 动态规划

这里要介绍的是动态规划：我们用 c[i] 来表示前 i 个元素组成的子数组的最大和，则有：<br/>
`c[i] = max(c[i - 1] + differences[i], differences[i])`<br/>
注意，上面说的子数组是肯定包括 differences[i]，但是最大和不一定包括 differences[i]。
因此，最大和应该是：max{ c[0], ..., c[n - 1] }，其中 n 是数组 differences 的长度。

## 源代码

```C++
// C++

class Solution {
  public:
    int max(int a, int b) {
      return a > b ? a : b;
    }

    int maxProfit(vector<int>& prices) {
      int n = (int)prices.size();
      if (n <= 1) return 0;
      int *c = new int[n];
      c[0] = 0;
      for (int i = 1; i < n; ++i) {
          int difference = prices[i] - prices[i - 1];
          c[i] = max(c[i - 1] + difference, difference);
          if (c[i] < 0) c[i] = 0;
      }
      int result = 0;
      for (int i = 0; i < n; ++i) {
          result = max(result, c[i]);
      }
      return result;
    }
};
```



```Java
// Java

// 改进版本 (Kadane 算法)
// 用 maxEndingHere 表示以当前为结束的子数组的最大和，以替代数组 c
public int maxProfit(int[] prices) {
  int maxEndingHere = 0, maxSoFar = 0;
  for (int i = 1; i < prices.length; i++) {
    maxEndingHere += prices[i] - prices[i - 1];
    maxEndingHere = Math.max(maxEndingHere, 0);
    maxSoFar = Math.max(maxEndingHere, maxSoFar);
  }
  return maxSoFar;
}
```
