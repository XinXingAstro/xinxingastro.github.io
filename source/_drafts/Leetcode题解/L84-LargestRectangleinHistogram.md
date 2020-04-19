---
title: Leetcode84. Largest Rectangle in Histogram
tags:
  - 动态规划
  - 分治法
comments: true
mathjax: false
date: 2018-04-09 09:43:34
categories: Leetcode题解
---

计算矩形条的最大面积。

<!-- more -->

### 84. Largest Rectangle in Histogram

Given *n* non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![img](https://leetcode.com/static/images/problemset/histogram.png)

Above is a histogram where width of each bar is 1, given height = `[2,1,5,6,2,3]`.

![img](https://leetcode.com/static/images/problemset/histogram_area.png)

The largest rectangle is shown in the shaded area, which has area = `10` unit.

For example,
Given heights = `[2,1,5,6,2,3]`,
return `10`.



#### 算法1: 辅助栈法

依次遍历所有矩形条，并尝试计算以该矩形条高度为高的矩形面积。对于能确定左右边界的矩形条，计算出最大矩形面积后从栈中弹出，对于不能确定左右边界的矩形条，先将其压入辅助栈，等待后续处理。

如何确定以一个矩形条的高为高的矩形的左右边界：

1. 如果一个矩形条左边某处出现比该矩形条低的矩形条，则左边界就是这个低矩形条的右边。
2. 如果一个矩形条右边某处出现比该矩形条低的矩形条，则右边界就是这个低矩形条的左边。

总之，**在一个矩形条左右两边找到第一个比该矩形条低的矩形条，以该矩形条高为高的矩形的左右边界就可以确定。**

开始遍历矩形条，由于目标时计算矩阵的面积，要计算矩形的宽度，高度可以直接得到，所以压入栈的应该是各矩形条的坐标：

1. 如果栈为空，则压入当前矩形条坐标。
2. 如果当前矩形条比栈顶矩形条高，则只可以确定以当前矩形条高为高的矩形的左边界，右边界无法确定，所以将该矩形条的坐标压入栈。
3. 如果当前矩形条比栈顶矩形条低，那么栈中所有比当前矩形条高的矩形条的相应矩形的右边界就都可以确定了。所以我们计算出这些矩形的面积，然后将相应的矩形条从栈中弹出，将当前矩形条坐标压入栈。
4. 遍历高度数组到最后的时候，栈中有可能还有元素，所以最后要压入一个比所有矩形条都低的元素，以确定所有矩形的右边界，然后计算所有矩形的面积，所以我们在矩形条高度数组最后加入元素0，来获得所有矩形的面积。



```java
public int largestRectangleArea(int[] heights) {
    if (heights == null || heights.length == 0) return 0;
    Stack<Integer> stack = new Stack<>();
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



#### 算法2：分治策略

将原始问题分解为子问题，对于该题，我们**用高度最小的矩形条，将原始问题分解为两个子问题**：

1. 如果子问题中所有矩形条的高度是从小到大排好序的，那么我们从左到右依次算出以各个矩形条的高的高的矩形的面积，找出最大面积。
2. 如果子问题中矩形条的高度不是排好序的，那么我们继续分割。

时间复杂度：O(logn)；空间复杂度：O(1)；

```java
public int largestRectangleArea(int[] heights) {
    return getMax(heights, 0, heights.length-1);
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

