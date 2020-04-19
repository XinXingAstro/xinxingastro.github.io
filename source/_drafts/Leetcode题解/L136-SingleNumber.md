---
title: Leetcode136. Single Number
tags:
  - 数组
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-09 15:22:25
categories: Leetcode题解
---

数组中其他数字出现两次，只有一个数字出现一次，找到出现一次的那个数字。

<!-- more -->

---

### 136. Single Number

Given a **non-empty** array of integers, every element appears *twice* except for one. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```
Input: [2,2,1]
Output: 1
```

**Example 2:**

```
Input: [4,1,2,1,2]
Output: 4
```

---

#### 算法：逐个异或

对一个相同的数字异或两次，相当于没有进行操作，我们只要逐个异或数组中的每个数字，最终得到的就是只出现一次的那个数字。

```java
public int singleNumber(int[] nums) {
    int ans = nums[0];
    for (int i = 1; i < nums.length; i++) {
        ans ^= nums[i];
    }
    return ans;
}
```

