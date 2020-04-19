---
title: Leetcode4. Median of Two Sorted Arrays
tags:
  - 分治法
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-07-20 16:50:58
categories: Leetcode题解
---

找到两个排序数组的中位数。

<!-- more -->

---

### 4. Median of Two Sorted Arrays

There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

**Example 1:**

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

---

#### 算法：查找第k小的数

这道题目的难点在于限制了时间复杂度为O(log(m+n))，所以我们就不能使用时间复杂的为O(m+n)的遍历搜索的方法进行解题。

算法的核心思想是找到两个数组合并以后数组中第(m + n + 1)/2小的数，我们记`k = (m + n + 1)/2`。

两个数组合并以后，第k小的数前面一定有k-1个数比他小，我们的做法是比较两个数组中第`k/2`小的两个数，会出现下面三种情况：

1）如果A[k/2-1] == B[k/2-1]，那么在A数组中A[k/2-1]前面有k/2-1个数组比它小，在B数组中B[k/2-1]前面有k/2-1个数组比它小，由于A[k/2-1] == B[k/2-1]所以A B合并后这两个数应该相邻，并且这两个数前面有(k/2-1+k/2-1) = k-2个数，那么A[k/2-1]或者B[k/2-1]就正好是两数组合并后第k小的数，我们就得到了解。

2）如果A[k/2-1] < B[k/2-1]，这说明A数组的前k/2个数合并后一定会排到B[k/2-1]这个数前面，这时候A[k/2-1]和B[k/2-1]这两个数在合并数组中的位置就不确定了，因为A中可能不止有k/2个数比B[k/2-1]小，所以B[k/2-1]的位置可能更靠后。这时我们的做法是直接抛弃A的前k/2个数，然后设置k=k-k/2，然后我们继续在剩下的A和B中比较A[k/2-1]和B[k/2-1]，直到找到两个相等的数，或者k=1为止。

3）如果A[k/2-1] > B[k/2-1]，这时就说明A[k/2-1]在合并数组中的位置可能更靠后，这时我们抛弃B数组的前k/2个数，然后设置k=k-k/2，然后继续比较A[k/2-1]和B[k/2-1]，直到找到两个相等的数，或者k=1为止。

```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {      
    int l = (nums1.length + nums2.length + 1) / 2;
    int r = (nums1.length + nums2.length + 2) / 2;
    return (findKth(nums1, 0, nums2, 0, l) + findKth(nums1, 0, nums2, 0, r)) / 2.0;
} 

public double findKth(int[] A, int aStart, int[] B, int bStart, int k) {
    if (aStart >= A.length) return B[bStart + k - 1];
    if (bStart >= B.length) return A[aStart + k - 1];
    if (k == 1) return Math.min(A[aStart], B[bStart]);

    int aMid = Integer.MAX_VALUE;
    int bMid = Integer.MAX_VALUE;
    if (aStart + k/2 - 1 < A.length) aMid = A[aStart + k/2 - 1];
    if (bStart + k/2 - 1 < B.length) bMid = B[bStart + k/2 - 1];

    if (aMid < bMid) {
        return findKth(A, aStart + k/2, B, bStart, k - k/2);
    } else {
        return findKth(A, aStart, B, bStart + k/2, k - k/2);
    }
}
```

时间复杂度：O(log(m+n)).

空间复杂度：O(1).