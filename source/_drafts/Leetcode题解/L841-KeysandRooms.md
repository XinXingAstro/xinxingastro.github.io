---
title: Leetcode841. Keys and Rooms
tags:
  - DFS
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-20 22:56:27
categories: Leetcode题解
---

在一个图中进行搜索，如果可以访问到每一个节点则返回true，如果不能则返回false。

<!-- more -->

---

### 841. Keys and Rooms

There are `N` rooms and you start in room `0`.  Each room has a distinct number in `0, 1, 2, ..., N-1`, and each room may have some keys to access the next room. 

Formally, each room `i` has a list of keys `rooms[i]`, and each key `rooms[i][j]` is an integer in `[0, 1, ..., N-1]` where `N = rooms.length`.  A key `rooms[i][j] = v` opens the room with number `v`.

Initially, all the rooms start locked (except for room `0`). 

You can walk back and forth between rooms freely.

Return `true` if and only if you can enter every room.


**Example 1:**

```
Input: [[1],[2],[3],[]]
Output: true
Explanation:  
We start in room 0, and pick up key 1.
We then go to room 1, and pick up key 2.
We then go to room 2, and pick up key 3.
We then go to room 3.  Since we were able to go to every room, we return true.
```

**Example 2:**

```
Input: [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can't enter the room with number 2.
```

**Note:**

1. `1 <= rooms.length <= 1000`
2. `0 <= rooms[i].length <= 1000`
3. The number of keys in all rooms combined is at most `3000`.

---

#### 算法：深度优先搜索

我们建立一个visited数组，记录图中每个节点是否被访问，深度优先搜索完后，遍历visited数组，如果所有的节点都是true则说明都访问过，如果有一个为false则返回false。

其中DFS深度优先搜索算法的结构是固定写法。

```java
public boolean canVisitAllRooms(List<List<Integer>> rooms) {
    boolean[] visited = new boolean[rooms.size()];
    DFS(visited, 0, rooms);
    for (int i = 0; i < visited.length; i++) {
        if (visited[i] == false)
            return false;
    }
    return true;
}
public void DFS(boolean[] visited, int node, List<List<Integer>> rooms) {
    if (visited[node] == true)
        return;

    visited[node] = true;

    for (Integer i : rooms.get(node)) {
        DFS(visited, i, rooms);
    }
}
```

