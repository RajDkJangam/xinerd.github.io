---
layout: post
title: Unique Paths 不同的路径 I & II
categories: [Dynamic Programming]
description: 机器人每一时刻只能向下或者向右移动一步，问有多少种方案走到右下角。
keywords: 总方案数, 动态规划, 动归, dp, dynamic programming,
---

# Unique Paths 不同的路径
有一个机器人的位于一个M×N个网格左上角（下图中标记为'Start'）。
机器人每一时刻只能向下或者向右移动一步。机器人试图达到网格的右下角（下图中标记为'Finish'）。
问有多少条不同的路径？

**注意事项**

n和m均不超过100


| 00 | 01 | 02 | 03 |

| 10 | 11 | 12 | 13 |

| 20 | 21 | 22 | 23 |



A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

**Notice**

m and n will be at most 100.


# 解题思路
机器人可以往右或者往下走，所以某一个位置要不就是从这个位置的上方来就是从这个位置的左侧来。
如果我们知道分别有多少种方案可以走到这个位置的上侧和左侧，两种情况相加的总数既是当前这个位置的方案数。
对于第一行和第一列，因为都只有一种可能性，既一直向右或下，所以方案数均为1。

## 1. 定义状态
f[i,j]：在i，j这个位置的总方案数


## 2. 定义状态转移方程
```
f[i, j] = f[i - 1][j] + f[i][j - 1]
```


## 3. 初始化状态
`f[row, 0] = 1`
`f[0, col] = 1`

## 4. 答案
`f[row - 1][col - 1]`

# Code
```java
public class Solution {
    /**
     * @param n, m: positive integer (1 <= n ,m <= 100)
     * @return an integer
     */
    public int uniquePaths(int m, int n) {
        int[][] f = new int [m][n];
        // rows
        for (int i = 0; i < m; i++) {
            f[i][0] = 1;
        }
        // cols
        for (int i = 1; i < n; i++) {
            f[0][i] = 1;
        }
        // transition
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                f[i][j] = f[i - 1][j] + f[i][j - 1];
            }
        }
        return f[m - 1][n - 1];
    }
}
```

# 复杂度分析
时间复杂度为：O(mn)
空间复杂度为：O(mn)

# Follow Up: Unique Paths II 不同的路径II
现在考虑网格中有障碍物，那样将会有多少条不同的路径？
网格中的障碍和空位置分别用 1 和 0 来表示。
 
**注意事项**
m 和 n 均不超过100

如下所示在3x3的网格中有一个障碍物：

```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```

一共有2条不同的路径从左上角到右下角。

# 解题思路
这一题唯一的区别就是在给定的路径中出现了障碍物，如果碰到了障碍物则不能继续前行。
所以当我们在计算当前位置有多少种可行的路径方案时，需要先判断当前这个位置是否有障碍物，如果是障碍物，则把当前位置的方案数置0，表示没有路径可达。否则和之前的求方案数方式一致。
另外还需要**注意**的是在初始化第一行和第一列的时候，一旦碰到障碍物，除了当前位置置0以外，后面所有的位置也都是0，因为后面的位置只有可能由前面的唯一的一条路径而来。

```java
public class Solution {
    /**
     * @param obstacleGrid: A list of lists of integers
     * @return: An integer
     */
    public int uniquePathsWithObstacles(int[][] grid) {
        if (grid == null || grid.length == 0) {
            return -1;
        }
        int row = grid.length;
        int col = grid[0].length;
        int[][] f = new int[row][col];
        // initialize row
        for (int i = 0; i < row; i++) {
            if (grid[i][0] == 1) {
                break;
            }
            f[i][0] = 1;
        }
        // col
        for (int j = 0; j < col; j++) {
            if (grid[0][j] == 1) {
                break;
            }
            f[0][j] = 1;
        }
        // transition
        for (int i = 1; i < row; i++) {
            for (int j = 1; j < col; j++) {
                if (grid[i][j] == 1) {
                    f[i][j] = 0;
                    continue;
                }
                f[i][j] = f[i - 1][j] + f[i][j - 1];
            }
        }
        return f[row - 1][col - 1];
    }
}
```

# Challenge 优化空间
当前空间复杂度为：O(mn)
通过滚动数组来优化后空间复杂度为：O(m+n)


# 相关问题列表 
* Unique Paths I, II
* Clibing Stairs I, II

# Reference 
Leetcode, Lintcode


