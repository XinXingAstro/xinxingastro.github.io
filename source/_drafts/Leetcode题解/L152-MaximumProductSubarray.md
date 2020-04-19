---
title: Leetcode152. Maximum Product Subarray
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-12 23:55:17
categories: Leetcode题解
---

求连续子数组的最大乘积。相关题目：编程之美2.13子数组的最大乘积。

<!-- more -->

### 152. Maximum Product Subarray

Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array `[2,3,-2,4]`,
the contiguous subarray `[2,3]` has the largest product = `6`.



#### 算法1: 动态规划

**设max(i)表示以i结尾的数组的最大累乘积, min(i)表示以i结尾的最小累乘积**。则有：

1. max(i) = Math.max(max(i-1)\*nums[i], min(i-1)\*nums[i], nums[i]);
2. min(i) = Math.min(max(i-1)\*nums[i], min(i-1)\*nums[i], nums[i]);

```java
public int maxProduct(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    int max = nums[0];
    int min = nums[0];
    int res = nums[0];
    int a = 0;
    int b = 0;
    for (int i = 1; i < nums.length; i++) {
        a = max * nums[i];
        b = min * nums[i];
        max = Math.max(Math.max(a, b), nums[i]);
        min = Math.min(Math.min(a, b), nums[i]);
        res = Math.max(res, max);
    }
    return res;
}
```



#### 算法2: 前后两遍遍历找到最大累乘积

该算法分两步：

1. 从前到后遍历累乘所有元素，遇到0元素让累乘积为1，然后继续向后累乘，在此过程中找到最大累乘积。
2. 从后到前重复同样过程。

最后找打的最大累乘积就是整个数组的最大累乘积。

```java
public int maxProduct(int[] nums) {
    int n = nums.length;
    if (n == 0) return 0;
    if (n == 1) return nums[0];
    int max = Integer.MIN_VALUE;
    int product = 1;
    for (int i = 0; i < n; i++) {
        max = Math.max(max, product *= nums[i]);
        if (nums[i] == 0) product = 1;
    }
    product = 1;
    for (int i = n-1; i >= 0; i--) {
        max = Math.max(max, product *= nums[i]);
        if (nums[i] == 0) product = 1;
    }
    return max;
}
```

