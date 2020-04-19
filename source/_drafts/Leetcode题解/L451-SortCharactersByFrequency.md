---
title: Leetcode451. Sort Characters By Frequency
tags:
  - 排序
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-25 15:42:23
categories: Leetcode题解
---

给出一个字符串，按照字符串中各字符出现的频率降序排序，相同字符必须挨在一起。

<!-- more -->

---

### 451. Sort Characters By Frequency

Given a string, sort it in decreasing order based on the frequency of characters.

**Example 1:**

```
Input:
"tree"

Output:
"eert"

Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
```

**Example 2:**

```
Input:
"cccaaa"

Output:
"cccaaa"

Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.
```

**Example 3:**

```
Input:
"Aabb"

Output:
"bbAa"

Explanation:
"bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.
```

---

#### 算法1: 计数排序

这道题我最先想到的算法是计数排序，由于每个数的频率最大不超过字符串的长度，所以可以将每个字符映射成一个频率，用计数排序将频率降序排序。

```java
public String frequencySort1(String s) {
    char[] chr = s.toCharArray();
    Arrays.sort(chr);
    int[] freq = new int[256];
    for (int i = 0; i < chr.length; i++) {
        freq[chr[i]]++;
    }
    int[] count = new int[chr.length + 1];
    for (int i = 0; i < chr.length; i++) {
        count[freq[chr[i]]]++;
    }
    for (int i = 1; i < count.length; i++) {
        count[i] += count[i - 1];
    }
    char[] res = new char[chr.length];
    for (int i = 0; i < chr.length; i++) {
        res[res.length - count[freq[chr[i]]]] = chr[i];
        count[freq[chr[i]]]--;
    }
    return String.valueOf(res);
}
```

由于题目中要求相同字符在排完序后必须挨到一起，但是计数排序时相同字符的顺序跟原字符串中的顺序一致，为了保证相同字符能挨到一起，我们就必须在计数排序前对字符串先排序，这样就降低了整个排序算法的时间复杂度，如果题目没有这个要求，计数排序应该是一个很快的算法。

时间复杂度：O(nlogn)：这里主要是前面排序算法的时间复杂度。

空间复杂度：O(n)

#### 算法2: Hash数组记录频率

和算法1一样我们仍然使用一个大小为256的数组统计字符串中各个字符的频率，然后排序时，我们每次从频率数组中选出一个最大的频率max，找到该频率对应的字符c，然后将频率数组中最大频率置零，在结果数组中放入max个c字符，重复上述步骤，我们就可以得到解。

```java
public String frequencySort(String s) {
    int[] freq = new int[256];
    char[] chr = s.toCharArray();
    for (int i = 0; i < chr.length; i++) {
        freq[chr[i]]++;
    }
    int i = 0;
    char[] res = new char[chr.length];
    while (i < chr.length) {
        int max = 0;
        char c = 0;
        for (int j = 0; j < 256; j++) {
            if (max < freq[j]){
                max = freq[j];
                c = (char)j;
            }
        }
        freq[c] = 0;
        while (max-- > 0) {
            res[i++] = c;
        }
    }
    return String.valueOf(res);
}
```

时间复杂度：O(n)

空间复杂度：O(n)