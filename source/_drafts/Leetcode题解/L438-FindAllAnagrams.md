---
title: Leetcode438. Find All Anagrams in a String
tags:
  - 字符串
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-12 15:50:22
categories: Leetcode题解
---

找到一个字符串s中所有p的同字母异序词。

<!-- more -->

---

### 438. Find All Anagrams in a String

Given a string **s** and a **non-empty** string **p**, find all the start indices of **p**'s anagrams in **s**.

Strings consists of lowercase English letters only and the length of both strings **s** and **p** will not be larger than 20,100.

The order of output does not matter.

**Example 1:**

```
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

---

####  算法1: 滑动窗口法

使用HashMap保存p中字符出现的次数，然后使用滑动窗口判断s中是否存在p的同字母异序词。其中滑动窗口算法是固定写法。

```java
public List<Integer> findAnagrams(String s, String t) {
    List<Integer> result = new LinkedList<>();
    if(t.length()> s.length()) return result;
    Map<Character, Integer> map = new HashMap<>();
    
    for(char c : t.toCharArray()){
        map.put(c, map.getOrDefault(c, 0) + 1);
    }
    
    int counter = map.size();
    int begin = 0, end = 0;
    while(end < s.length()){
        char c = s.charAt(end);
        if( map.containsKey(c) ){
            map.put(c, map.get(c)-1);
            if(map.get(c) == 0) counter--;
        }
        end++;

        while(counter == 0){
            if(end-begin == t.length()){
                result.add(begin);
            }
            char tempc = s.charAt(begin);
            if(map.containsKey(tempc)){
                map.put(tempc, map.get(tempc) + 1);
                if(map.get(tempc) > 0){
                    counter++;
                }
            }
            begin++;
        }
    }
    return result;
}
```

由于两个字符串中都只有字母，所以我们可以用一个长度为256的整型数组来代替HashMap来优化空间和时间性能。

```java
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> res = new ArrayList<>();
    int[] map = new int[256];
    for (char c : p.toCharArray())
        map[c]++;

    int count = p.length();
    int start = 0;
    for (int end = 0; end < s.length(); end++) {
        if (map[s.charAt(end)]-- > 0){
            count--;
        }

        while (count == 0) {
            if (end - start + 1 == p.length())
                res.add(start);
            if (++map[s.charAt(start++)] > 0)
                count++;
        }
    }
    return res;
}
```

