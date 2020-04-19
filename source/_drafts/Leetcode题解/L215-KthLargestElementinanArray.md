---
title: Leetcode215. Kth Largest Element in an Array
tags:
  - 分治法
  - 快速排序
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-13 17:16:27
categories: Leetcode题解
---

在一个无序数组中找出第k大的元素。

<!-- more -->

---

### 215. Kth Largest Element in an Array

Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Example 1:**

```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

**Example 2:**

```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

**Note:** 
You may assume k is always valid, 1 ≤ k ≤ array's length.

---

#### 算法：使用快速排序中的partition函数

快速排序中的partition函数每次可以找到一个元素的最终位置，并且该元素前面都是比它小的元素，后面都是比它大的元素，我们只要找到数组倒数第k的元素的最终位置就可以。

```java
public int findKthLargest(int[] nums, int k) {
    if (nums.length == 1)
        return nums[0];
    int target = nums.length - k;
    int left = 0, right = nums.length - 1;
    int index = partition(nums, left, right);
    while (index != target) {
        if (target > index) {
            left = index + 1;
            index = partition(nums, left, right);
        } else {
            right = index - 1;
            index = partition(nums, left, right);
        }
    }
    return nums[index];
}
public int partition(int[] nums, int start, int end) {
    if (start >= end)
        return start;
    // int mid = (start + end) >> 1;
    int mid = start + (int)(Math.random() * (end - start));
    swap(nums, mid, end);
    int x = nums[end];
    int small = start - 1;
    for (int i = start; i <= end - 1; i++) {
        if (nums[i] < x){
            swap(nums, i, ++small);
        }
    }
    swap(nums, ++small, end);
    return small;
}
public void swap(int[] nums, int a, int b) {
    if (a == b) return;
    int tmp = nums[a];
    nums[a] = nums[b];
    nums[b] = tmp;
}
```

在paitition函数中如果使用随机化算法可以使算法的性能得到很大程度的提升，实测：

1）如果直接使用区间最后一个元素作为基准元素进行划分，Leetcode运行时间为：28ms。

2）如果取区间中间的数字，或者使用随机化算法在区间中随机取一个元素作为基准元素，则Leecode的运行时间为：5ms。

时间复杂度：O(nlogn)

空间复杂度：O(1)