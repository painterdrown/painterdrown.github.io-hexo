---
title: Stingy Sat
date: 2018-01-15 22:15:04
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 19 周的作业
- 题目类型：NP Complete

## 问题描述

STINGY SAT is the following problem: given a set of clauses (each a disjunction of literals) and an integer k, find a satisfying assignment in which at most k variables are true, if such an assignment exists. Prove that STINGY SAT is NP-complete.

## 问题分析

  + 给定一个 STINGY SAT 问题的解，可以在多项式时间内验证。遍历查看是否最多只有 k 个值为真即可。
  + 对于一个 SAT 问题，将 k 赋值给其变量的总数，即可将其转换为 STINGY SAT 问题，所以其转化是可以在多项式时间内完成的。
  + 若 STINGY SAT 有解，则对应的 SAT 问题一定也存在解，其本质相同。
  + 若无解，则对应的 SAT 问题也是无解的。

综上， SAT 问题可以归约到 STINGY SAT 问题，所以 STINGY SAT 问题是 NP Complete 的。
