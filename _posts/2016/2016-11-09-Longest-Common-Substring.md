---
layout: post
title: Longest Common Substring 最长公共子串
categories: [Dynamic Programming]
description: 给出两个字符串，找到最长公共子串，并返回其长度。
keywords: dp, dynamic programming, 动态规划, 动归
---

# Longest Common Substring 最长公共子串
**中文**
给出两个字符串，找到最长公共子串，并返回其长度。

**注意事项**
子串的字符应该连续的出现在原字符串中，这与子序列有所不同。

**样例**
给出A=“ABCD”，B=“CBCE”，返回 2

**ENGLISH**
Given two strings, find the longest common substring.
Return the length of it.

**Notice**
The characters in substring should occur continuously in original string. This is different with subsequence.

**Example**
Given A = "ABCD", B = "CBCE", return 2.

# 解题思路
这题和LCS类似，字串和子序列唯一的不同时，字串必须时连续的。
当间断后得重新计数。看看下面这个例子。

```
"ABCD", "CBCE"

1 1     a       c 0
1 2     a       cb 0
1 3     a       cbc 0
1 4     a       cbce 0
2 1     ab      c 0
2 2     ab      cb 1 + f[1][1] = 1
2 3     ab      cbc 0
2 4     ab      cbce 0
3 1     abc     c 1 + f[3][0] = 1
3 2     abc     cb 0
3 3     abc     cbc 1 + f[2][2] = 2
3 4     abc     cbce 0
4 1     abcd    c 0
4 2     abcd    cb 0
4 3     abcd    cbc 0
4 4     abcd    cbce 0
```

## 1. 定义状态
f[i][j]为第一个串前i个位置和第二个串前j个位置的最长公共子串。

## 2. 定义状态转移方程
```
f[i][j] = f[i - 1][j - 1] 
```
当第1个串当前位置和第2个串当前位置的字符相等时，

## 3. 初始化状态
f[0][0] = 0
f[i][0] = 0
f[0][j] = 0
初始化f时把0,0空出来，长度分别为2个字符串的长度加1。
f[i]对应的原给定字符串的第i-1个位置。

## 4. 答案
max{f[i][j]} f中的最大值。

# Code
```java
public class Solution {
    /**
    * @param A, B: Two string.
    * @return: the length of the longest common substring.
    */
    public int longestCommonSubstring(String A, String B) {
        if (A == null || B == null) {
            return 0;
        }
        int m = A.length();
        int n = B.length();
        int[][] f = new int[m + 1][n + 1];
        // initialize 0 by default
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (A.charAt(i - 1) == B.charAt(j - 1)) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                } else {
                    f[i][j] = 0;
                }
            }
        }
        int max = 0;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                max = Math.max(max, f[i][j]);
            }
        }
        return max;
    }
}
```

# 复杂度分析
时间复杂度为：O(mn)
空间复杂度为：O(mn)


# 相关问题列表 
* Longest Common Subsequence

# Reference 
[Leetcode](https://leetcode.com/), [Lintcode](http://www.lintcode.com/), [Jiuzhang](www.jiuzhang.com)


