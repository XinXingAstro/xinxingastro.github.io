---
title: Leetcode442. Find All Duplicates in an Array
tags:
  - 数组
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-20 23:35:56
categories: Leetcode题解
---

找出数组中出现两次的数字。

相关题目：剑指Offer[面试题3: 数组中重复的数字](https://xinxingastro.github.io/2018/05/03/剑指Offer/面试题3-数组中重复的数字/)。

<!-- more -->

---

### 442. Find All Duplicates in an Array

Given an array of integers, 1 ≤ a[i] ≤ *n* (*n* = size of array), some elements appear **twice** and others appear **once**.

Find all the elements that appear **twice** in this array.

Could you do it without extra space and in O(*n*) runtime?

**Example:**

```
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
```

---

#### 算法1：将相应数字交换到相应位置(Time Limit Exceeded)

由题设条件可以看出，该题应该使用[面试题3: 数组中重复的数字](https://xinxingastro.github.io/2018/05/03/剑指Offer/面试题3-数组中重复的数字/)算法2。

由于数组中的元素的范围是`1 <= a[i] <= n`所以我们每次讲一个数字交换到数字相应位置，如果发现重复元素则将其放入结果列表。

```java
public List<Integer> findDuplicates(int[] nums) {
    List<Integer> res = new ArrayList<>();
    for (int i = 0; i < nums.length; i++) {
        if ((nums[i] - 1) != i && !res.contains(nums[i])) {
            if (nums[nums[i] - 1] == nums[i] && !res.contains(nums[i]))
                res.add(nums[i]);
            else {
                // swap nums[i] nums[nums[i] - 1]
                int tmp = nums[i];
                nums[i] = nums[tmp - 1];
                nums[tmp - 1] = tmp;
                i--;
            }
        }
    }
    return res;
}
```

这个算法超时的原因是：我们每次将指针下的数字交换到指定位置后，指针下将出现一个新的数字，我们需要继续交换这个新的数字，这样指针就很难向前移动，最后导致了超时。我们可以对该算法做进一步改进，提升它的时间性能。

#### 算法2: 每次将指定位置标记为负数

对于数组中的每个元素我们知道它的最终位置，我们不进行交换，而是将这个位置的数字乘以-1（初始情况下数组中所有元素都是正数），当我们下次访问一个位置，这个位置上的数字是负数的时候，我们就找到了一个重复数字，这样对于数组中的每个数字，我们仅需要访问它一次就可以判断是否是重复数字。

```java
public List<Integer> findDuplicates(int[] nums) {
    List<Integer> res = new ArrayList<>();
    for (int i = 0; i < nums.length; i++) {
        int index = Math.abs(nums[i]) - 1;
        if (nums[index] < 0) 
            res.add(Math.abs(nums[i]));
        else 
            nums[index] = -nums[index];
    }
    return res;
}
```

时间复杂度：O(n)

空间复杂度：O(1)