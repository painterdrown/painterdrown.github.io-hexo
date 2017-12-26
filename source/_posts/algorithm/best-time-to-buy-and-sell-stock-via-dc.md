---
title: Best Time to Buy and Sell Stock (via DC)
date: 2017-12-04 15:37:05
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 13 周的作业
- 题目来源：[LeetCode #121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock)
- 题目类型：Divide and Conquer
- 编程语言：C++
- 运行结果：![](images/result.png)
- 参考：[连续子数组最大和](http://www.cnblogs.com/en-heng/p/3970231.html)

## 问题描述

![](images/description.png)

## 问题分析

> 参考：[Best Time to Buy and Sell Stock (via DP)](https://painterdrown.github.io/algorithm/best-time-to-buy-and-sell-stock-via-dp)

参考上面博客中“问题分析”，先将问题转化为最大连续子数组的问题：<br/>
`prices[n] -> differences[n - 1]`

对于 differences 数组，想要求它的最大连续子数组，运用分治的思想，将 differences 划分为两个相等长度的子数组a, b：

![](images/ab.png)

那么最大连续子数组不外乎下面三种情况：
  + 原数组的最大连续子数组就是 a 的最大连续子数组 ma<br/>
    ![](images/ma.png)
  + 原数组的最大连续子数组就是 b 的最大连续子数组 mb<br/>
    ![](images/mb.png)
  + 原数组的最大连续子数组同时在 a, b 中间，即下图中的 mc<br/>
    ![](images/mc.png)

即 result = max(sum(ma), sum(mb), sum(mc))。
其中，sum(mc) = 从中间元素开始往左累加的最大值 + 从中间元素往右累加的最大值。

## 源代码

```C++
// 相比于动态规划的解法 O(n)，这里的算法复杂度是 O(nlogn)
class Solution {
private:
    vector<int> differences;
public:
    int max2(int a, int b) {
        return a > b ? a : b;
    }
    
    int max3(int a, int b, int c) {
        int temp = a > b ? a : b;
        return temp > c ? temp : c;
    }
    
    int dc(int l, int u) {
        if (l > u) return 0;
        if (l == u) return max2(0, differences[0]);
        int m = (l + u) / 2;
        int sum;
        int lmax = 0, rmax = 0;
        sum = 0;
        for (int i = m; i >= l; --i) {
            sum += differences[i];
            lmax = max2(lmax, sum);
        }
        sum = 0;
        for (int i = m + 1; i <= u; ++i) {
            sum += differences[i];
            rmax = max2(rmax, sum);
        }
        return max3(dc(l, m), dc(m + 1, u), lmax + rmax);
    }
    
    int maxProfit(vector<int>& prices) {
        int n = (int)prices.size();
        for (int i = 1; i < n; ++i) {
            differences.push_back(prices[i] - prices[i - 1]);
        }
        // 注意这里是 n - 2，因为 differences 比 prices 少一个元素
        return dc(0, n - 2);
    }
};
```