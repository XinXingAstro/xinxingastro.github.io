---
title: Leetcode739. Daily Temperatures
tags:
  - 数组
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-22 14:15:27
categories: Leetcode题解
---

给出一个温度数组每个元素代表当天温度，要求返回一个数组，数组元素代表对于每一天的温度需要等待几天才能出现比当前温度更高的温度。

<!-- more -->

---

### 739. Daily Temperatures

Given a list of daily `temperatures`, produce a list that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put `0` instead.

For example, given the list `temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`, your output should be `[1, 1, 4, 2, 1, 1, 0, 0]`.

**Note:** The length of `temperatures` will be in the range `[1, 30000]`. Each temperature will be an integer in the range `[30, 100]`.

---

#### 算法1: 暴力搜索

我们从后向面遍历数组，对于每个元素我们再向后搜索知道找到第一个比他大的元素，记录他们他们的距离。

```java
public int[] dailyTemperatures(int[] temperatures) {
    int[] res = new int[temperatures.length];
    for (int i = temperatures.length - 1; i >= 0 ; i--) {
        for (int j = i + 1; j < temperatures.length; j++) {
            if(temperatures[j] > temperatures[i]) {
                res[i] = j - i;
                break;
            }
        }
    }
    return res;
}
```

时间复杂度：O(n^2)

空间复杂度：O(1)

#### 算法2: 使用数组记录温度坐标

我们对算法1进行优化，看能否降低算法设计复杂度。由于温度的范围在30～100之间，所以我们可以用一个长度为101的数组map记录温度在输入数组中出现的坐标（map中元素的下标代表温度，元素内容代表该温度在输入数组中出现的坐标），对于每一天的温度T，我们在map中搜索比它高的温度，从中找到坐标离它最近的那一个，然后计算出距离差就是解。

```java
public int[] dailyTemperatures(int[] temperatures) {
    int[] res = new int[temperatures.length];
    int[] map = new int[101];
    Arrays.fill(map, Integer.MAX_VALUE);
    for (int i = temperatures.length - 1; i >= 0 ; i--) {
        map[temperatures[i]] = i;
        int warm = Integer.MAX_VALUE;
        for (int j = temperatures[i] + 1; j <= 100; j++) {
            if (map[j] < warm)
                warm = map[j];
        }
        if (warm < Integer.MAX_VALUE) 
            res[i] = warm - i;
    }
    return res;
}
```

时间复杂度：O(n)

空间复杂度：O(1)

#### 算法3: 使用栈记录温度坐标

原理和算法2的大致相同，我们还是从后向前遍历数组，然后逐个对比当前温度和栈顶坐标所代表的温度。

1）如果当前温度大于栈顶温度，则弹出栈顶温度坐标，直到栈顶出现一个大于当前温度的坐标或者栈为空。

2）如果栈为空则数名当前温度后面没有比其更高的温度结果为0，如果得到栈顶温度大于当前温度，则记录坐标的距离差得到解。

```java
public int[] dailyTemperatures(int[] t) {
    LinkedList<Integer> stack = new LinkedList();
    int[] res = new int[t.length];
    for (int i = t.length - 1; i >= 0; i--) {
        while (!stack.isEmpty() && t[i] >= t[stack.peek()])
            stack.pop();
        res[i] = stack.isEmpty() ? 0 : stack.peek() - i;
        stack.push(i);
    }
    return res;
}
```

我们可以用数组代替栈来提高算法时间复杂度：

```java
public int[] dailyTemperatures(int[] t) {
    // LinkedList<Integer> stack = new LinkedList();
    int[] stack = new int[t.length];
    int size = 0;
    int[] res = new int[t.length];
    for (int i = t.length - 1; i >= 0; i--) {
        while (size > 0 && t[i] >= t[stack[size]]) {
            // stack.pop();
            stack[size--] = 0;
        }
        // res[i] = stack.isEmpty() ? 0 : stack.peek() - i;
        res[i] = size == 0 ? 0 : stack[size] - i;
        // stack.push(i);
        stack[++size] = i;
    }
    return res;
}
```

时间复杂度：O(n)

空间复杂度：O(n)