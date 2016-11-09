---
layout: post
title: Longest Common Subsequence 最长公共子序列
categories: [Dynamic Programming]
description: 给出两个字符串，找到最长公共子序列(LCS)，返回LCS的长度。
keywords: LCS, 最大值, two sequence dp, dynamic programming, 动态规划, 动归
---

# Longest Common Subsequence 最长公共子序列
**中文**
给出两个字符串，找到最长公共子序列(LCS)，返回LCS的长度。

**注意事项**
最长公共子序列的定义：

>最长公共子序列问题是在一组序列（通常2个）中找到最长公共子序列（注意：不同于子串，LCS不需要是连续的子串）。该问题是典型的计算机科学问题，是文件差异比较程序的基础，在生物信息学中也有所应用。
[LCS wiki](https://en.wikipedia.org/wiki/Longest_common_subsequence_problem)

**样例**
给出`"ABCD"` 和 `"EDCA"`，这个LCS是 "A" (或 D或C)，返回1

给出 `"ABCD"` 和 `"EACB"`，这个LCS是"AC"返回 2

**ENGLISH**
Given two strings, find the longest common subsequence (LCS).

Your code should return the length of LCS.

**Clarification**
What's the definition of Longest Common Subsequence?

[LCS wiki](https://en.wikipedia.org/wiki/Longest_common_subsequence_problem)

**Example**
For `"ABCD"` and `"EDCA"`, the LCS is "A" (or "D", "C"), return 1.

For `"ABCD"` and `"EACB"`, the LCS is "AC", return 2.



# 解题思路
LCS和LIS一样，也是一道经典的动归题目。首先看一个2个字符串`abcdefg`和`cef`
要想求2个字符串最后一个位置的LCS，我们需要在下面3中情况中取最大值
如果最后一个字符不相等，则我们在下面两种情况中取一个最大值

1. 第一个字符串取最后一个位置和第二个字符串不取最后一个位置，即`abcdefg`和`ce`
2. 第一个字符串不取最后一个位置和第二个字符串取最后一个位置，即`abcdef`和`cef`
3. 如果最后一个字符相等，我们只需要把前面的子串的LCS长度（即`abcdef`和`ce`）加上最后一个最后一个字符是否相等，相等为1，不等为0。

这里有一个问题就是，为什么当两个字符串最后一个字符相等的时候，我们取了最终就一定可以得到LCS了？
比如`abcd`和`bdd`，

* 如果我们两个字符串都不取最后一个d，则剩下的串为`abc`和`bd`，前面构成的LCS一定比取最后一个少1。
* 如果我们只取其中一个，则剩下的串有可能为`abcd`和`bd`和`abc`和`bdd`，这2种情况的LCS最好也是和我们之前结果一样。

## 1. 定义状态
f[i][j]为第一个串前i个位置和第二个串前j个位置的LCS

## 2. 定义状态转移方程
```
f[i][j] = Max{f[i - 1][j], f[i][j - 1], f[i - 1][j - 1] + 最后一个字符是否相等} 
```

## 3. 初始化状态
f[0][0] 为两个串都为空的情况
f[i][0] = 0
f[0][j] = 0
当其中一个串为空时，对应的LCS则为0。

## 4. 答案
f[m][n]
m为第一个串的长度
n为第二个串的长度

# Code
```java
public class Solution {
    /**
     * @param A, B: Two strings.
     * @return: The length of longest common subsequence of A and B.
     */
    public int longestCommonSubsequence(String A, String B) {
        if (A == null || B == null) {
            return 0;
        }
        int m = A.length();
        int n = B.length();
        int[][] f = new int[m + 1][n + 1];
        // initialize 0 on default
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
                int isLastEqual = A.charAt(i - 1) == B.charAt(j - 1) ? 1 : 0;
                f[i][j] = Math.max(f[i][j], f[i - 1][j - 1] + isLastEqual);
            }
        }
        return f[m][n];
    }
}
```

# 复杂度分析
时间复杂度为：O(mn)
空间复杂度为：O(mn)


# 相关问题列表 
* Edit Distance
* Longest Common Substring

# Reference 
[Leetcode](https://leetcode.com/), [Lintcode](http://www.lintcode.com/), [Jiuzhang](www.jiuzhang.com)
