---
title: Merge k Sorted Lists
date: 2017-10-30 14:27:01
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 8 周的作业
- 题目来源：[LeetCode #23](https://leetcode.com/problems/merge-k-sorted-lists)
- 题目类型：Divide and Conquer
- 编程语言：C++
- 运行结果：![](images/result.png)

## 问题描述

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

## 问题分析

> 参考：[Merge Two Sorted Lists](https://painterdrown.github.com/algorithm/merge-two-sorted-lists)

这道题让我们合并 k 个有序链表，参考 Merge Two Sorted Lists 混合插入有序链表，是混合插入两个有序链表。这道题增加了难度，变成合并 k 个有序链表了，但是不管合并几个，基本还是要两两合并。那么我们首先考虑的方法是能不能利用之前那道题的解法来解答此题。

答案是肯定的，但是需要修改，怎么修改呢，最先想到的就是两两合并，就是前两个先合并，合并好了再跟第三个，然后第四个直到第 k 个。这样的思路是对的，但是效率不高，没法通过OJ，所以我们只能换一种思路，这里就需要用到分治法。

简单来说就是不停的对半划分，比如k个链表先划分为合并两个 k/2 个链表的任务，再不停的往下划分，直到划分成只有一个或两个链表的任务，开始合并。举个例子来说比如合并 6 个链表，那么按照分治法，我们首先分别合并 1 和 4, 2 和 5, 3 和 6。这样下一次只需合并 3 个链表，我们再合并 1 和 3，最后和 2 合并就可以了。

## 源代码

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        if (lists.size() == 0) return NULL;
        int n = lists.size();
        while (n > 1) {
            int k = (n + 1) / 2;
            for (int i = 0; i < n / 2; ++i) {
                lists[i] = mergeTwoLists(lists[i], lists[i + k]);
            }
            n = k;
        }
        return lists[0];
    }
    
    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
        ListNode *head = new ListNode(-1);
        ListNode *cur = head;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                cur->next = l1;
                l1 = l1->next;
            } else {
                cur->next = l2;
                l2 = l2->next;
            }
            cur = cur->next;
        }
        if (l1) cur->next = l1;
        if (l2) cur->next = l2;
        return head->next;
    }
};
```
