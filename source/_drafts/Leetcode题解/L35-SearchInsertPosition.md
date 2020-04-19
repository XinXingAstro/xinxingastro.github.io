---
title: Leetcode35. Search Insert Position
tags:
  - 二分查找
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-07 16:48:47
categories: Leetcode题解
---

找到数组的位置或插入位置。

<!-- more -->

---

### 35. Search Insert Position

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example 1:**

```
Input: [1,3,5,6], 5
Output: 2
```

**Example 2:**

```
Input: [1,3,5,6], 2
Output: 1
```

**Example 3:**

```
Input: [1,3,5,6], 7
Output: 4
```

**Example 4:**

```
Input: [1,3,5,6], 0
Output: 0
```

---

#### 算法：二分查找算法

二分查找算法的应用

```java
public int searchInsert(int[] nums, int target) {
    if (nums == null) return 0;
    if (target < nums[0]) return 0;
    if (target > nums[nums.length - 1]) return nums.length;
    int start = 0;
    int end = nums.length - 1;
    while (start <= end) {
        int mid = (start + end) >> 1;
        if (target > nums[mid]) start = mid + 1;
        else if (target < nums[mid]) end = mid - 1;
        else return mid;
    }
    return start;
}
```

时间复杂度：O(logn)

空间复杂度：O(1)