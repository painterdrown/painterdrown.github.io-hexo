---
title: Merge Two Sorted Lists
date: 2017-10-23 14:37:01
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 7 周的作业
- 题目来源：[LeetCode #21](https://leetcode.com/problems/merge-two-sorted-lists)
- 题目类型：Divide and Conquer
- 编程语言：C++
- 运行结果：![](images/result.png)

## 问题描述

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

## 问题分析

其实就是归并排序的思想：新建一个链表，然后比较两个链表中的元素值，把较小的那个链到新链表中，由于两个输入链表的长度可能不同，所以最终会有一个链表先完成插入所有元素，则直接另一个未完成的链表直接链入新链表的末尾。

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *dummy = new ListNode(-1), *cur = dummy;
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
        cur->next = l1 ? l1 : l2;
        return dummy->next;
    }
};
```