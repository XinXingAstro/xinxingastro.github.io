---
title: Leetcode85. Maximal Rectangle
tags:
  - 动态规划
comments: true
mathjax: false
date: 2018-04-09 22:35:57
categories: Leetcode题解
---

找到矩阵中最大的全是1的矩阵块。

<!-- more -->

### 85. Maximal Rectangle

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

For example, given the following matrix:

```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

Return 6.

#### 算法1：动态规划+分治法

我们可以根据Leetcode 84题的解法，将矩阵的每一行看作一个矩形条，逐行遍历，构建矩形条数组，计算最大矩形面积：

1. 遍历矩阵第一行的时候，矩阵第一行的矩形条数组和矩阵第一行完全一样 `[1, 0, 1, 0, 0]` 找出该矩形条的最大矩形面积为1。
2. 遍历矩阵第二行，当第二行的某个元素不为0的时候，该元素就累加第一行相应元素的值，如果第二行某元素为0，则矩形条数组中这个位置也为0，`[2, 0, 2, 1, 1]` 相应的最大矩形面积为3。
3. 遍历矩阵第三行，得到矩形条数组：`[3, 1, 3, 2, 2] ` 则最大矩形面积为6。
4. 遍历矩阵第四行，`[4, 0, 0, 3, 0]` 最大矩形面积4。

综合上述四步，得到该矩阵最大的1矩阵的面积为6。

时间复杂的：O(mLogn)；空间复杂度：O(n)；

```java
public int maximalRectangle(char[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;
    int m = matrix.length;
    int n = matrix[0].length;
    int maxArea = 0;
    int[] h = new int[n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (matrix[i][j] == '1') h[j] += 1;
            else h[j] = 0;
        }
        maxArea = Math.max(maxArea, getMax(h, 0, n-1));
    }
    return maxArea;
}
private int getMax(int[] h, int left, int right) {
    if (left > right) return 0;
    if (left == right) return h[left];
    int minIndex = left;
    boolean sorted = true;
    for (int i = left+1; i <= right; i++) {
        if (h[i] < h[i-1]) sorted = false;
        if (h[i] < h[minIndex]) minIndex = i;
    }
    if (sorted) {
        int max = 0;
        for (int i = left; i <= right; i++) {
            int area = h[i] * (right - i + 1);
            if (area > max) max = area;
        }
        return max;
    } else {
        int maxLeft = getMax(h, left, minIndex-1);
        int maxRight = getMax(h, minIndex+1, right);
        int crossMax = h[minIndex] * (right - left + 1);
        return Math.max(Math.max(maxLeft, maxRight), crossMax);
    }
}
```



#### 算法2：动态规划+辅助栈

根据 Leetcode 84 题的辅助栈算法，用动态规划构建好矩形条数组，然后直接调用辅助栈算法求出最大矩形面积。

```java
public int maximalRectangle(char[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;
    int m = matrix.length;
    int n = matrix[0].length;
    int maxArea = 0;
    int[] h = new int[n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (matrix[i][j] == '1') h[j] += 1;
            else h[j] = 0;
        }
        maxArea = Math.max(maxArea, largestRectangleArea(h));
    }
    return maxArea;
}

public int largestRectangleArea(int[] heights) {
    if (heights == null || heights.length == 0) return 0;
    ArrayDeque<Integer> stack = new ArrayDeque<>();
    int length = heights.length;
    int maxArea = 0;
    int h, w, height;   
    for (int i = 0; i <= length; i++) {
        if (i == length) height = 0;
        else height = heights[i];
        if (stack.isEmpty() || height > heights[stack.peek()]) {
            stack.push(i);
        } else {
            while (!stack.isEmpty() && height < heights[stack.peek()]) {
                h = heights[stack.pop()];
                //这里要计算矩形宽度,如果此时栈为空，则说明栈顶矩形条前面没有比该矩形条低的矩形条，
                //所以左边界就是0，矩形宽度就是i，如果栈中还有元素，则该元素右边就是矩形的左边界，
                //所以矩形宽度就是 i-(stack.peek() + 1)
                w = stack.isEmpty() ? i : i - (stack.peek() + 1);
                maxArea = Math.max(maxArea, h*w);
            }
            stack.push(i);
        }
    }
    return maxArea;
}
```



