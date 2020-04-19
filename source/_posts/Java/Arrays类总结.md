---
title: Arrays类总结
tags:
comments: true
mathjax: false
date: 2018-04-15 21:09:37
categories: Java
---

Arrays累里面有很多操作数组很有用的方法。本文是对Arrays类里常用方法的总结。

<!-- more -->

## Arrays.sort()方法

Arrays.sort()方法不仅可以对 byte[], char[], double[], float[], int[], long[], short[], Object[] 数组进行排序，而且还可以对自定义类型的数组进行排序。默认的排序顺序是从小到大，可以自定义比较器来自定义排序顺序。

- `static void sort(byte[] a)`
- `static void sort(byte[] a, int fromIndex, int toIndex)`

### 例1: 对二维数组进行排序

这里传入一个二维数组matrix，以每行第1个元素为标准，从小到大对int[]类型的元素排序。

```java
public int[][] testArraysSort(int[][] matrix) {
    Arrays.sort(matirx, new Comparator<int[]>() {
        @Override
        public int compare(int[] o1, int[] o2) {
            return o1[0] - o2[0];
        }
    });
}
```

上面compare函数中国返回值是`o1[0] - o2[0]`，这样就会从小到大排序，如果换成`o2[0] - o1[0]`就会从大到小排序。

### 例2: 对自定义类型数组进行排序

我们自定义一个People类型：

```java
class People {
    int age;
    String name;
    
    People(int age, String name) {
        this.age = age;
        this.name = name;
    }
}
```

然后使用Arrays.sort()方法以每个People对象的age从小到大排序：

```java
public testArraysSort() {
    People[] people = new People[10];
    for (int i = 0; i < 10; i++) {
        people[i] = new People(i, "xx");
    }
    Arrays.sort(people, new Comparator<Bear>() {
        @Override
        public int compare(People o1, People o2) {
            return o1.age - o2.age;
        }
    });
}
```

Arrays.sort()第一个传入参数是需要排序的对象，第二个传入参数是我们自定义的比较器，使用匿名内部类的形式编写，在匿名内部类内部，我们重写Comparator中的compare方法，传入两个参数，代表是people对象中需要进行对比的两个元素。

在compare方法内部，我们就可自定义排序规则了，compare方法返回一个int类型的数据，如果compare返回值是正数，则o1排在o2前面，如果返回负数o1排在o2后面。也可以这样记：`o1.age - o2.age`是从小到大排，`o2.age - o1.age`是从大到小排。

---

## Arrays.fill()方法

Arrays.fill()方法可以往数组里面填充内容，支持的数组类型有：boolean[], byte[], char[], double[], float[], int[], long[], short[], Object[]等。

- `static void fill(boolean[] a, boolean val)`
- `static void fill(boolean[] a, int fromIndex, int toIndex, boolean val)`

---

## Arrays.binarySearch()方法

Arrays.binarySearch()方法可以在数组里进行二分查找，如果找到则返回对应元素的下标，如果没有找到，则返回一个负数，这个负数的含义是：`(-(insertion point) - 1)`。

支持的数组类型有：byte[], char[], double[], float[], int[], long[], short[], Object[], 还有自定义类型数组。

- `static int binarySearch(byte[] a, byte key)`
- `static int binarySearch(byte[] a, int fromIndex, int toIndex, byte key)`

### 例1：在自定义类型数组中搜索元素

新建一个Bear类型，Bear有两个属性id和name，然后在main方法中新建一个长度为10的Bear数组，我们在Arrays.binarySearch方法中定义比较器，只比较Bear的id，如果id相同就认为找到了元素，然后返回找到元素在数组中的下标，如果没找到元素，会根据id从小到大的顺序返回一个包含插入点信息的负数。

```java
static class Bear {
    int id;
    String name;
    Bear(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
public static void main(String[] args) {
    Bear[] bears = new Bear[10];
    for (int i = 0; i < 10; i++) {
        bears[i] = new Bear(i, "xx"+i);
    }
    Bear targetBear = new Bear(12, "XX");
    int res = Arrays.binarySearch(bears, targetBear, new Comparator<Bear>() {
        @Override
        public int compare(Bear o1, Bear o2) {
            return o1.id - o2.id;
        }
    });
    System.out.println(res);
}
```

