---
layout: post
title: 状态压缩DP
tag: 动态规划
---

## AcWing 291. 蒙德里安的梦想   [原题链接](https://www.acwing.com/problem/content/293/)

求把N*M的棋盘分割成若干个1*2的的长方形，有多少种方案。

例如当N=2，M=4时，共有5种方案。当N=2，M=3时，共有3种方案。

如下图所示：

![ygu8sO.png](https://s3.ax1x.com/2021/02/16/ygu8sO.png)

#### 输入格式

输入包含多组测试用例。

每组测试用例占一行，包含两个整数N和M。

当输入用例N=0，M=0时，表示输入终止，且该用例无需处理。

#### 输出格式

每个测试用例输出一个结果，每个结果占一行。  

#### 数据范围

1≤N,M≤111≤N,M≤11

#### 输入样例：

```
1 2
1 3
1 4
2 2
2 3
2 4
2 11
4 11
0 0
```

#### 输出样例：

```
1
0
1
2
3
5
144
51205
```

## 题目思路

![yguYee.png](https://s3.ax1x.com/2021/02/16/yguYee.png)

```java
    public static void main(String[] args) {
        int n;
        int m;
        long[][] dp;
        boolean[] st;
        while ((n = in.nextInt()) != 0 && (m = in.nextInt()) != 0) {
            dp = new long[m + 1][1 << n];
            st = new boolean[1 << n];
            for (int i = 0; i < (1 << n); i++) {
                int count = 0;
                boolean isValid = true;
                for (int j = 0; j < n; j++) {
                    if (((i >> j) & 1) == 1) {
                        if ((count & 1) == 1) {
                            isValid = false;
                            break;
                        }
                        count = 0;
                    }else {
                        count++;
                    }
                }
                if ((count & 1) == 1) isValid = false;
                st[i] = isValid;
            }
            dp[0][0] = 1;
            for (int i = 1; i <= m; i++) {
                for (int j = 0; j < (1 << n); j++) {
                    for (int k = 0; k < (1 << n); k++) {
                        if ((j & k) == 0 && st[j | k]) {
                            dp[i][j] += dp[i - 1][k];
                        }
                    }
                }
            }
            out.println(dp[m][0]);
        }
        out.flush();
        out.close();
    }
```

## AcWing 91. 最短Hamilton路径   [原题链接](https://www.acwing.com/problem/content/93/)

给定一张 nn 个点的带权无向图，点从 0~n-1 标号，求起点 0 到终点 n-1 的最短Hamilton路径。 Hamilton路径的定义是从 0 到 n-1 不重不漏地经过每个点恰好一次。

#### 输入格式

第一行输入整数nn。

接下来nn行每行nn个整数，其中第ii行第jj个整数表示点ii到jj的距离（记为a[i,j]）。

对于任意的x,y,zx,y,z，数据保证 a[x,x]=0，a[x,y]=a[y,x] 并且 a[x,y]+a[y,z]>=a[x,z]。

#### 输出格式

输出一个整数，表示最短Hamilton路径的长度。

#### 数据范围

1≤n≤201≤n≤20
0≤a[i,j]≤1070≤a[i,j]≤107

#### 输入样例：

```
5
0 2 4 5 1
2 0 6 5 3
4 6 0 8 3
5 5 8 0 5
1 3 3 5 0
```

#### 输出样例：

```
18
```

## 题目思路

![yguNod.png](https://s3.ax1x.com/2021/02/16/yguNod.png)

```java
    public static void main(String[] args) {
        int n = in.nextInt();
        int[][] arr = new int[n][n];
        for (int i = 0; i < n; i++) in.nextIntegerArray(arr[i]);
        int[][] dp = new int[1 << n][n];
        for (int i = 0; i < 1 << n; i++) Arrays.fill(dp[i], Integer.MAX_VALUE / 2);
        dp[1][0] = 0;
        for (int i = 0; i < 1 << n; i++) {
            for (int j = 0; j < n; j++) {
                if ((i >> j & 1) == 1) {
                    for (int k = 0; k < n; k++) {
                        if ((((i - (1 << j)) >> k) & 1) == 1) {
                            dp[i][j] = Math.min(dp[i][j], dp[i - (1 << j)][k] + arr[k][j]);
                        }
                    }
                }
            }
        }
        out.println(dp[(1 << n) - 1][n - 1]);
        out.flush();
        out.close();
    }
```

