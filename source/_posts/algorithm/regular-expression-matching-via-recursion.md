---
title: Regular Expression Matching (via Recursion)
date: 2017-11-27 10:54:28
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 12 周的作业
- 题目来源：[LeetCode #10](https://leetcode.com/problems/regular-expression-matching)
- 题目类型：Recursion
- 编程语言：C++
- 运行结果：![](images/result.png)

## 问题描述

![](images/description.png)

## 问题分析

用递归来解，大概思路如下：
  + 若 p 为空，若 s 也为空，返回 true，反之返回 false
  + 若 p 的长度为1，若 s 长度也为1，且相同或是 p 为'.'则返回 true，反之返回 false
  + 若 p 的第二个字符不为 *，若此时 s 为空返回 false，否则判断首字符是否匹配，且从各自的第二个字符开始调用递归函数匹配
  +  若p的第二个字符为 *，若 s 不为空且字符匹配，调用递归函数匹配 s 和去掉前两个字符的 p，若匹配返回 true，否则 s 去掉首字母
  + 返回调用递归函数匹配 s 和去掉前两个字符的 p 的结果

我们先来判断 p 是否为空，若为空则根据 s 的为空的情况返回结果。当 p 的第二个字符为 * 号时，由于 * 号前面的字符的个数可以任意，可以为 0，那么我们先用递归来调用为 0 的情况，就是直接把这两个字符去掉再比较，或者当 s 不为空，且第一个字符和 p 的第一个字符相同时，我们再对去掉首字符的 s 和 p 调用递归，注意 p 不能去掉首字符，因为 * 号前面的字符可以有无限个；如果第二个字符不为 * 号，那么我们就老老实实的比较第一个字符，然后对后面的字符串调用递归，

## 源代码

```C++
// 解法一
class Solution {
public:
    bool isMatch(string s, string p) {
        if (p.empty()) return s.empty();
        if (p.size() == 1) {
            return (s.size() == 1 && (s[0] == p[0] || p[0] == '.'));
        }
        if (p[1] != '*') {
            if (s.empty()) return false;
            return (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p.substr(1));
        }
        while (!s.empty() && (s[0] == p[0] || p[0] == '.')) {
            if (isMatch(s, p.substr(2))) return true;
            s = s.substr(1);
        }
        return isMatch(s, p.substr(2));
    }
};
```

```C++
// 解法二
class Solution {
public:
    bool isMatch(string s, string p) {
        if (p.empty()) return s.empty();
        if (p.size() > 1 && p[1] == '*') {
            return isMatch(s, p.substr(2)) || (!s.empty() && (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p));
        } else {
            return !s.empty() && (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p.substr(1));
        }
    }
};
```
