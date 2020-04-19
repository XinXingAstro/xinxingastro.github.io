---
title: Leetcode260. Single Number III
tags:
  - 数组
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-22 10:56:09
categories: Leetcode题解
---

数组中只有两个数字出现1次，其他数字都出现2次，找出这两个出现1次的数字。

<!-- more -->

---

### 260. Single Number III

Given an array of numbers `nums`, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

**Example:**

```
Input:  [1,2,1,3,2,5]
Output: [3,5]
```

**Note**:

1. The order of the result is not important. So in the above example, `[5, 3]` is also correct.
2. Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

---

#### 算法：逐个异或+子数组划分

先将数组中所有元素进行异或，得到的结果实际上是那两个只出现1次的2个元素的异或，用这个异或结果的第一位不为零的位对子数组进行划分，将划分后的数组在逐个异或，得到的两个元素就是解。

```java
public int[] singleNumber(int[] nums) {
    int xor = 0;
    for (int i = 0; i < nums.length; i++) {
        xor ^= nums[i];
    }
    int div = 1;
    for (int i = 0; i < 32; i++) {
        if ((div & xor) != 0) 
            break;
        div <<= 1;
    }
    int a = 0, b = 0;
    for (int i = 0; i < nums.length; i++) {
        if ((div & nums[i]) != 0)
            a ^= nums[i];
        else
            b ^= nums[i];
    }
    return new int[]{a, b};
}
```

时间复杂度：O(n)

空间复杂度：O(1)