---
layout: post
title: 区间DP
tag: 动态规划
---

# 石子合并

![y6otGF.png](https://s3.ax1x.com/2021/02/15/y6otGF.png)

## AcWing 282. 石子合并   [原题链接](https://www.acwing.com/problem/content/284/)

设有N堆石子排成一排，其编号为1，2，3，…，N。

每堆石子有一定的质量，可以用一个整数来描述，现在要将这N堆石子合并成为一堆。

每次只能合并相邻的两堆，合并的代价为这两堆石子的质量之和，合并后与这两堆石子相邻的石子将和新堆相邻，合并时由于选择的顺序不同，合并的总代价也不相同。

例如有4堆石子分别为 1 3 5 2， 我们可以先合并1、2堆，代价为4，得到4 5 2， 又合并 1，2堆，代价为9，得到9 2 ，再合并得到11，总代价为4+9+11=24；

如果第二步是先合并2，3堆，则代价为7，得到4 7，最后一次合并代价为11，总代价为4+7+11=22。

问题是：找出一种合理的方法，使总的代价最小，输出最小代价。

#### 输入格式

第一行一个数N表示石子的堆数N。

第二行N个数，表示每堆石子的质量(均不超过1000)。

#### 输出格式

输出一个整数，表示最小代价。

#### 数据范围

1≤N≤3001≤N≤300

#### 输入样例：

```
4
1 3 5 2
```

#### 输出样例：

```
22
```

```java
    public static void main(String[] args) {
        int n = in.nextInt();
        int[] arr = new int[n];
        in.nextIntegerArray(arr);
        int[] sum = new int[n + 1];
        for (int i = 1; i < n + 1; i++) sum[i] = sum[i - 1] + arr[i - 1];
        int[][] dp = new int[n + 1][n + 1];
        for (int i = 0; i < n + 1; i++)
            for (int j = 0; j < n + 1; j++) {
                if (i != j) dp[i][j] = Integer.MAX_VALUE;
            }
        for (int len = 1; len < n + 1; len++) {
            for (int start = 1; start + len - 1 < n + 1; start++) {
                int l = start;
                int r = start + len - 1;
                for (int k = l; k < r; k++) {
                    dp[l][r] = Math.min(dp[l][r], dp[l][k] + dp[k + 1][r] + sum[r] - sum[l - 1]);
                }
            }
        }
        out.println(dp[1][n]);
        out.flush();
        out.close();
    }
```

