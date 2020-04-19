---
title: Leetcode300. Longest Increasing Subsequence
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-15 19:32:59
categories: Leetcode题解
---

给一个非排序的整数矩阵，找到最长递增(非连续)子数组。

相关题目：编程之美2.16 求数组中最长递增子序列。面试指南：最长递增子序列。

<!-- more -->

### 300. Longest Increasing Subsequence

Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,
Given `[10, 9, 2, 5, 3, 7, 101, 18]`,
The longest increasing subsequence is `[2, 3, 7, 101]`, therefore the length is `4`. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

Your algorithm should run in O(n^2) complexity.

**Follow up:** Could you improve it to O(*n* log *n*) time complexity? 

**Credits:**
Special thanks to [@pbrother](https://leetcode.com/discuss/user/pbrother) for adding this problem and creating all test cases.



### 算法1: 动态规划

**设dp[i]表示以i元素结尾的最长递增子数组的最大长度。**每次更新dp[i]的时候，都需要遍历前面0～i-1个元素，在比nums[i]小的元素里面找出dp值最大的那个，dp[i]就为那个数加1.

**dp[i] = max(dp[j]) + 1, 0 <= j < i**

数组最长递增子数组的长度就是dp数组中的的最大值。

时间复杂度：O(n^2)

空间复杂度：O(n)

```java
public int lengthOfLIS(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    int[] dp = new int[nums.length];
    dp[0] = 1;
    int maxans = 1;
    for (int i = 1; i < dp.length; i++) {
        int maxval = 0;
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                maxval = Math.max(maxval, dp[j]);
            }
        }
        dp[i] = maxval + 1;
        maxans = Math.max(maxans, dp[i]);
    }
    return maxans;
}
```



### 算法2: 动态规划+二分搜索

在这个算法里，dp数组里存的不再是最长递增子数组的长度，而是从原数组里选出来的一个递增子数组。

我们从头遍历原数组nums[]，对于nums[i]，我们在dp[]数组里用二分查找函数查找该数的位置，如果dp数组中不包含nums[i]，二分查找函数会返回nums[i]应该插入dp[]中的位置，我们直接用nums[i]替换这个位置上的数，遍历nums[i]一遍以后，dp[]数组中存的不是nums[]的最长递增子数组，但是dp[]的长度和nums[]最长递增子数组的长度一样。

直觉：在遍历nums[]的时候，只有找到比前面所有元素都大的元素，dp[]的长度才会增长，否则只会替换dp[]中间的元素，所以dp[]数组的长度体现了nums[]数组的增长性。

```java
public int lengthOfLIS(int[] nums) {
    int[] dp = new int[nums.length];
    int len = 0;
    for (int num : nums) {
        int i = Arrays.binarySearch(dp, 0, len, num);
        if (i < 0) {
            i = -(i + 1);
        }
        dp[i] = num;
        if (i == len) {
            len++;
        }
    }
    return len;
}
```

时间复杂度：O(nlogn)（二分查找算法时间复杂度O(logn), 调用了n次）

空间复杂度：O(n)

