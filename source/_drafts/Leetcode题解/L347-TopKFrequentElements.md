---
title: Leetcode347. Top K Frequent Elements
tags:
  - 桶排序
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-13 09:54:11
categories: Leetcode题解
---

给出一个数组，找出数组中出现频率最高的k个数。

<!-- more -->

---

#### 347. Top K Frequent Elements

Given a non-empty array of integers, return the **k** most frequent elements.

For example,
Given `[1,1,1,2,2,3]` and k = 2, return `[1,2]`.

**Note:** 

- You may assume *k* is always valid, 1 ≤ *k* ≤ number of unique elements.
- Your algorithm's time complexity **must be** better than O(*n* log *n*), where *n* is the array's size.

---

#### 算法：桶排序

桶排序分为以下几步：

1）用一个map统计数组中各数字出现的频率。

2）创建一个桶数组（List数组），数组中每个元素都是一个表，数组中元素的下标表示该表中元素出现的频率。遍历map将map中的元素放到对应的表中。

3）由于桶数组的下表代表元素出现的频率，所以我们从后向前遍历桶的每个元素，找到下标最大的k个元素中的数字就是解。

```java
public List<Integer> topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int n : nums) {
        map.put(n, map.getOrDefault(n, 0) + 1);
    }
    List<Integer>[] bucket = new List[nums.length + 1];
    for (Integer i : map.keySet()) {
        int freq = map.get(i);
        if (bucket[freq] == null)
            bucket[freq] = new ArrayList<>();
        bucket[freq].add(i);
    }
    List<Integer> res = new ArrayList<>();
    for (int i = bucket.length - 1; i >= 0 && res.size() < k; i--) {
        if (bucket[i] != null)
            res.addAll(bucket[i]);
    }
    return res;
}
```

时间复杂度：O(n)

空间复杂度：O(n)