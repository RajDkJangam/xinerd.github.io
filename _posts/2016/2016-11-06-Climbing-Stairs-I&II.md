---
layout: post
title: Climbing Stairs 爬楼梯
categories: [Dynamic Programming, 动态规划]
description: 假设你正在爬楼梯，需要n步你才能到达顶部。但每次你只能爬一步或者两步，你能有多少种不同的方法爬到楼顶部？
keywords: dp, dynamic programming, 动态规划, 动归, 滚动数组
---

# Climbing Stairs 爬楼梯
**中文** 假设你正在爬楼梯，需要n步你才能到达顶部。但每次你只能爬一步或者两步，你能有多少种不同的方法爬到楼顶部？

**样例**
比如n=3，1+1+1=1+2=2+1=3，共有3中不同的方法。返回 3

**ENGLISH** You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

**Example**
Given an example n=3 , 1+1+1=2+1=1+2=3. return 3

# 解题思路
第n阶楼梯一定是从第`n-1`或者`n-2`阶楼梯走来，所以如果我们知道了前面这两种状态的方案数，既知道了当前楼梯的方案总数。

再来考虑初始状态，当只有一层楼梯时，总方案数为1，当有两层楼梯时，方案总数为2。

对于没有楼梯的情况，可以根据题意来选择赋值为1或0，两者都可以做。如果赋值为1，可以认为没有楼梯时，就不走位一种方案。如果赋值为0，可以认为没有楼梯时就没有必要选择。
下面给出的答案种认为没有楼梯时我们就不走为一种方案。 

## 1. 定义状态
`f[i]`为第i层楼梯的方案总数

## 2. 定义状态转移方程
```
f[i] = f[i - 1] + f[i - 2]
```

## 3. 初始化状态
f[0] = 1
f[1] = 1

## 4. 答案
f[n]

# Code
```java
public class Solution {
    public int climbStairs(int n) {
        if (n <= 1) {
            return 1;
        }
        int[] f = new int[n + 1]; 
        f[0] = 1;
        f[1] = 1;
        for (int i = 2; i <= n; i++) {
            f[i] = f[i - 1] + f[i - 2];
        }
        return f[n];
    }
}
```

# 复杂度分析
时间复杂度为：O(n)
空间复杂度为：O(n)

# Challenge O(1) memory

## 滚动数组优化空间

通过观察上面的状态转移方程，我们知道f[i]只与f[i-1]和f[i-2]有关，所以我们只用保存i前面的2个状态，而并不需要保存i前面所有的状态。
这里仍然使用了和[Triangle 数字三角形](https://xinerd.github.io/2016/11/05/Triangle/)一样的优化方式-滚动数组。

```java
public class Solution {
    public int climbStairs(int n) {
        if (n <= 1) {
            return 1;
        }
        int[] f = new int[2];
        f[0] = 1;
        f[1] = 1;
        for (int i = 2; i <= n; i++) {
            f[i % 2] = f[(i - 1) % 2] + f[(i - 2) % 2];
        }
        return f[n % 2];
    }
}
```

通过这样优化后空间复杂度为O(1)。 


# 相关问题列表 
* Unique Paths I, II
* Clibing Stairs I, II

# Reference 
Leetcode, Lintcode


