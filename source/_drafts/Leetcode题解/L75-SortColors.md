---
title: Leetcode75. Sort Colors
tags:
  - 排序
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-09 11:55:52
categories: Leetcode题解
---

对只包含0，1，2三个数的数组进行排序。

<!-- more -->

---

### 75. Sort Colors

Given an array with *n* objects colored red, white or blue, sort them **in-place** so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

**Note:** You are not suppose to use the library's sort function for this problem.

**Example:**

```
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

**Follow up:**

- A rather straight forward solution is a two-pass algorithm using counting sort.
  First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
- Could you come up with a one-pass algorithm using only constant space?

---

#### 算法1: 三向切分快速排序

由于题目要求最好使用一次遍历就将数组排序，由于数组中只存在0，1，2三个数字，所以我们可以使用快速排序的优化版本：三向切分快速排序。

三向切分快速排序的基本原理就是将大于目标元素的数组放到数组末尾，将小与目标元素的数字放到数组开头，这里我们的目标元素就是1，我们需要将0放到数组开头，将2放到数组末尾。

这个算法的难点是对三个指针的定义和操作，我们定义如下：

1）lt指针指向最后一个小与1的元素，初始值为-1

2）i指针指向第一个未知大小的元素，初始值为0

3）gt指针指向左后一个大于1的元素，初始值为nums.length

这里有一点需要特别注意，如果lt和i指向同一点，则会导致i进入lt的区域，程序出现错误，所以我们每次移动lt指针之后，都要判断lt是否等于i。

```java
public void sortColors(int[] nums) {
    if (nums == null || nums.length == 0) return;
    int lt = -1;
    int gt = nums.length;
    int i = 0;
    while (i < gt) {
        if (nums[i] == 0) {
            lt++;
            swap(nums, i, lt);
            // 防止i指针进入lt的左边
            if (lt == i) i++; 
        } else if (nums[i] == 2) {
            gt--;
            swap(nums, i, gt);
        } else i++;
    }
}
public void swap(int[] nums, int a, int b) {
    if (a == b) return;
    int tmp = nums[a];
    nums[a] = nums[b];
    nums[b] = tmp;
}
```

