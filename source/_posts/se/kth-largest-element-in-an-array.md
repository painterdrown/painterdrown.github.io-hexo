---
title: Kth Largest Element in an Array
date: 2017-09-18 13:11:47
tags: [se, algorithm, c]
---

## 前言

- 本博客作为《算法与设计》课程第二周的作业
- 题目来源：[LeetCode 215](https://leetcode.com/problems/kth-largest-element-in-an-array)
- 题目类型：Divide and Conquer
- 编程语言：C
- 运行结果：![](imgs/1.png)

## 问题描述

Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

For example,
Given [3,2,1,5,6,4] and k = 2, return 5.

Note: 
You may assume k is always valid, 1 ≤ k ≤ array's length.

## 问题分析

题目的要求很清晰，就是要求出一个整数数组中第k大的元素。
最容易想到的做法，也是最简单粗暴的做法，就是先从大到小排序，然后取排序后数组的第k个元素即可。

但是仔细一想，这种做法是不是有点浪费了？你将一整个数组排好序，但最终只是利用了第k个元素。
所以我们是不是可以找出一个比较节能的算法，使得时间复杂度上做到更优呢？

排序算法的时间复杂度最快是O(nlogn)，用Quicksort或者Mergesort可以做到。
观察一下这道题，从某种程度上来说，它的做法跟排序的做法应该区别不大。
Quicksort和Mergesort算法都是来源于分治思想，因此，我们可以以此为切入点。

假如我们随机地从数组中选取一个元素作为主元（pivot），然后线性遍历一下数组，将其分成三个子数组：
- 小于pivot的，记作[small]，假设其长度为small_len
- 等于pivot的，记作[equal]，假设其长度为equal_len
- 大于pivot的，记作[large]，假设其长度为large_len

做完分组之后，我们可以这样表示原来的数组：[[small], [equal], [large]]。
后面为了实际操作的方便，我将表示为[[small], [large], [equal]]。

接下来我们可以作出以下的分析：
- 如果**k <= large_len**，即有不止k个元素大于pivot，所以pivot是在要找的第k大的元素之后，此时我们继续递归地在[large]子数组中继续寻求第k大的元素
- 如果**large_len < k <= large_len + equal_length**，即第k大的元素落在[equal]子区间内，也就是此时对应的pivot
- 如果**large_len + equal_length < k**，即[large]和[equal]两个数组的元素数目之和都没有k大，那么我们就还需要在[small]里面找到子数组的第(k - large_len - equal_len)大的元素

上面三种情况对应的结果是：findKthLargest(初始数组, k) =
- findKthLargest(大于pivot的数组, k)
- pivot
- findKthLargest(小于pivot的数组, k - large_len - equal_len)

算法其实不复杂，就是下面两步：
- STEP 1. 将数组划分成三个子数组，可参考Quicksort的划分算法![](imgs/2.png)

划分时，比Quicksort多出来的一步是：将与主元相等的元素交换到数组的最右边。这也是我上面提到为什么记成[[small], [large], [equal]]的原因了。

- STEP 2. 根据三个子数组的长度判断是否已经取到目标元素，若否则递归求解之

时间复杂度上，
- 最优的情况是每次取到的主元都是当前数组的中位数，这样能使原数组划分后的[large]和[small]的长度大致相同。T(n) = T(n/2) + O(n)，由大师定理得：时间复杂度是O(n)
- 最坏的情况当然是每次都取到当前数组的最小值。T(n) = n + (n-1) + ... + n/2 = O(n^2)，时间复杂度是O(n^2)

因此我们定义一种“不错”的情况：选取的主元位置落在元素组的0.25到0.75之间（概率为50%，我们平均需要做两次选取主元操作才能选取到那个“不错”的主元），这时候的时间复杂度存在一个上界：

T(n) <= T(3n/4) + O(n)

其中，T(3n/4)是指主元刚刚落在0.25或0.75这两个边界，O(n)是进行分组（线性遍历）时花费的时间。
根据大师定理可以得出对应的时间复杂度为O(n)。

因此我们可以得出结论：
- 该算法最坏情况下的时间复杂度为O(n^2)，但是发生的几率极低
- 该算法最优情况下的时间复杂度为O(n)，发生的几率也很低
- 该算法“不错”情况下的时间复杂度为O(n)，发生的几率很高，可以视作该算法的平均复杂度

## 源代码

```C
void swap(int *a, int *b) {
  int temp = *a;
  *a = *b;
  *b = temp;
}

int choosePivot(int *nums, int nums_len) {
  return nums_len / 2;
}

void partition(int *nums, int nums_len, int *small_pos, int *large_pos) {
  // 选择主元
  int pivot_pos = choosePivot(nums, nums_len);
  int equal_count = 1;
  int last_small = 0;
  
  // 将主元与数组首元素交换位置
  swap(nums + 0, nums + pivot_pos);

  // 遍历数组，划分成三组
  for (int i = 1; i <= nums_len - equal_count; ++i) {
    if (nums[i] < nums[0])
      swap(nums + i, nums + ++last_small);
    else if (nums[i] == nums[0])
      swap(nums + i--, nums + nums_len - equal_count++);
    else;
  }
  swap(nums + 0, nums + last_small);
  swap(nums + last_small, nums + nums_len - equal_count);

  // 获取边界
  *small_pos = last_small - 1;
  *large_pos = nums_len - 1 - equal_count;
}

int findKthLargest(int* nums, int nums_len, int k) {
  int small_pos, large_pos;

  // 对数组进行划分，得到三组的边界
  partition(nums, nums_len, &small_pos, &large_pos);
  int small_len = small_pos + 1;
  int large_len = large_pos - small_pos;
  int equal_len = nums_len - 1 - large_pos;

  if (k <= large_len)
    return findKthLargest(nums + small_len, large_len, k);
  else if (large_len < k && k <= large_len + equal_len)
    return nums[nums_len - 1];
  else
    return findKthLargest(nums, small_len, k - large_len - equal_len);

  // 如果是Kth Smallest
  // if (k <= small_len) {
  //   return findKthLargest(nums, small_len, k);
  // }
  // else if (small_len < k && k <= small_len + equal_len) {
  //   return nums[nums_len - 1];
  // }
  // else {
  //   return findKthLargest(nums + small_len, large_len, k - small_len - equal_len);
  // }
}
```