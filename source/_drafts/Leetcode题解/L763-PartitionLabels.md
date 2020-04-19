---
title: Leetcode763. Partition Labels
tags:
  - 数组
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-20 16:36:58
categories: Leetcode题解
---

给出一个字符串，试着将这个字符串分成最多的组，使字符串中的每一个字符都只出现在一个分组中。

<!-- more -->

---

### 763. Partition Labels

A string `S` of lowercase letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.

**Example 1:**

```
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
```

**Note:**

1. `S` will have length in range `[1, 500]`.
2. `S` will consist of lowercase letters (`'a'` to `'z'`) only.

---

#### 算法：Hash数组记录字符出现的最后位置

我们使用一个长度为26的数组来记录字符串S中每个字符出现的最后位置，例如对于字符串`S = "ababcbacadefegdehijhklij"`a最后出现的位置是8，那么我们在0～8的范围内查询每一个字符出现的最后位置，如果最后位置大于8，则更新分组的长度，对于字符'd'首次出现的位置为9，最后出现的位置为14，在9～14范围内，包含字符'e'，'e'出现的最后位置为15，所以我们该分组的最后位置更新为15，以此类推，直到检查到字符串最后一个字符。

```java
public List<Integer> partitionLabels(String S) {
    List<Integer> res = new ArrayList<>();
    if (S == null || S.length() == 0)
        return res;
    int[] end = new int[26];
    char[] chr = S.toCharArray();
    for (int i = 0; i < chr.length; i++) {
        end[chr[i] - 'a'] = i;
    }
    int start = 0;
    while (start < chr.length) {
        int endPosition = end[chr[start] - 'a'];
        for (int i = start; i <= endPosition; i++) {
            if (end[chr[i] - 'a'] > endPosition)
                endPosition = end[chr[i] - 'a'];
        }
        res.add(endPosition - start + 1);
        start = endPosition + 1;
    }
    return res;
}
```

