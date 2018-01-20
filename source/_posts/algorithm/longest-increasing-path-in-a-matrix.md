---
title: Longest Increasing Path in a Matrix
date: 2017-09-25 15:38:38
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 3 周的作业
- 题目来源：[LeetCode #329](https://leetcode.com/problems/longest-increasing-path-in-a-matrix)
- 题目类型：Topological Sort
- 编程语言：C++
- 运行结果：![](images/result.png)

## 问题描述

![](images/description.png)

## 问题分析

本题实质是用 DSP 搜索最长路径。不过没必要对图中每个节点都用 DSP 求一遍最长路径，参考拓扑排序只从入度为 0 的节点开始 DSP，在 DSP 过程中记忆每个节点作为起点的最长路径的长度。

具体步骤为：
  + 1. 根据矩阵构建邻接表，并记录和更新节点的入度。构建邻接表时，要注意满足题意即：当某节点小于上下左右节点才有一条相应路径。
  + 2. DSP 搜索，搜索入口为入度为 0 的结点。

## 源代码

```Java
class Solution {
  HashMap<Integer, List<Integer>> maps = new HashMap<Integer, List<Integer>>();// 有向图的邻接表
	int lens[];// 记住每个节点作为起点的最大长度
    
  public int longestIncreasingPath(int[][] matrix) {
    if (matrix.length == 0)
		return 0;
		
		int rows = matrix.length, cols = matrix[0].length;
		int[] digree = new int[rows * cols];// 入度
		lens = new int[rows * cols];
		
		// 初始化邻接表
		for (int i = 0; i < rows; i++) {
			for (int j = 0; j < cols; j++) {
				List<Integer> neibors = new LinkedList<Integer>();
				if (i - 1 >= 0 && matrix[i - 1][j] > matrix[i][j]) {// 上
					neibors.add((i - 1) * cols + j);
					digree[(i - 1) * cols + j]++;
				}
				if (i + 1 < rows && matrix[i + 1][j] > matrix[i][j]) {// 下
					neibors.add((i + 1) * cols + j);
					digree[(i + 1) * cols + j]++;
				}
				if (j - 1 >= 0 && matrix[i][j - 1] > matrix[i][j]) {// 左
					neibors.add(i * cols + (j - 1));
					digree[i * cols + (j - 1)]++;
				}
				if (j + 1 < cols && matrix[i][j + 1] > matrix[i][j]) {// 右
					neibors.add(i * cols + (j + 1));
					digree[i * cols + (j + 1)]++;
				}
				maps.put(i * cols + j, neibors);
			}
		}
		int max = 0;
		for (int i = 0; i < rows; i++) {
			for (int j = 0; j < cols; j++) {
				if (digree[i * cols + j] == 0) {//入度为0的节点为搜索入口
					int len = dsp(i * cols + j, 0);
					max = Math.max(max, len);
				}
			}
		}
		return max;
  }
    
  public int dsp(int s, int depth) {
		if (lens[s] != 0)
			return lens[s];

		List<Integer> neibors = maps.get(s);
		int max = 0;
		for (int n : neibors) {
			lens[n] = dsp(n, depth++);
			max = Math.max(max, lens[n]);
		}
		return max + 1;
	}
}
```
