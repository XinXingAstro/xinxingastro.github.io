---
title: Leetcode39. Combination Sum
tags:
  - 回溯法
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-07 18:19:16
categories: Leetcode题解
---

给出一个target和一个数组，找出所有的组合，每个组合中元素的和等于target，组合内的元素都是数组中的元素，数组中每个元素都可以重复选择。

<!-- more -->

---

### 39. Combination Sum

Given a **set** of candidate numbers (`candidates`) **(without duplicates)** and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

---

#### 算法：回溯法

这是一个典型的用回溯法求解的题，回溯法大多用递归实现。

我们可以将target想象成一个盒子，数组中的元素想象成各种长度一定的积木，我们要找的答案就是所有正好装满这个盒子的所有装法。我们先从最小的元素开始实验，将最小的积木放到盒子中，知道盒子正好装满或者装不下下一块最小积木，然后我们逐个取出盒子中的积木，换用更长一点的积木实验，在实验过程中记录所有正好装满盒子的组合，就是最终解。

回溯算法的回溯过程就是我们取出较小的积木换用长一点的积木再次实验的过程，这个过程需要保存之前的实验状态，所以我们通常使用递归来实现。

算法分为一下几步：

1）先将数组内元素从小到大排序，然后从小元素开始，逐个加到一个数组中，直到数组中元素的和大于或者等于target，如果元素和正好等于target我们将这个组合加到一个结果集合。

2）然后我们逐个减去数组中最后面的元素，直到容器能放下更大的元素时再放入，并判断容器是否刚好装满。

3）重复上述过程，直到所有元素都实验过一次。

算法的难点是在循环的过程中进行递归调用。

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    if (candidates == null) return new ArrayList<>();
    List<List<Integer>> ans = new ArrayList<>();
    Arrays.sort(candidates); //将数组中元素从小到大排序
    helper(candidates, target, 0, ans, new ArrayList<Integer>());
    return ans;
}
public void helper(int[] candidates, int target, int start, List<List<Integer>> ans, ArrayList<Integer> cur) {
    if (target == 0) ans.add(new ArrayList<>(cur));
    else if (target > 0) {
        for (int i = start; i <= candidates.length - 1 && target >= candidates[i]; i++) {
            cur.add(candidates[i]);
            helper(candidates, target - candidates[i], i, ans, cur);
            cur.remove(cur.size() - 1);
        }
    }
}
```

