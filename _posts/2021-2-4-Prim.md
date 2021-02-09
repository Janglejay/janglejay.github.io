---
layout: post
title: Prim
tag: 搜索与图论
---

# Prim

适用于无向稠密图，可以存在负边

不需要初始化`dist[1] = 0`，因为`dist[i]`表示点`i`到整个集合的距离，应该存这个点到集合最短边的值

任选一点作为开始都行

## 算法思路

1. `dist[n] = Integer.MAX_VALUE / 2`
2. 进行`n`次迭代，找到没有在集合中（及已经确定的最小连通块）的距离最小的点`t`
3. 用点`t`来更新其他点到集合的距离
4. `st[t] = true`,表示这个点已经确定了，并将`t`放入到集合中

## AcWing 858. Prim算法求最小生成树   [原题链接](https://www.acwing.com/problem/content/860/)

给定一个n个点m条边的无向图，图中可能存在重边和自环，边权可能为负数。

求最小生成树的树边权重之和，如果最小生成树不存在则输出impossible。

给定一张边带权的无向图G=(V, E)，其中V表示图中点的集合，E表示图中边的集合，n=|V|，m=|E|。

由V中的全部n个顶点和E中n-1条边构成的无向连通子图被称为G的一棵生成树，其中边的权值之和最小的生成树被称为无向图G的最小生成树。

#### 输入格式

第一行包含两个整数n和m。

接下来m行，每行包含三个整数u，v，w，表示点u和点v之间存在一条权值为w的边。

#### 输出格式

共一行，若存在最小生成树，则输出一个整数，表示最小生成树的树边权重之和，如果最小生成树不存在则输出impossible。

#### 数据范围

1≤n≤5001≤n≤500,
1≤m≤1051≤m≤105,
图中涉及边的边权的绝对值均不超过10000。

#### 输入样例：

```
4 5
1 2 1
1 3 2
1 4 3
2 3 2
3 4 4
```

#### 输出样例：

```
6
```

```java
	static int n;
    static int m;
    static int[][] graph;
    static int[] dist;
    static boolean[] st;
    public static void main(String[] args) {
        n = in.nextInt();
        m = in.nextInt();
        graph = new int[n + 1][n + 1];
        dist = new int[n + 1];
        st = new boolean[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE / 2);
        for (int i = 0; i < n + 1; i++) Arrays.fill(graph[i], Integer.MAX_VALUE / 2);
        for (int i = 0; i < m; i++) {
            int a = in.nextInt();
            int b = in.nextInt();
            int v = in.nextInt();
            //无向图存储
            graph[a][b] = graph[b][a] = Math.min(graph[a][b], v);
        }
        int res = prim();
        if (res == Integer.MAX_VALUE / 2) {
            out.println("impossible");
        }else {
            out.println(res);
        }
        out.flush();
        out.close();
    }
    private static int prim() {
        int sum = 0;
        for (int i = 1; i < n + 1; i++) {
            int t = -1;
            for (int j = 1; j < n + 1; j++) {
                if (!st[j] && (t == -1 || dist[t] > dist[j])) {
                    t = j;
                }
            }
            st[t] = true;
            //第一条边dist还是MAX
            if (i != 1) {
                if (dist[t] == Integer.MAX_VALUE / 2) {
                    return Integer.MAX_VALUE / 2;
                }
                sum += dist[t];
            }
            for (int j = 1; j < n + 1; j++) {
                //边长，就是点到集合的距离
                dist[j] = Math.min(dist[j], graph[j][t]);
            }
        }
        return sum;
    }
```

