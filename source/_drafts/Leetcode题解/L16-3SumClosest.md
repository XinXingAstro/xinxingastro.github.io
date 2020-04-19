---
title: Leetcode16. 3Sum Closest
tags:
  - 数组
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-10 17:42:53
categories: Leetcode题解
---

找到离目标最近的三个数的和。

<!-- more -->

---

### 16. 3Sum Closest

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example:**

```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

---

#### 算法1: 暴力搜索

枚举出数组中所有三个数的组合，在这些组合中找到和离目标最近的那个。

```java
public int threeSumClosest(int[] nums, int target) {
    int min = Integer.MAX_VALUE;
    int dif, sum;
    int ans = nums[0] + nums[1] + nums[2];
    for (int a = 0; a < nums.length - 2; a++)
        for (int b = a + 1; b < nums.length - 1; b++)
            for (int c = b + 1; c < nums.length; c++) {
                sum = nums[a] + nums[b] + nums[c];
                dif = Math.abs(sum - target);
                if (dif < min) {
                    min = dif;
                    ans = sum;
                }
            }
    return ans;
}
```

时间复杂度：O(n^3)

空间复杂度：O(1)

#### 算法2: 两边夹

```java
public int threeSumClosest(int[] nums, int target) {
    int n = nums.length;
    Arrays.sort(nums);
    int less = nums[0] + nums[1] + nums[2];
    int more = nums[n-3] + nums[n-2] + nums[n-1];
    if (less >= target)
        return less;
    if (more <= target)
        return more;
    for (int i = 0; i < n - 2; i++){
        int min = nums[i] + nums[i+1] + nums[i+2];
        int max = nums[i] + nums[n-2] + nums[n-1];
        if (min > target){
            more = Math.min(more, min);
            break;
        }
        if (max < target){
            less = Math.max(less, max);
            continue;
        }
        int j = i + 1, k = n - 1;
        while(j < k){
            int sum = nums[i] + nums[j] + nums[k];
            if(sum == target) return target;
            else if(sum > target){
                more = Math.min(more, sum);
                while(--k > j && nums[k]==nums[k+1]);
            }else{
                less = Math.max(less, sum);
                while(++j < k && nums[j]==nums[j-1]);
            }
        }
    }
    return more - target > target - less ? less : more;
}
```

