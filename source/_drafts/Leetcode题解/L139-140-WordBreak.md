---
title: Leetcode139/140. Word Break I II
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-12 14:05:22
categories: Leetcode题解
---

139: 检查一个字符串是否能被拆分成字典中的字。

<!-- more -->

---

### 139. Word Break

Given a **non-empty** string *s* and a dictionary *wordDict* containing a list of **non-empty** words, determine if *s* can be segmented into a space-separated sequence of one or more dictionary words.

**Note:**

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.

**Example 1:**

```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**

```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

 

#### 算法：动态规划

<font color=red>**设dp(i)表示以第i个字符结尾(不包含第i个字符)的子字符串是否能拆分成字典中的单词**</font>

1. 如果`dp(i-k) == true (k = i - j)`即上一个子串可以在字典中找到，并且`s.substring(j, i)`即当前子串也可以在字典中找到，则`dp(i) = true`.
2. 其他情况`dp(i) = false`.

**边界条件：**

1. 扫描子串的最大长度不应该超过字典里字的最大长度。

```java
public boolean wordBreak(String s, List<String> wordDict) {
    int maxWordLen = 0;
    Set<String> dict = new HashSet<>(); //这里用HashSet可以提高检索速度
    for (String word : wordDict) {
        dict.add(word);
        maxWordLen = Math.max(maxWordLen, word.length());
    }
    boolean[] dp = new boolean[s.length() + 1];
    dp[0] = true;
    for (int i = 1; i <= s.length(); i++) {
        for (int j = i - 1; j >= 0 && i - j <= maxWordLen; j--) {
            if (dp[j] && dict.contains(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }        
    return dp[s.length()];
}
```

---

### 140. Word Break II

Given a **non-empty** string *s* and a dictionary *wordDict* containing a list of **non-empty** words, add spaces in *s* to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

**Note:**

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.

**Example 1:**

```
Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]
```

**Example 2:**

```
Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```
Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```

 



```java
public List<String> wordBreak(String s, List<String> wordDict) {
    Set<String> set= new HashSet<>(wordDict);
    int max=0;
    for (String w: wordDict) max=Math.max(max, w.length());
    boolean[] canBreak= new boolean[s.length()+1];
    Arrays.fill(canBreak, true);
    List<String> res= new ArrayList<>();
    DFS(res, new StringBuilder(), canBreak, set, s, max, 0);
    return res;
}

private boolean DFS(List<String> res, StringBuilder sb, boolean[] canBreak, Set<String> set, String s, int max, int lo){
    if (lo==s.length()){
        res.add(sb.toString().trim());
        return true;
    }
    boolean breakable=false;
    int rightBound= Math.min(s.length(), lo+max);
    int len= sb.length();
    for (int hi=lo+1; hi<=rightBound; hi++){
        if (!canBreak[hi]) continue;
        String temp= s.substring(lo,hi);
        if (set.contains(temp)){
            sb.append(temp).append(" ");
            breakable |= DFS(res, sb, canBreak, set, s, max, hi);
            sb.setLength(len);
        }
    }
    canBreak[lo]=breakable;
    return breakable;
}
```

