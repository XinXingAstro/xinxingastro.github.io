---
title: Leetcode540. Single Element in a Sorted Array
tags:
  - 二分查找
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-21 16:04:17
categories: Leetcode题解
---

给出一个排序数组，数组中只有一个元素出现1次，其他元素都出现2次，找到只出现一次的元素。

<!-- more -->

---

### 540. Single Element in a Sorted Array

Given a sorted array consisting of only integers where every element appears twice except for one element which appears once. Find this single element that appears only once. 

**Example 1:**

```
Input: [1,1,2,3,3,4,4,8,8]
Output: 2
```

**Example 2:**

```
Input: [3,3,7,7,10,11,11]
Output: 10
```

**Note:** Your solution should run in O(log n) time and O(1) space.

---

#### 算法：二分查找

根据题设条件，我们应该使用二分查找算法，对于在数组中找出重复元素的其他算法见[数组中只出现1次的数字](https://xinxingastro.github.io/2018/04/26/算法与数据结构/数组中只出现1次的数字/)。

迭代实现：

```java
public int singleNonDuplicate(int[] nums) {
    int start = 0, end = nums.length - 1;
    while(start < end) {
        int mid = (start + end) >> 1;
        if ((mid & 1) == 1) mid--;
        if (nums[mid] != nums[mid + 1])
            end = mid; // 这里end设为mid是防止mid正好指向出现一次的元素的情况
        else
            start = mid + 2;
    }
    return nums[start];
}
```

递归实现：

```java
public int singleNonDuplicate(int[] nums) {
    return binarySearch(nums, 0, nums.length - 1);
}
public int binarySearch(int[] nums, int start, int end) {
    if (start >= end)
        return nums[start];

    int mid = (start + end) >> 1;
    if ((mid & 1) == 1) mid--;
    if (nums[mid] != nums[mid + 1])
        // 这里end设为mid是防止mid正好指向出现一次的元素的情况
        return binarySearch(nums, start, mid); 
    else
        return binarySearch(nums, mid + 2, end);

}
```

时间复杂度：O(n)

空间复杂度：O(1)