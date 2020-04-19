---
title: Leetcode53. Maximum Subarray
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-08 10:44:18
categories: Leetcode题解
---

求连续子数组的最大和。相关题目：剑指Offer面试题42: 连续子数组的最大和、编程之美2.14: 求数组的子数组之和的最大值、编程之美2.15: 子数组之和的最大值（二维）

<!-- more -->

### 53. Maximum Subarray

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array `[-2,1,-3,4,-1,2,1,-5,4]`,
the contiguous subarray `[4,-1,2,1]` has the largest sum = `6`.

More practice:

If you have figured out the O(*n*) solution, try coding another solution using the divide and conquer approach, which is more subtle.



#### 算法1：动态规划

**设 `sum(i)` 表示已 `nums[i]` 结尾的子数组的最大和。**则可得：

1. `sum(i) = nums[i]                 sum(i - 1) <= 0 或 i = 0`
2. `sum(i) = sum(i - 1) + nums[i]    sum(i - 1) > 0 且 i != 0`

剑指offer面试题42。逐个累加数组中元素，如果出现负数则抛弃前面的数，从当前数开始计算。

```java
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    int size = nums.length;
    int sum = 0;
    int maxSum = Integer.MIN_VALUE;
    for (int i = 0; i < size; i++) {
        sum += nums[i];
        if (sum > maxSum) maxSum = sum;
        if (sum < 0) sum = 0;
    }
    return maxSum;
}
```

