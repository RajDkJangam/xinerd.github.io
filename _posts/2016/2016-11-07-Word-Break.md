---
layout: post
title: Word Break 单词切分
categories: [Dynamic Programming, 动态规划]
description: 给出一个字符串s和一个词典，判断字符串s是否可以被空格切分成一个或多个出现在字典中的单词。
keywords: 单序列dp, dynamic programming, 动态规划, 动归
---

# Word Break 单词切分
**中文**
给出一个字符串s和一个词典，判断字符串s是否可以被空格切分成一个或多个出现在字典中的单词。

**样例**
给出`s = "lintcode"`, `dict = ["lint","code"]`

返回 true 因为"lintcode"可以被空格切分成"lint code"

**ENGLISH**
Given a string s and a dictionary of words dict, determine if s can be break into a space-separated sequence of one or more dictionary words.

**Example**
Given `s = "lintcode"`, `dict = ["lint", "code"]`.

Return true because "lintcode" can be break as "lint code".

# 解题思路
为了判断给定字符串是否能够切分成字典中的单词，我们可以从最后一个字符倒着向前搜索，比如lint，
第一次：最后一个字符是t，前面的字符为lin，前后都不能在字典中查找到
第二次：nt， 前面为li
第三次：int，前面为l
第四次：lint，前面为空串（默认为true）， 前后都为true， 则t在的位置可以标记为true，代表可以切分成字典中的单词。

对于lintcode也是一样的道理，
第一次：e，前面为lintcod
。。。
第四次：code，前面为lint（已经标记为true了），前后都为true

## 1. 定义状态
f[i]为前i个字符是否能切分成字典中的词

## 2. 定义状态转移方程
```
f[i] = {f[j]} && dict.contains(s.substring(j, i))
```
其中j的取值范围为0到i-1，意思是f[j]要可以切分成字典中的单词，并且同时从[j到i)也可以切分为字典中的单词，则f[i]为true。

## 3. 初始化状态
f[0] = true
空串默认置空。

## 4. 答案
f[n] n为字符串长度。

# Code
```java
public class Solution {
    /**
     * @param s:    A string s
     * @param dict: A dictionary of words dict
     */
    public boolean wordBreak(String s, Set<String> dict) {
        // lintcode
        
        // in order to optimize time complexity
        int max = maxLength(dict);
        int n = s.length();
        boolean[] f = new boolean[n + 1];
        f[0] = true;
        for (int i = 1; i <= n; i++) {
            for (int j = i - 1; j >= Math.max(i - max, 0); j--) {
                if (f[j] && dict.contains(s.substring(j, i))){
                    f[i] = true;
                    break;
                }
            }
        }
        return f[n];
    }
    private int maxLength(Set<String> dict) {
        int max = 0;
        for (String s : dict) {
            if (max < s.length()) {
                max = s.length();
            }
        }
        return max;
    }
}
```

上面代码进行了一次小优化，从i个位置搜索前面的词的时候默认只搜索到i-max的位置，max是字典中最长单词的长度。因为如果这个长度下在字典中都搜不到单词的话，继续往前面搜索已经没有意义了，因为字典中不存在更长的单词。

# 复杂度分析
时间复杂度为：O(n^2)
空间复杂度为：O(n)

# 相关问题列表 
* Word Break II

# Reference 
[Leetcode](https://leetcode.com/), [Lintcode](http://www.lintcode.com/), [Jiuzhang](www.jiuzhang.com)


