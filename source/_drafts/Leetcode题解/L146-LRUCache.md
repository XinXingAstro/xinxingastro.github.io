---
title: Leetcode146. LRU Cache
tags:
  - null
  - null
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-20 10:27:58
categories: Leetcode题解
---

实现一个使用LRU淘汰策略的缓存。参考：LRU Cache的原理及实现。

<!-- more -->

---

### 146. LRU Cache

Design and implement a data structure for [Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU). It should support the following operations: `get` and `put`.

`get(key)` - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
`put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

**Follow up:**
Could you do both operations in **O(1)** time complexity?

**Example:**

```
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

---

### 算法1: 使用Java自带容器LinkedHashMap实现

LinkedHashMap是支持排序的HashMap，其排序方法有两种，一种是按照元素插入顺序排序，一种是按照访问顺序排序，我们使用访问顺序排序，每次调用get方法后，被访问的元素就会被放在链表尾部，经过多次访问后，链表头部就是近期最少访问到的节点，当链表中元素的数量超过指定容量的时候，我们将链表头节点删除即可。

完全理解这个算法需要了解LinkedHashMap的底层实现，参考：LinkedHashMap的底层实现。

```java
class LRUCache {
    private Map<Integer, Integer> map;
    private Object eldestKey;

    public LRUCache(final int capacity) {
        map = new LinkedHashMap<Integer, Integer>(capacity, .75F, true) {
            @Override
            protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
                boolean tooBig = size() > capacity;
                if (tooBig) {
                    eldestKey = eldest.getKey();
                }
                return tooBig;
            }
        };
    }
    
    public int get(int key) {
        Integer ans = map.get(key);
        return ans == null ? -1 : ans;
    }
    
    public void put(int key, int value) {
        map.put(key, value);
        if (eldestKey != null) {
            map.remove(eldestKey);
            eldestKey = null;
        }
    }
}
```

### 算法2: 自己手写一个LRU Cache

我们自定义一个双向链表节点，然后结合HashMap容器实现一个支持按访问顺序排序的链表结构。

```java
class LRUCache {
    private int size;
    private Map<Integer, LinkedNode> map;
    private LinkedNode head;
    private LinkedNode tail;

    public LRUCache(int size) {
        this.size = size;
        map = new HashMap<Integer, LinkedNode>(size, .75F);
    }

    public int get(int key) {
        LinkedNode node = map.get(key);
        if (node == null) {
            return -1;
        } else {
            if (map.size() > 1 && tail != node)
                moveToTail(node);
            return tail.value;
        }
    }

    public void put(int key, int value) {
        LinkedNode node = map.get(key);
        if (node == null) {
            node = new LinkedNode(key, value);
            if (map.size() >= size) {
                // remove head node
                if (head != null) {
                    LinkedNode tmp = head;
                    head = head.pos;
                    if (head != null) head.pre = null;
                    if (tail == tmp) tail = null;
                    map.remove(tmp.key);
                }
            }
            map.put(key, node);
            insertTail(node);
        } else {
            // 如果节点存在，则更新节点的value，然后将几点放到末尾
            node.value = value;
            if (tail != node)
                moveToTail(node);

        }
    }

    public void insertTail(LinkedNode node) {
        if (tail == null) {
            tail = node;
        } else {
            tail.pos = node;
            node.pre = tail;
            tail = node;
        }
        if (head == null) {
            head = tail;
        }
    }

    public void moveToTail(LinkedNode node) {
        if (tail == null) {
            return;
        } else if (head == node && tail != node) {
            head = head.pos;
            node.pos = null;
            head.pre = null;
            insertTail(node);
        } else {
            LinkedNode preNode = node.pre;
            LinkedNode posNode = node.pos;
            preNode.pos = posNode;
            posNode.pre = preNode;
            node.pos = null;
            node.pre = null;
            insertTail(node);
        }
    }

    class LinkedNode {
        private int key;
        private int value;
        private LinkedNode pre;
        private LinkedNode pos;

        public LinkedNode(int key, int value) {
            this.key = key;
            this.value = value;
            pre = null;
            pos = null;
        }
    }
}
```

我定义了两个指针head，tail分别指向链表的开头节点和末尾节点，由于没有定义额外的开头节点和末尾节点，所以在代码中会出现很多堆书head或者tail判断是否为null的语句，在进行指针操作时也需要判断head和tail当前的值是否为null，所以为了简化操作，我们可以定义两个哨兵节点，使head和tail分别指向这两个节点，那么在代码中就会省略很多null值的判断。

优化代码如下：

```java
class LRUCache {
    private int size;
    private Map<Integer, LinkedNode> map;
    private LinkedNode head;
    private LinkedNode tail;

    public LRUCache(int size) {
        this.size = size;
        head = new LinkedNode(-1, -1);
        tail = new LinkedNode(-1, -1);
        head.pos = tail;
        tail.pre = head;
        map = new HashMap<Integer, LinkedNode>(size, .75F);
    }

    public int get(int key) {
        LinkedNode node = map.get(key);
        if (node == null) {
            return -1;
        }
        else {
            int value = node.value;
            moveToTail(node);
            return value;
        }
    }

    public void put(int key, int value) {
        LinkedNode node = map.get(key);
        if (node == null) {
            node = new LinkedNode(key, value);
            if (map.size() >= size) {
                // remove head node
                LinkedNode tmp = head.pos;
                LinkedNode posNode = head.pos.pos;
                head.pos = posNode;
                posNode.pre = head;
                map.remove(tmp.key);

            }
            map.put(key, node);
            insertTail(node);
        } else {
            // 如果节点存在，则更新节点的value，然后将几点放到末尾
            node.value = value;
            moveToTail(node);

        }
    }

    public void insertTail(LinkedNode node) {
        LinkedNode preNode = tail.pre;
        preNode.pos = node;
        node.pre = preNode;
        node.pos = tail;
        tail.pre = node;
    }

    public void moveToTail(LinkedNode node) {
        if (tail.pre == node) return;
        LinkedNode preNode = node.pre;
        LinkedNode posNode = node.pos;
        preNode.pos = posNode;
        posNode.pre = preNode;
        insertTail(node);
    }

    class LinkedNode {
        private int key;
        private int value;
        private LinkedNode pre;
        private LinkedNode pos;

        public LinkedNode(int key, int value) {
            this.key = key;
            this.value = value;
            pre = null;
            pos = null;
        }
    }
}
```





