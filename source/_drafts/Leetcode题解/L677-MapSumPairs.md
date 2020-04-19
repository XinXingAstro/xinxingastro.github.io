---
title: Leetcode677. Map Sum Pairs
tags:
  - null
  - null
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-20 13:39:33
categories: Leetcode题解
---

根据前缀累加Map中Key带有该前缀的value的值。

<!-- more -->

---

### 677. Map Sum Pairs

Implement a MapSum class with `insert`, and `sum` methods.

For the method `insert`, you'll be given a pair of (string, integer). The string represents the key and the integer represents the value. If the key already existed, then the original key-value pair will be overridden to the new one.

For the method `sum`, you'll be given a string representing the prefix, and you need to return the sum of all the pairs' value whose key starts with the prefix.

**Example 1:**

```
Input: insert("apple", 3), Output: Null
Input: sum("ap"), Output: 3
Input: insert("app", 2), Output: Null
Input: sum("ap"), Output: 5
```

---

#### 算法1：暴力搜索

我们将所有前缀等于prefix的字段的值都加起来，得到sum输出。String中的`startsWith(prefix)`函数可以判断该字符串是否以prefix开头。

```java
class MapSum {
    HashMap<String, Integer> map;
    /** Initialize your data structure here. */
    public MapSum() {
        map = new HashMap();
    }
    public void insert(String key, int val) {
        map.put(key, val);
    }
    public int sum(String prefix) {
        int sum = 0;
        for (String s : map.keySet()) {
            if (s.startsWith(prefix))
                sum += map.get(s);
        }
        return sum;
    }
}
```

时间复杂度：insert：O(1)，sum：O(N * P)，其中N是map中元素的个数，P是前缀的长度。

空间复杂度：O(n)。

#### 算法2：使用前缀HashMap

我们可以构建一个计算前缀和前缀对应分数的HashMap，记录所有可能出现的前缀的对应分数值，求和时直接从前缀HashMap中取就可以。

```java
class MapSum {
    Map<String, Integer> map;
    Map<String, Integer> score;
    public MapSum() {
        map = new HashMap();
        score = new HashMap();
    }
    public void insert(String key, int val) {
        int delta = val - map.getOrDefault(key, 0);
        map.put(key, val);
        String prefix = "";
        for (char c: key.toCharArray()) {
            prefix += c;
            score.put(prefix, score.getOrDefault(prefix, 0) + delta);
        }
    }
    public int sum(String prefix) {
        return score.getOrDefault(prefix, 0);
    }
}
```

时间复杂度：insert：O(K^2)，其中K时key的长度，sum：O(1)。

空间复杂度：O(n)。

#### 算法3：使用字典树

我们可以使用一个字典树来记录所有可能出现的前缀对应的分数是多少。

```java
class MapSum {
    HashMap<String, Integer> map;
    TrieNode root;
    public MapSum() {
        map = new HashMap();
        root = new TrieNode();
    }
    public void insert(String key, int val) {
        int delta = val - map.getOrDefault(key, 0);
        map.put(key, val);
        TrieNode cur = root;
        cur.score += delta;
        for (char c: key.toCharArray()) {
            cur.children.putIfAbsent(c, new TrieNode());
            cur = cur.children.get(c);
            cur.score += delta;
        }
    }
    public int sum(String prefix) {
        TrieNode cur = root;
        for (char c: prefix.toCharArray()) {
            cur = cur.children.get(c);
            if (cur == null) return 0;
        }
        return cur.score;
    }
}
class TrieNode {
    Map<Character, TrieNode> children = new HashMap();
    int score;
}
```

时间复杂度：insert: O(K)其中K是key的长度，sum：O(K)。

空间复杂度：O(n)。