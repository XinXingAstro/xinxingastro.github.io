---
title: LRU Cache的原理及实现
tags:
  - null
  - null
comments: true
mathjax: false
updated:
date: 2018-07-19 19:40:29
categories: 计算机基础
---

LRU近期最少使用缓存替换策略的原理及实现。

<!-- more -->

---

> 局部性原理：CPU访问存储器时，无论是存取指令还是存取数据，所访问的存储单元都趋于聚集在一个较小的连续区域中。
>
> 三种不同的局部性：
>
> 时间局部性（Temporal Locality）：如果一个信息项正在被访问，那么在近期它很可能还会被再次访问。程序循环、堆栈等是产生时间局部性的原因。
>
> 空间局部性（Spatial Locality）：在最近的将来将用到的信息很可能与现在正在使用的信息在空间地址上是临近的。
>
> 顺序局部性（Order Locality）：在典型程序中，除转移类指令外，大部分指令是顺序进行的。顺序执行和非顺序执行的比例大致是5:1。此外，对大型数组访问也是顺序的。指令的顺序执行、数组的连续存放等是产生顺序局部性的原因。

>LRU(Least Recently Used)近期最少使用算法，是内存管理的一种页面置换算法，通常用与缓存的淘汰策略实现，由于缓存的内存非常宝贵，所以需要根据某种规则来剔除数据，保证内存不被撑满。

常用的缓存Redis中可以设置内存的最大使用量，当内存的使用量超过这个最大值时，Redis就会实施淘汰策略，Redis中有6种常用的数据淘汰策略：

|                 |                                                          |
| --------------- | -------------------------------------------------------- |
| volatile-lru    | 从已设置过期时间的数据集中挑选**最近最少使用**的数据淘汰 |
| volatile-ttl    | 从已设置过期时间的数据集中挑选**将要过期**的数据淘汰     |
| volatile-random | 从已设置过期时间的数据集中**随机挑选**数据淘汰           |
| allkeys-lru     | 从全部数据集中挑选**最近最少使用**的数据淘汰             |
| allkeys-random  | 从全部数据集中**随机挑选**数据淘汰                       |
| no-enviction    | 禁止驱逐数据                                             |

在Mybatis中也实现了使用LRU策略的缓存，我们通过Mybatis中的[LruCache.java](https://github.com/mybatis/mybatis-3/blob/master/src/main/java/org/apache/ibatis/cache/decorators/LruCache.java)看一下LRU Cache的实现原理。

```java
package org.apache.ibatis.cache.decorators;

import java.util.LinkedHashMap;
import java.util.Map;
import java.util.concurrent.locks.ReadWriteLock;

import org.apache.ibatis.cache.Cache;

/**
 * Lru (least recently used) cache decorator
 *
 * @author Clinton Begin
 */
public class LruCache implements Cache {

  private final Cache delegate;
  private Map<Object, Object> keyMap;
  private Object eldestKey;

  public LruCache(Cache delegate) {
    this.delegate = delegate;
    setSize(1024);
  }

  public void setSize(final int size) {
    keyMap = new LinkedHashMap<Object, Object>(size, .75F, true) {
      private static final long serialVersionUID = 4267176411845948333L;

      @Override
      protected boolean removeEldestEntry(Map.Entry<Object, Object> eldest) {
        boolean tooBig = size() > size;
        if (tooBig) {
          eldestKey = eldest.getKey();
        }
        return tooBig;
      }
    };
  }

  @Override
  public void putObject(Object key, Object value) {
    delegate.putObject(key, value);
    cycleKeyList(key);
  }

  @Override
  public Object removeObject(Object key) {
    return delegate.removeObject(key);
  }
    
  private void cycleKeyList(Object key) {
    keyMap.put(key, key);
    if (eldestKey != null) {
      delegate.removeObject(eldestKey);
      eldestKey = null;
    }
  }

  @Override
  public Object getObject(Object key) {
    keyMap.get(key); //touch
    return delegate.getObject(key);
  }
}
```

LruCache类中定义了三个属性，其中keyMap指向一个LinkedHashMap对象用来存储缓存数据，eldestKey指向当前数据中最老的节点（既LinkedHashMap中的头节点）。

在LurCache的构造方法中指定缓存的默认大小是1024个节点，在新建LinkedHashMap对象时指定了LinkedHashMap的容量（默认1024），负载因子0.75，并且指定元素的顺序为按**访问顺序**排序，在指定按照访问顺序排序后，我们每次调用get方法后都会将这次访问的节点移至链表末尾，不断访问可以形成按照访问顺序排序的链表，这就是为什么链表头节点是链表中最近最少访问的节点。（关于LinkedHashMap的实现原理请参考：LinkedHashMap底层实现原理）

在新建LinkedHashMap对象时，还重写了LinkedHashMap的removeEldestEntry方法，在该方法中判断，如果当前链表的节点数大于1024时就将tooBig变量设为true，然后将eldestKey指向LinkedHashMap的头节点，在cycleKeyList方法中每次向链表中put完节点后，都会检查eldestKey是否为null，如果不为null就删除eldestKey指向的节点，然后将eldestKey设为null，至此就实现了使用LRU策略的缓存。

> 参考：[动手实现一个 LRU cache](http://ifeve.com/动手实现一个-lru-cache/#more-37981)
>
> [LinkedHashMap 的实现原理](http://wiki.jikexueyuan.com/project/java-collection/linkedhashmap.html)

