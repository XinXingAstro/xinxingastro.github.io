---
title: Leetcode697. Degree of an Array
tags:
  - HashMap
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-01 18:53:32
categories: Leetcode题解
---

给出一个数组，数组的深度定义为数组中元素最大重复次数，找出一个最小子数组，这个子数组的深度和原数组一致。

<!-- more -->

---

### 697. Degree of an Array

Given a non-empty array of non-negative integers `nums`, the **degree** of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of `nums`, that has the same degree as `nums`.

**Example 1:**

```
Input: [1, 2, 2, 3, 1]
Output: 2
Explanation: 
The input array has a degree of 2 because both elements 1 and 2 appear twice.
Of the subarrays that have the same degree:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
The shortest length is 2. So return 2.
```

**Example 2:**

```
Input: [1,2,2,3,1,4,2]
Output: 6
```

**Note:**

`nums.length` will be between 1 and 50,000.

`nums[i]` will be an integer between 0 and 49,999.

---

#### 算法：用Hash表统计元素出现次数和首末出现位置

我们可以定义三个HashMap：freq first last，其中freq统计各元素出现的次数，first统计各元素首次出现的位置，last统计各元素最后出现的位置。

统计完成后，我们在freq中找出频率最大的元素，频率最大的元素可能不止一个，所以我们要逐个判断频率最大的元素出现的首末位置距离差，最小的距离差就是解。

```java
public int findShortestSubArray(int[] nums) {
    HashMap<Integer, Integer> freq = new HashMap(), first = new HashMap(), last = new HashMap();
    int max = 0, f = 0, res = Integer.MAX_VALUE, dis = 0;
    for (int i = 0; i < nums.length; i++) {
        if (first.get(nums[i]) == null)
            first.put(nums[i], i);
        last.put(nums[i], i);
        freq.put(nums[i], freq.getOrDefault(nums[i], 0) + 1);
        f = freq.get(nums[i]);
        if (f > max) max = f;
    }
    for (Integer i : freq.keySet()) {
        if (freq.get(i) == max) {
            dis = last.get(i) - first.get(i) + 1;
            if (dis < res) res = dis;
        }
    }
    return res;
}
```

上述算法中由于使用了HashMap这种数据结构，所以在数据读取和放入的时候会有较大的时间消耗，我们完全可以使用普通数组来取带HashMap。

用数组来替代HashMap，我们就需要确定数组的长度，由于我们使用数组的下标作为key值，所以key的取值范围应该和原数组中元素的取值范围相同，所以我们需要遍历原数组，找到数组中最大的元素max，Hash数组的长度len = max + 1。

```java
public int findShortestSubArray(int[] nums) {
    int max = Integer.MIN_VALUE;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] > max) max = nums[i];
    }
    int[] freq = new int[max + 1];
    int[] first = new int[max + 1];
    int[] last = new int[max + 1];
    max = Integer.MIN_VALUE;
    for (int i = 0; i < nums.length; i++) {
        if (freq[nums[i]] == 0) {
            first[nums[i]] = i;
            last[nums[i]] = i;
        } else 
            last[nums[i]] = i;
        freq[nums[i]]++;
        if (freq[nums[i]] > max) max = freq[nums[i]];
    }
    int res = Integer.MAX_VALUE;
    for (int i = 0; i < nums.length; i++) {
        if (freq[nums[i]] == max)
            res = Math.min(res, last[nums[i]] - first[nums[i]] + 1);
    }
    return res;
}
```

时间复杂度：O(n)

空间复杂度：O(n)