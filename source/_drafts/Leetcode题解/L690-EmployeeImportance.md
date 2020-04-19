---
title: Leetcode690. Employee Importance
tags:
  - DFS
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2018-06-30 10:58:59
categories: Leetcode题解
---

给出一个员工表，求指定员工及其下属的所有importance的和。

<!-- more -->

---

### 690. Employee Importance

You are given a data structure of employee information, which includes the employee's **unique id**, his **importance value** and his **direct**subordinates' id. 

For example, employee 1 is the leader of employee 2, and employee 2 is the leader of employee 3. They have importance value 15, 10 and 5, respectively. Then employee 1 has a data structure like [1, 15, [2]], and employee 2 has [2, 10, [3]], and employee 3 has [3, 5, []]. Note that although employee 3 is also a subordinate of employee 1, the relationship is **not direct**.

Now given the employee information of a company, and an employee id, you need to return the total importance value of this employee and all his subordinates.

**Example 1:**

```
Input: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
Output: 11
Explanation:
Employee 1 has importance value 5, and he has two direct subordinates: employee 2 and employee 3. They both have importance value 3. So the total importance value of employee 1 is 5 + 3 + 3 = 11.
```

**Note:**

1. One employee has at most one **direct** leader and may have several subordinates.
2. The maximum number of employees won't exceed 2000.

---

#### 算法：DFS

利用深度优先遍历算法求出所有员工的importance和。

```java
private int res;
public int getImportance(List<Employee> employees, int id) {
    if (employees == null) return res;
    DFS(employees, getEmployee(employees, id));
    return res;
}
public void DFS(List<Employee> employees, Employee e) {
    if (e != null) {
        res += e.importance;
        for (Integer id : e.subordinates) {
            DFS(employees, getEmployee(employees, id));
        }
    }
}
public Employee getEmployee(List<Employee> employees, int id) {
    Employee res = null;
    for (Employee e : employees) {
        if (e.id == id) {
            res = e;
            break;
        }
    }
    return res;
}
```

Leetcode运行时间：30ms

在上面算法中要得到一个指定id的employee我们必须遍历整个employees链表，算法的时间主要消耗在这里，我们可以添加一个map来存储id和相应employee信息，加快employee的检索速度。

```java
private Employee[] map = new Employee[2001];
private int res;
public int getImportance(List<Employee> employees, int id) {
    if (employees == null) return res;
    for (Employee e : employees) {
        map[e.id] = e;
    }
    DFS(map[id]);
    return res;
}
public void DFS(Employee e) {
    if (e != null) {
        res += e.importance;
        for (int id : e.subordinates) {
            DFS(map[id]);
        }
    }
}
```

Leetcode运行时间：11ms