---
layout: post
title: House Robber 打劫房屋 
categories: [Dynamic Programming]
description: 假设你是一个专业的窃贼，准备沿着一条街打劫房屋。每个房子都存放着特定金额的钱。你面临的唯一约束条件是：相邻的房子装着相互联系的防盗系统，且当相邻的两个房子同一天被打劫时，该系统会自动报警。
keywords: 最大值, 动态规划, 动归, 滚动数组, dp, dynamic programming
---

# House Robber 打劫房屋
假设你是一个专业的窃贼，准备沿着一条街打劫房屋。每个房子都存放着特定金额的钱。你面临的唯一约束条件是：相邻的房子装着相互联系的防盗系统，且当相邻的两个房子同一天被打劫时，该系统会自动报警。

给定一个非负整数列表，表示每个房子中存放的钱， 算一算，如果今晚去打劫，你最多可以得到多少钱 在不触动报警装置的情况下。

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.



# 解题思路
要想最终偷到的价值最大，则必须是以下两种情况中的其中一种
* 截止到倒数第二个房子的最大价值加上最后一个房子的价值
* 截止到倒数第一个房子的最大价值

这两种情况谁大，谁就是最终答案。

## 1. 定义状态
f[i] = 截止到第i个房子的最大价值

## 2. 定义状态转移方程
```
f[i] = Max(f[i - 2] + a[i - 1], f[i - 1]);
```
这里的a[i - 1]代表数组中最后一个房子

## 3. 初始化状态
f[0] = 0;//没有房子可偷
f[1] = a[0];//只有一个房子可偷

## 4. 答案
f[n], n为最后一个房子

# Code
```
/**
 * @param A: An array of non-negative integers.
 * return: The maximum amount of money you can rob tonight
 */
public long houseRobber(int[] A) {
    if (A == null || A.length == 0) {
        return (long) 0;
    }
    int n = A.length;
    // define status
    long[] f = new long[n + 1];// if value is too big, overflow
    // initalize
    f[0] = 0;
    f[1] = A[0];
    for (int i = 2; i <= n; i++) {
        f[i] = Math.max(f[i - 1], f[i - 2] + A[i - 1]);    
    }
    return f[n];
}
```

# Challenge 
O(n) time and O(1) memory.

上面的时间复杂度为O(n),空间复杂度为O(n)
为了优化空间复杂度，则必须压缩f数组。
f数组保存的是截止到第i个元素的最大价值，
在状态转移方程中为了计算截止到第i个房子的最大价值，我们只用到了```f[i - 1]```和```f[i - 2]```
所以我们只需要2个空间来保存之前房子的最大价值状态。
优化后代码如下

```
/**
 * @param A: An array of non-negative integers.
 * return: The maximum amount of money you can rob tonight
 */
public long houseRobber(int[] A) {
    if (A == null || A.length == 0) {
        return (long) 0;
    }
    int n = A.length;
    // define status
    long[] f = new long[2];// if value is too big, overflow
    // initalize
    f[0] = 0;
    f[1] = A[0];
    for (int i = 2; i <= n; i++) {
        f[i % 2] = Math.max(f[(i - 1) % 2], f[(i - 2) % 2] + A[i - 1]);    
    }
    return f[n % 2];
}
```

这里用了一个小技巧
因为f[i]只与前面第 i-1 和 i-2 有关，所以可以用%2的方式来取前面的2个值。

# 相关问题列表 
* House Robber III 
* House Robber II 
* Paint House 
* Paint Fence 
* Fibonacci 
* Climbing Stairs

# Reference 
Leetcode, Lintcode
