---
layout: post
title: Floyd
tag: 搜索与图论
---

# Floyd

用来求多源汇最短路问题

原理是动态规划，代码形式是三重循环

## 关键数据结构

`dist[i][j]`:代表从`i`到`j`最短路

**注意`dist`数组的初始化，`dist[i][i] = 0`**

## AcWing 854. Floyd求最短路   [原题链接](https://www.acwing.com/problem/content/856/)

给定一个n个点m条边的有向图，图中可能存在重边和自环，边权可能为负数。

再给定k个询问，每个询问包含两个整数x和y，表示查询从点x到点y的最短距离，如果路径不存在，则输出“impossible”。

数据保证图中不存在负权回路。

#### 输入格式

第一行包含三个整数n，m，k

接下来m行，每行包含三个整数x，y，z，表示存在一条从点x到点y的有向边，边长为z。

接下来k行，每行包含两个整数x，y，表示询问点x到点y的最短距离。

#### 输出格式

共k行，每行输出一个整数，表示询问的结果，若询问两点间不存在路径，则输出“impossible”。

#### 数据范围

1≤n≤2001≤n≤200,
1≤k≤n21≤k≤n2
1≤m≤200001≤m≤20000,
图中涉及边长绝对值均不超过10000。

#### 输入样例：

```
3 3 2
1 2 1
2 3 2
1 3 1
2 1
1 3
```

#### 输出样例：

```
impossible
1
```

```java
 static int n;
    static int m;
    static int k;
    static int[][] dist;
    public static void main(String[] args) {
        n = in.nextInt();
        m = in.nextInt();
        k = in.nextInt();
        dist = new int[n + 1][n + 1];
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= n; j++) {
                if (i == j) {
                    dist[i][j] = 0;
                } else {
                    dist[i][j] = Integer.MAX_VALUE / 2;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            int a = in.nextInt();
            int b = in.nextInt();
            int v = in.nextInt();
            dist[a][b] = Math.min(dist[a][b], v);
        }
        floyd();
        for (int i = 0; i < k; i++) {
            int a = in.nextInt();
            int b = in.nextInt();
            if (dist[a][b] >= Integer.MAX_VALUE / 4) {
                out.println("impossible");
            }else {
                out.println(dist[a][b]);
            }
        }
        out.flush();
        out.close();
    }
    private static void floyd() {
        for (int k = 1; k < n + 1; k++) {
            for (int i = 1; i < n + 1; i++) {
                for (int j = 1; j < n + 1; j ++)
                    dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
```

