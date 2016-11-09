---
layout: post
title: Palindrome Partitioning II 分割回文串 II
categories: [Dynamic Programming]
description: some word here
keywords: 最小值, 单序列dp, dynamic programming, 动态规划, 动归
---

# Palindrome Partitioning II 分割回文串 II 
**中文**
给定一个字符串s，将s分割成一些子串，使每个子串都是回文。

返回s符合要求的的最少分割次数。

**样例**
比如，给出字符串`s = "aab"`，

返回 1， 因为进行一次分割可以将字符串s分割成`["aa","b"]`这样两个回文子串

**ENGLISH**
Given a string s, cut s into some substrings such that every substring is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

**Example**
Given `s = "aab"`,

Return 1 since the palindrome partitioning `["aa", "b"]` could be produced using 1 cut.

# 解题思路
这一题跟Palindrome Partitioning I不同，不是求所有方案，还是求一个最小的cut数，求最优值自然想到dp来解决。

对于一个给定的字符串，当我们要去找到最后一个位置的最小切割数时，我们需要去查询它和前面的字符组成的所有可能性，最后比较得出最小的切割机数。

在定义状态的时候，如果我们把f[i]定义成前i个字符中需要的最小切割数，那么f[1]=0代表1个字符的时候不需要切分,f[0]不好直观的初始化。
这里我们可以把f[i]初始化为前i个字符中出现的最小回文串的个数，那么这个个数减1就是切割数了，而f[0]代表空串没有回文串，f[1]代表1个字符串本身就是最小的回文串。

看一个例子， 前面的状态比较简单就不扩展了，主要看后几个状态。

abbacc

f[0] = 0 , cut -1 空串不能切割
f[1] = a , cut 0 一个字符本身时最小的回文串，不需要切割
f[2] = ab, cut 1 切后为 a b
f[3] = abb, cut 的具体过程为

```
a bb, bb是回文数, 更新f[3] = f[1] + 1 = 1
ab b, b是回文数, 更新f[3] = f[2] + 1 = 2 比f[3]保存的值大, 最终不更新
```

f[4] = abba, cut 0 
f[5] = abbac, cut 的具体过程为

```
a bbac 
ab bac
abb ac
abba c, c是回文数，更新f[6] = f[5] + 1 = 2
```

f[6] = abbacc, cut 的具体过程为

```
a bbacc 
ab bacc
abb acc
abba cc, cc是回文数，更新f[6] = f[4] + 1 = 1
abbac c, c是回文数，更新f[6] = f[5] + 1 = 2, 比f[6]保存的值大，最终不更新
```

所以f[6]的最小值是1

## 1. 定义状态
f[i]为前i个字符能构成的最小回文串个数


## 2. 定义状态转移方程
```
f[i] = Min{f[j]}
```
前提条件是从j到i所构成的子串是回文串
然后去前面这些满足条件的`f[j]+1`的最小值。

## 3. 初始化状态
f[0] = 0
f[1] = 1

## 4. 答案
f[n] - 1
比如一个字符串中存在3个回文串，那么需要的切割数则是3-1

# Code
```java
public class Solution {
    /**
     * @param s a string
     * @return an integer
     */
    public int minCut(String s) {
        int n = s.length();
        int[] f = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            f[i] = i;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = i - 1; j >= 0; j--) {
                if (isPalindrome(s.substring(j, i))){
                    f[i] = Math.min(f[i], f[j] + 1);
                }    
            }
        }
        return f[n] - 1;
    }
    private boolean isPalindrome(String s) {
        for (int i = 0, j = s.length() - 1; i < j; i++, j--) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
        }
        return true;
    }
}
```

# 复杂度分析
时间复杂度为：`O(n^3)`
这里搜索子串的复杂度为`O(n^2)`,同时在内循环中判断是否是回文串又需要消耗`O(n)`,所以一共需要`O(n^3)`

空间复杂度为：O(n)

# Followup O(n^2) time
通过花费O(n^2)的时间和空间来建立一个查询是否为回文串的isP[i][j]二维数组，代表从i到j构成的子串是否为回文串， 使得查询是消耗O(1)的时间。

# 优化后的代码
```java
public class Solution {
    /**
     * @param s a string
     * @return an integer
     */
    public int minCut(String s) {
       int n = s.length();
       // prepare
       boolean[][] isP= getIsPalindrome(s);
       // transition function
       int[] f = new int[n + 1];
       for (int i = 0; i <= n; i++) {
           f[i] = i;
       }
       for (int i = 1; i <= n; i++) {
           for (int j = 0; j < i; j++) {
               if (isP[j][i - 1]) {
                   f[i] = Math.min(f[i], f[j] + 1);
               }
           }
       }
       return f[n] - 1;
    }
    private boolean[][] getIsPalindrome(String s) {
        int n = s.length();
        boolean [][] isP = new boolean[n][n];
        // for one letter
        for (int i = 0; i < n; i++) {
            isP[i][i] = true;
        }
        // for adjacent letters
        for (int i = 1; i < n; i++) {
            isP[i-1][i] = s.charAt(i - 1) == s.charAt(i);
        }
        for (int l = 2; l < n; l++) {
            for (int start = 0; start + l < n; start++) {
                 isP[start][start + l] = isP[start + 1][start + l - 1] 
                    && s.charAt(start) == s.charAt(start + l); 
            }
        }
        return isP;
    }
};
```

# 优化后的复杂度
时间复杂度为：`O(n^2)`
空间复杂度为：`O(n^2)`
典型的时间空间上的tradeoff

* TODO
[O(n) space](https://oj.leetcode.com/discuss/9476/solution-does-not-need-table-palindrome-right-uses-only-space) 

# 相关问题列表 
* Palindrome Partitioning
* Longest Palindromic Substring
* Wiggle Sort II

# Reference 
[Leetcode](https://leetcode.com/), [Lintcode](http://www.lintcode.com/), [Jiuzhang](www.jiuzhang.com)


