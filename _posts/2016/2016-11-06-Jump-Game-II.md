---
layout: post
title: Jump Game II 跳跃游戏 II
categories: [Greedy, Dynamic Programming]
description: 给出一个非负整数数组，你最初定位在数组的第一个位置。数组中的每个元素代表你在那个位置可以跳跃的最大长度。你的目标是使用最少的跳跃次数到达数组的最后一个位置。
keywords: 最小值, 贪心, 动态规划, 动归, dp, dynamic programming
---

# Jump Game II 跳跃游戏 II 
**中文**
给出一个非负整数数组，你最初定位在数组的第一个位置。

数组中的每个元素代表你在那个位置可以跳跃的最大长度。　　　

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

**注意事项**

**样例**
给出数组`A = [2,3,1,1,4]`，最少到达数组最后一个位置的跳跃次数是2(从数组下标0跳一步到数组下标1，然后跳3步到数组的最后一个位置，一共跳跃2次)

**ENGLISH**
Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

**Example**
Given array `A = [2,3,1,1,4]`

The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index.)


# 解题思路
求最小值最大值就是动归擅长做的事情，所以看到题后自然而然的往dp开始思考。
某一个状态是否是通过最少步数跳跃过来的就依赖于它的上一步是否是通过最少步数跳过去的。
如果之前走过的地方都是通过最少步数到达的，那么这个位置的消耗也一定是最少的，假如前面有一个位置的消耗不是最少的，那么就跟我们定义的状态矛盾了，则应该更新为最少步数。

## 1. 定义状态
f[i]为在第i个位置，所需要消耗的最少的步数

## 2. 定义状态转移方程
```
f[i] = f[j] + 1 
```
其中f[j]为从0到i-1中自身消耗最少，并且能通过那个位置直接到达i的位置。

## 3. 初始化状态
f[0] = 0

默认在第一个位置，所以没有消耗。

## 4. 答案
`f[n-1]` 
n为数组长度

# Code
```java
public class Solution {
    /**
     * DP
     * @param A: A list of lists of integers
     * @return: An integer
     */
    public int jump(int[] A) {
        if (A == null || A.length == 0) {
            return -1;
        }
        int n = A.length;
        int[] f = new int[n];
        f[0] = 0;
        for (int i = 1; i < n; i++) {
            f[i] = Integer.MAX_VALUE;
            for (int j = i - 1; j >= 0; j--) {
                if (f[j] != Integer.MAX_VALUE && j + A[j] >= i) {
                    f[i] = Math.min(f[i], f[j] + 1);
                }
            }
        }
        return f[n - 1];
    }
}
```

代表j这个位置要可以到达i，然后i前面的当前这个f[j]本身要可以到达，最后f[i]和之前所有能到达该位置且自身也可达的j打擂台，把最小步数存到f[i]中去。

# 复杂度分析
时间复杂度为：O(n^2)
空间复杂度为：O(n)

# Challenge 时间复杂度O(n)
和Jump Game I 一样，时间的主要消耗在于，为了找出到达第i个位置的最小步数，每次都必须把i前面所有的元素都重新遍历一次，假如每个元素都只用遍历一次的话，就降低了时间复杂度。
在Jump Game I当中，我们遍历一次数组，就能知道最远能到达的位置，如果这个位置大于数组长度-1，则代表可以跳到最后一个元素。
在这个题目中，思路也非常类似，如果我们每次扫描得到最远的距离后，下一次扫描就没有必要再去已经扫描过的元素了，因为已经扫描过的元素能到达最远的距离我们已经知道了。

所以该题的算法如下
1. 用start和end代表我们当前这一次准备扫描的区间，maxDistance初始化第1个元素能去的最远的位置
2. 每次扫描一次区间时，我们更新该区间内的元素能带我们去到最远的地方maxDistance
3. 然后把start更新到end+1的位置，end更新到最远的位置，这样就保证了我们不会在扫描同一个区间/元素。
4. 直到end超出了我们需要到达的数组长度减1时则退出。
5. 注意！如果前面某一个元素已经不能到达，后面的元素也必然不能到达，则通过返回最大值来表示不可达。这种不可达的状态会使得start的值大于end, 因为start每次循环都更新为end加1，而end只有在start加上A[start]的值大于maxDistance的时候才会更新为更大的值，否则保持不变。

# 优化后代码
```java
public class Solution {
    /**
     * Greedy
     * @param A: A list of lists of integers
     * @return: An integer
     */
    public int jump(int[] A) {
        if (A == null || A.length == 0) {
            return Integer.MAX_VALUE;
        }
        int start = 0;
        int end = 0;
        int steps = 0;
        int maxDistance = A[0];
        while (end < A.length - 1) {
            if (start > end) { // can't reach out further
                return Integer.MAX_VALUE;
            }
            steps++;
            while (start <= end) {
                if (start + A[start] > maxDistance) {
                    maxDistance = start + A[start];
                }
                start++;
            }
            start = end + 1;
            end = maxDistance;
        }
        return steps;
    }
}
```

# 优化后复杂度分析
时间复杂度为：O(n)
空间复杂度为：O(1)

# 小练习
下方是九章提供的贪心答案，看看有什么问题。

```java
public class Solution {
    public int jump(int[] A) {
        if (A == null || A.length == 0) {
            return -1;
        }
        int start = 0, end = 0, jumps = 0;
        while (end < A.length - 1) {
            jumps++;
            int farthest = end;
            for (int i = start; i <= end; i++) {
                if (A[i] + i > farthest) {
                    farthest = A[i] + i;
                }
            }
            start = end + 1;
            end = farthest;
        }
        return jumps;
    }
}
```

test case : `[3,2,1,0,4,4]`



# 相关问题列表 
* Jump Game I

# Reference 
Leetcode, Lintcode


