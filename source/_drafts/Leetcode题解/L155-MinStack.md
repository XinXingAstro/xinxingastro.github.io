---
title: Leetcode155. Min Stack
tags:
  - 栈
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-09 16:19:31
categories: Leetcode题解
---

实现带有getMin()方法的栈数据结构。

剑指Offer面试题30: 包含min函数的栈。

<!-- more -->

---

### 155. Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

**Example:**

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

---

#### 算法：最小数栈

我们采用两个栈来实现取最小数这个功能，stack栈是正常栈，mstack栈中存放当前stack中的最小数，当我们压人一个数时，如果该数比mstakc.peek()大则向mstack中压入mstack.peek()，否则向mstack中压入该数。

注意：不要使用变量来存放最小值，这样会出现很多异常，应该始终使用mstack.peek()来判断当前stack()栈中的最小值，这样既方便又能保证正确。

```java
class MinStack {
    private LinkedList<Integer> stack;
    private LinkedList<Integer> mstack;
    /** initialize your data structure here. */
    public MinStack() {
        this.stack = new LinkedList<>();
        this.mstack = new LinkedList<>();
    }
    public void push(int x) {
        // 这部分是核心代码
        if (mstack.isEmpty() || x < mstack.peek()) {
            mstack.push(x);
        } else {
            mstack.push(mstack.peek());
        }
        stack.push(x);
    }
    public void pop() {
        stack.pop();
        mstack.pop();
    }
    public int top() {
        return stack.peek();
    }
    public int getMin() {
        return mstack.peek();
    }
}
```

