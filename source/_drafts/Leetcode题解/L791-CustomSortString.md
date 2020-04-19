---
title: Leetcode791. Custom Sort String
tags:
  - 计数排序
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-20 22:07:51
categories: Leetcode题解
---

给出两个字符串S T，S代表字符的顺序，既T中如果出现了S中的字符，这些字符必须按照S中的顺序排列。

<!-- more -->

---

### 791. Custom Sort String

`S` and `T` are strings composed of lowercase letters. In `S`, no letter occurs more than once.

`S` was sorted in some custom order previously. We want to permute the characters of `T` so that they match the order that `S` was sorted. More specifically, if `x` occurs before `y` in `S`, then `x` should occur before `y` in the returned string.

Return any permutation of `T` (as a string) that satisfies this property.

```
Example :
Input: 
S = "cba"
T = "abcd"
Output: "cbad"
Explanation: 
"a", "b", "c" appear in S, so the order of "a", "b", "c" should be "c", "b", and "a". 
Since "d" does not appear in S, it can be at any position in T. "dcba", "cdba", "cbda" are also valid outputs.
```

 

**Note:**

- `S` has length at most `26`, and no character is repeated in `S`.
- `T` has length at most `200`.
- `S` and `T` consist of lowercase letters only.

---

#### 算法：计数排序

S中的字符代表顺序，那么我们将S中字符出现的顺序量化为优先级，第一个字符为1，第二个为2，以此类推，没有在S中出现的字符优先级为0。那么我们就可以将T中的每个字符都映射为一个优先级，将相应优先级的字符放到对应的位置上就可以了，那么这实际上是一个排序问题，而且排序的元素在一个非常小的范围内，这个范围是：`[0, S.length()]`，而且数据中存在很多重复元素，那么这种情况我们通常选择**计数排序**算法对数据进行排序。

```java
public String customSortString(String S, String T) {
    // 用map统计S中各字符的优先级，字符越靠后优先级越高
    // 优先级从1开始，不在S中的字符优先级为0
    int[] map = new int[26];
    char[] s = S.toCharArray();
    for (int i = 0; i < s.length; i++) {
        map[s[i] - 'a'] = i + 1;
    }
    // 我们将T中的每个字符映射为优先级，那么我们就相当于对这几个优先级数字进行排序
    // 我们使用计数排序算法可以达到O(n)时间复杂度
    // count数组中记录各个优先级出现频次
    int[] count = new int[s.length + 1];
    char[] t = T.toCharArray();
    for (int i = 0; i < t.length; i++) {
        count[map[t[i] - 'a']]++;
    }
    // 累加优先级出现的频次，对应各个优先级在结果数组中出现的起始位置
    for (int i = 1; i < count.length; i++) {
        count[i] += count[i - 1];
    }
    // 通过t中每个字符找到对应的优先级，然后在count数组中找到对应位置
    char[] res = new char[t.length];
    for (int i = 0; i < t.length; i++) {
        res[count[map[t[i] - 'a']] - 1] = t[i]; // 由于频次最低为1，所以在映射时要注意索引-1
        count[map[t[i] - 'a']]--;
    }
    return String.valueOf(res);
}
```

时间复杂度：O(n)

空间复杂度：O(n)