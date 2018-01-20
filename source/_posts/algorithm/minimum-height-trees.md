---
title: Minimum Height Trees
date: 2017-10-02 15:30:13
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 4 周的作业
- 题目来源：[LeetCode #310](https://leetcode.com/problems/minimum-height-trees)
- 题目类型：BFS
- 编程语言：Java
- 运行结果：![](images/result.png)

## 问题描述

![](images/description.png)

## 问题分析

直觉就是使用 BFS 来解，每次放入只有一个 edge 的 node（现在的 leaf）。然后直到只剩最上面一层。注意考虑单独的 node（和别的 node 不相连）。比如: [[1,2], [2,3]], n = 6 这种情况，就有 [0, 4, 5] 三个点都不和别的 node 相连。还有 [], n = 2 的时候就要返回 [0, 1]。

## 源代码

```Java
public class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        // adjacent set
        Set<Integer>[] set = new HashSet[n];
        for(int i = 0; i < n; i++) set[i] = new HashSet();
        for(int[] edge : edges) {
            set[edge[0]].add(edge[1]);
            set[edge[1]].add(edge[0]);
        }
        
        // use queue to do bfs
        LinkedList<Integer> q = new LinkedList();
        List<Integer> edge_case = new ArrayList();
        for(int i = 0; i < set.length; i++) {
            if(set[i].size() == 1) q.add(i);
            if(set[i].size() == 0) edge_case.add(i);
        }
        // if cycle
        if(q.size() == 0) return edge_case;
        
        int count = edge_case.size();
        while(count + q.size() < n) {
            int size = q.size();
            count += size;
            while(size-- > 0) {
                int node = q.remove();
                int parent = set[node].iterator().next();
                set[node].remove(parent);
                set[parent].remove(node);
                if(set[parent].size() == 1) q.add(parent);
            }
        }
        return q;
    }
}
```
