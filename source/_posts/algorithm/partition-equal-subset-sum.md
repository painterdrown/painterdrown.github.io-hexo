---
title: Partition Equal Subset Sum
date: 2017-12-11 19:25:22
tags: [algorithm, java]
---

## 前言

- 本博客作为《算法与设计》课程第 15 周的作业
- 题目来源：[LeetCode #416](https://leetcode.com/problems/partition-equal-subset-sum)
- 题目类型：Dynamic Programming
- 编程语言：Java
- 运行结果：![](images/result.png)
- 参考：[0-1背包问题](https://www.cnblogs.com/shinning/p/6027743.html)

## 问题描述

![](images/description.png)

## 问题分析

这道题理解起来非常简单，穷举一下数组的子集合，看是否存在一个子集合的加和刚好是原数组加和的一半即可。但穷举...肯定会超时，实际做起来需要动用到动态规划！

分析一下这个问题，其实跟 01-背包问题很是类似。区别其实只在于：01-背包问题中的变量有两个：价值 v 和重量 w。而这道题中，变量只有一个：加和 s。用数组 dp[i][j] 来表示总和不超过 j 时的前 i 个元素的最大和。可得出状态转移方程为：<br/>
```
dp[i][j] = Math.max(dp[i - 1][j - nums[i]] + nums[i], dp[i - 1][j])
```
在本题情况中，dp[i][j] 只与上一层循环得到的dp[i - 1][x] 有关，故只需要一维数组，每次更新数组值即可。

假设 dp[s] 表示在不超过 s 的情况下，子集合最大的加和。用题目中的例子 [1, 5, 11, 5] 来解释就是：
  + dp[1] = 1
  + dp[5] = 5
  + dp[9] = 6（由 1 + 5 得到）
  + dp[10] = 10
  + dp[11] = 11（由 1 + 5 + 5 得到）
  + dp[22] = 22（所有元素相加得到）
值得注意的是，总有：dp[i] <= i 成立。

继续分析，假设输入的数组是 nums，我们首先将其求和，得到 sum。

如果 sum 是基数，肯定不存在满足条件的子数组，直接返回 false 即可。

如果 sum 是偶数，则才有可能存在满足条件的子数组。那如何引入 01-背包的思想呢？如果我们能找到刚好和为 sum / 2 的子数组，那么剩下没选到的元素之和肯定也是 sum / 2。因此就等价于我们在 01-背包问题中找到恰好满足：dp[sum] = sum 的情况，这也就意味着这时的子数组刚好能凑到原数组的一半。

## 源代码

```Java
public class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
        }
        if (sum % 2 != 0) return false; 
        sum /= 2;
        int[] dp = new int[sum + 1];
        for (int i = 0; i < nums.length; i++) {
            // j 要从大到小遍历，避免在更新数组的过程中使用到当次更新的数组元素
            for (int j = sum; j >= nums[i]; j--) {
                dp[j] = Math.max(dp[j - nums[i]] + nums[i], dp[j]);
            }
        }
        return dp[sum] == sum;
    }
}
```
