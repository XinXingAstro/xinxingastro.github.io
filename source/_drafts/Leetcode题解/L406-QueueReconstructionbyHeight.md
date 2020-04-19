---
title: Leetcode406. Queue Reconstruction by Height
tags:
  - 贪心算法
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-13 15:44:40
categories: Leetcode题解
---

给出一个二维数组，每行有2个元素，第一个元素代表一个人的身高，第二个元素代表这个人前面有几个比他高的人，按照这个规则对这个二维数组重新排序。

<!-- more -->

---

#### 406. Queue Reconstruction by Height

Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers `(h, k)`, where `h` is the height of the person and `k` is the number of people in front of this person who have a height greater than or equal to `h`. Write an algorithm to reconstruct the queue.

**Note:**
The number of people is less than 1,100.

 

**Example**

```
Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

---

#### 算法：贪心算法

按照贪心算法的思想，我们先对个高的进行排序，然后将个子小的人插入的高个子队列中。在两个高度相同的人之间，由于第二个参数表示他前面可以有几个比他高或者一样高的人，所以第二个参数小的那个人应该排前面。所以我们以每个人的身高从大到小排序，如果身高相同以第二个元素从小到大排序，然后从前到后逐个将人插入到队列中去。

```java
public int[][] reconstructQueue(int[][] people) {
    Arrays.sort(people, new Comparator<int[]>() {
        @Override
        public int compare(int[] o1, int[] o2) {
            return o1[0] != o2[0] ? Integer.compare(o2[0], o1[0]) : Integer.compare(o1[1], o2[1]);
        }
    });
    ArrayList<int[]> list = new ArrayList<>();
    for (int[] p : people)
        list.add(p[1], p);
    return list.toArray(new int[people.length][]);
}
```

这个算法有两处写法需要注意：

1）我们在使用Arrays.sort()算法对一个数组进行排序时，如果想要自定义排序规则，我么需要传入一个Comparator类型的匿名内部类，并且实现它的compare()方法，在方法中，如果想要从大到小排序，则需要在后面的元素小于前面的元素时返回正数，等于时返回0，大于时返回负数。如果我们使用了Integer.compare()方法，则只需要将第一个传入参数写为后面的元素，第二个写为前面的元素，因为Integer.compare()方法的源码为：

```java
public static int compare(int x, int y) {
    return (x < y) ? -1 : ((x == y) ? 0 : 1);
}
```

2）我们在使用ArrayList的toArray()方法将list转换成矩阵时，如果不指定怎么转换，则会返回一个object[]，我们需要在toArray()方法中指定矩阵的类型和大小如：

```java
list.toArray(new int[people.length][]);
```

