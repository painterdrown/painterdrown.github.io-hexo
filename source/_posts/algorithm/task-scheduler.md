---
title: Task Scheduler
date: 2017-10-16 14:49:58
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 6 周的作业
- 题目来源：[LeetCode #621](https://leetcode.com/problems/task-scheduler)
- 题目类型：Greedy
- 编程语言：C++
- 运行结果：![](images/result.png)

## 问题描述

![](images/description.png)

## 问题分析

这道题让我们安排 CPU 的任务，规定在两个相同任务之间至少隔 n 个时间点。由于题目中规定了两个相同任务之间至少隔 n 个时间点，那么我们首先应该处理的出现次数最多的那个任务，先确定好这些高频任务，然后再来安排那些低频任务。如果任务F的出现频率最高，为 k 次，那么我们用 n 个空位将每两个 F 分隔开，然后我们按顺序加入其他低频的任务，来看一个例子：

`
AAAABBBEEFFGG 3
`

我们发现任务 A 出现了 4 次，频率最高，于是我们在每个A中间加入三个空位，如下：

`
A---A---A---A  (初始)<br/>
AB--AB--AB--A  (加入 B)<br/>
ABE-ABE-AB--A  (加入 E)<br/>
ABEFABE-ABF-A  (加入 F，每次尽可能填满或者是均匀填充)<br/>
ABEFABEGABFGA  (加入 G)
`

再来看一个例子：

`
ACCCEEE 2
`

我们发现任务 C 和 E 都出现了三次，那么我们就将 CE 看作一个整体，在中间加入一个位置即可：

`
CE-CE-CE  (初始)<br/>
CEACE-CE  (加入 A)
`

注意最后面那个 idle 不能省略，不然就不满足相同两个任务之间要隔2个时间点了。

我们仔细观察上面两个例子可以发现，都分成了 (mx-1) 块，再加上最后面的字母，其中 mx 为最大出现次数。

例子 1 中，A 出现了4次，所以有 A--- 模块出现了 3 次，再加上最后的 A，每个模块的长度为 4。

例子 2 中，CE- 出现了 2 次，再加上最后的 CE，每个模块长度为 3。

我们可以发现，模块的次数为任务最大次数减 1，模块的长度为 n+1，最后加上的字母个数为出现次数最多的任务，可能有多个并列。这样三个部分都搞清楚了，写起来就不难了，我们统计每个大写字母出现的次数，然后排序，这样出现次数最多的字母就到了末尾，然后我们向前遍历，找出出现次数一样多的任务个数，就可以迅速求出总时间长了。

## 源代码

```C++
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        vector<int> cnt(26, 0);
        for (char task : tasks) {
            ++cnt[task - 'A'];
        }
        sort(cnt.begin(), cnt.end());
        int i = 25, mx = cnt[25], len = tasks.size();
        while (i >= 0 && cnt[i] == mx) --i;
        return max(len, (mx - 1) * (n + 1) + 25 - i);
    }
};
```
