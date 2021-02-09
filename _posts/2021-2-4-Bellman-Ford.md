---
layout: post
title: Bellman-Ford
tag: 搜索与图论
---

# Bellman-Ford

1. 存储所有的边
2. 如果有`负权回路`则答案可能是负无穷
   1. 可以用来判断`负权回路`是否存在，不过一般用`SPFA`来做
3. 循环`k`次，表示不经过超过`k`条边
4. 每次循环要进行备份，否则可能发生串联（即用更新后的结果更新下个点），在一次完整的更新过程中只能用原来的结果来迭代后面的结果
5. 每次循环所有的边找最短的
6. 要注意最后能否到达的判断，因为存在`负权边`所以可能使不可到达的距离不为`Integer.MAX_VALUE / 2`，所以我们使用`>= Integer.MAX_VALUE / 4`来判定不可达

## AcWing 853. 有边数限制的最短路   [原题链接](https://www.acwing.com/problem/content/855/)

给定一个n个点m条边的有向图，图中可能存在重边和自环， **边权可能为负数**。

请你求出从1号点到n号点的最多经过k条边的最短距离，如果无法从1号点走到n号点，输出impossible。

注意：图中可能 **存在负权回路** 。

#### 输入格式

第一行包含三个整数n，m，k。

接下来m行，每行包含三个整数x，y，z，表示存在一条从点x到点y的有向边，边长为z。

#### 输出格式

输出一个整数，表示从1号点到n号点的最多经过k条边的最短距离。

如果不存在满足条件的路径，则输出“impossible”。

#### 数据范围

1≤n,k≤5001≤n,k≤500,
1≤m≤100001≤m≤10000,
任意边长的绝对值不超过10000。

#### 输入样例：

```
3 3 1
1 2 1
2 3 1
1 3 3
```

#### 输出样例：

```
3
```

```java
static int n;
    static int k;
    static int m;
    static int[] dist;
    static List<Edge> graph = new ArrayList<>();
    private static class Edge{
        private int start;
        private int end;
        private int weight;

        public Edge(int start, int end, int weight) {
            this.start = start;
            this.end = end;
            this.weight = weight;
        }
    }
    private static void add(int a, int b, int v) {
        graph.add(new Edge(a, b, v));
    }
    public static void main(String[] args) {
        n = in.nextInt();
        m = in.nextInt();
        k = in.nextInt();
        dist = new int[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE / 2);
        for (int i = 0; i < m; i++) {
            add(in.nextInt(), in.nextInt(), in.nextInt());
        }
        int res = bellmanFord();
        if (res == -1) {
            out.println("impossible");
        }else {
            out.println(res);
        }
        out.flush();
        out.close();
    }
    private static int bellmanFord() {
        dist[1] = 0;
        for (int i = 0; i < k; i++) {
            int[] bak = new int[n + 1];
            System.arraycopy(dist, 0, bak, 0, n + 1);
            for (Edge e : graph) {
                dist[e.end] = Math.min(dist[e.end], bak[e.start] + e.weight);
            }
        }
        if (dist[n] >= Integer.MAX_VALUE / 4) {
            return -1;
        }
        return dist[n];
    }
```

