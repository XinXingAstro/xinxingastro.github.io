---
title: Leetcode78. Subsets
tags:
  - 数组
  - 全组合
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-09 13:25:56
categories: Leetcode题解
---

输出一个数组的所有子集，既全组合。

<!-- more -->

---

### 78. Subsets

Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

**Example:**

```
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

---

#### 算法：全组合

输出一个数组的所有子集是一个非常基本的算法（既全组合），重点是理解子集生成的过程：

```
nums:[1,2,3]
//首先我们在结果集中加入一个空集
[]
//然后我们取nums中第一个数字并和这个空集合并，得到一个新的数组[1]，然后我们将[1]加入到结果集
[] [1]
//我们取nums中第二个数字，然后和结果集中所有数组合并，得到两个新数组[2] [1,2]，然后将新数组加入结果集
[] [1] [2] [1,2]
//同理我们取nums中第三个数字，然后和结果集中所有数组合并，将得到的新数组加入结果集。。。。
[] [1] [2] [1,2] [3] [1,3] [2,3] [1,2,3]
//循环执行上面操作，直到取到nums的最后一个数字为止。
```

算法中有一点很重要，就是在遍历结果集的时候，由于会有新的数组加入到结果集中，所以结果集的长度应该在遍历之前事先取定，然后再遍历。

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums.length == 0) return res;
    res.add(new ArrayList<>());
    for (int n : nums) {
        int size = res.size();
        for (int i = 0; i < size; i++) {
            ArrayList<Integer> tmp = new ArrayList<>(res.get(i));
            tmp.add(n);
            res.add(tmp);
        }
    }
    return res;
}
```

时间复杂度：O(2^n)；其中n是nums的长度。

空间复杂度：O(1)