---
layout: post
title: Longest Increasing Subsequence 最长上升子序列
categories: [Dynamic Programming, 动态规划]
description: 给定一个整数序列，找到最长上升子序列（LIS），返回LIS的长度。
keywords: LIS, 最大值, dp, dynamic programming, 动态规划, 动归
---

# Longest Increasing Subsequence 最长上升子序列
**中文**
给定一个整数序列，找到最长上升子序列（LIS），返回LIS的长度。

**说明**
最长上升子序列的定义：
最长上升子序列问题是在一个无序的给定序列中找到一个尽可能长的由低到高排列的子序列，这种子序列不一定是连续的或者唯一的。
[Longest_increasing_subsequence wiki](https://en.wikipedia.org/wiki/Longest_increasing_subsequence)

**样例**
For `[5, 4, 1, 2, 3]`, the LIS is `[1, 2, 3]`, return 3
For `[4, 2, 4, 5, 3, 7]`, the LIS is `[2, 4, 5, 7]`, return 4

**ENGLISH**
Given a sequence of integers, find the longest increasing subsequence (LIS).

You code should return the length of the LIS.

**Clarification**
What's the definition of longest increasing subsequence?

The longest increasing subsequence problem is to find a subsequence of a given sequence in which the subsequence's elements are in sorted order, lowest to highest, and in which the subsequence is as long as possible. This subsequence is not necessarily contiguous, or unique.

[Longest_increasing_subsequence wiki](https://en.wikipedia.org/wiki/Longest_increasing_subsequence)

**Example**
For `[5, 4, 1, 2, 3]`, the LIS is `[1, 2, 3]`, return 3
For `[4, 2, 4, 5, 3, 7]`, the LIS is `[2, 4, 5, 7]`, return 4



# 解题思路
CLRS中的经典题目
这一题和Jump Game II（动归的解法）有类似的地方，同样都是在求第i个位置的状态时，需要把i前面的所有状态比较一次，把最好的结果保存成当前的结果。而不一样的地方则是在Jump Game II中，对于f[i]这个状态来说，起点和终点时确定的，而LIS中的f[i]起点和终点时不确定的。
观察以下几个例子

* `[1,3,2,5] -> [1,2,5]` : 起点在开头
* `[4,1,2,3] -> [1,2,3]` : 起点在中间
* `[2,3,4,1] -> [2,3,4]` : 终点不是最后一个位置

所以为了得到第i个位置的LIS，只能把前面所有的情况都比较一次，如果前面某个位置的值`nums[j]`小于`nums[i]`并且j这个位置的`f[j]`加上1大于当前`f[i]`的值，则更新`f[i]`，遍历完i前所有的可能性之后，才能得到第i个位置的LIS，数组长度为n的话，总耗费为`1+2+...+(n-1)`，时间复杂度即为`O(n^2)`

## 1. 定义状态
f[i]为第i个位置的LIS

## 2. 定义状态转移方程
```
f[i] = Max(f[0...i-1] + 1, f[i])
```
前提条件为`nums[i] > nums[j]`, j为i前面的索引，取值范围`[0,i-1]`

## 3. 初始化状态
`f[i] = 1`
每个位置的LIS至少为1，既本身。

## 4. 答案
Max(f[0...i-1])

# Code
```java
public class Solution {
    /**
     * @param nums: The integer array
     * @return: The length of LIS (longest increasing subsequence)
     */
    public int longestIncreasingSubsequence(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int max = 1;
        int n = nums.length;
        int[] f = new int[n];
        for (int i = 0; i < n; i++) {
            f[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    f[i] = Math.max(f[i], f[j] + 1);
                }
            }
            max = Math.max(max, f[i]);
        }
        return max;
    }
}
```

# 复杂度分析
时间复杂度为：O(n^2)
空间复杂度为：O(n)

# 优化
有一种二分的方式可以用来优化时间复杂度，这里没有研究。


# 相关问题列表 
* Jump Game II

# Reference 
Leetcode, Lintcode


