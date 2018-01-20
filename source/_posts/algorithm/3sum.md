---
title: 3Sum
date: 2017-11-06 14:11:58
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 9 周的作业
- 题目来源：[LeetCode #15](https://leetcode.com/problems/3sum)
- 题目类型：Divide and Conquer
- 编程语言：C++
- 运行结果：![](images/result.png)

## 问题描述

![](images/description.png)

## 问题分析

假设 3sum 问题的目标是 target。每次从数组中选出一个数 k，从剩下的数中求目标等于 target-k 的 2sum 问题。这里需要注意的是有个小的 trick：当我们从数组中选出第 i 数时，我们只需要求数值中从第 i+1 个到最后一个范围内字数组的 2sum 问题。假设数组为 A[]，总共有 n 个元素 A1，A2....An。很显然，当选出 A1 时，我们在子数组 [A2~An] 中求目标位 target-A1 的 2sum 问题，我们要证明的是当选出 A2 时，我们只需要在子数组 [A3~An] 中计算目标位 target-A2 的 2sum 问题，而不是在子数组 [A1,A3~An] 中，证明如下：

假设在子数组 [A1, A3~An] 目标位 target-A2 的 2sum 问题中，存在 A1+m = target-A2（m 为 A3~An 中的某个数），即 A2+m = target-A1，这刚好是“对于子数组 [A2~An]，目标为 target-A1 的 2sum 问题”的一个解。即我们相当于对满足 3sum 的三个数 A1+A2+m = target 重复计算了。因此为了避免重复计算，在子数组 [A1，A3~An] 中，可以把 A1 去掉，再来计算目标是 target-A2 的 2sum 问题。

## 源代码

```C++
class Solution {
public:
    vector< vector<int> > threeSum(vector<int>& nums) {
        int n = nums.size();
        vector< vector<int> > res;
        if (n < 3)
            return res;
        sort(nums.begin(), nums.end());
        for(int i = 0; i < n - 2; i++)
        {
            if(i == 0 ||  (i > 0 && nums[i] != nums[i - 1]))
            {
                int left = i + 1, right = n - 1;
                int sum = 0 - nums[i];
                while(left < right)
                {
                    if(nums[left] + nums[right] == sum)
                    {
                        vector<int> vi;
                        vi.push_back(nums[i]);
                        vi.push_back(nums[left]);
                        vi.push_back(nums[right]);
                        res.push_back(vi);
                        while(left < right && nums[left] == nums[left + 1])
                            left++;
                        while(left < right && nums[right] == nums[right - 1])
                            right--;
                        left++;
                        right--;
                    }
                    else if(nums[left] + nums[right] < sum)
                        left++;
                    else
                        right--;
                }
            }
        }
        return res;
    }
};
```