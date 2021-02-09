---
layout: post
title: Dijkstra
tag: 搜索与图论
---

# Dijkstra

# 朴素版Dijkstra

适用于稠密图，用`邻接矩阵`来存储图

对于重边只需要存储最小的那一条

1. 将开始点的`dist[0] = 0 others dist[n] = Integer.MAX_VALUE / 2`
2. 遍历所有点：找到未确定最短距离的点中距离最短的点
3. 利用这个点更新与之相连的点到起点的最短路径

## 关键数据结构

`st[]`:表示第`i`个点已经确定了最短路，及要以这个点为基准来更新其他路径，而这个点的最短路已经确定。

`dist[]`:表示第`i`个点到起点的最短路

# 堆优化版Dijkstra

适用于稀疏图，利用`邻接表`来存储图

1. 使用`heap`来维护最小的路径

## 关键数据结构

`heap`:用来维护最小的路径

`st[]`:表示这个点已经确定了最短路径

`dist[]`：表示这个点到起点的最短路径

`Pair`：表示某个点，利用这个点的`weight`进行排序，利用这个点的`position`访问原位置

## AcWing 849. Dijkstra求最短路 I   [原题链接](https://www.acwing.com/problem/content/851/)

给定一个n个点m条边的有向图，图中可能存在重边和自环，所有边权均为正值。

请你求出1号点到n号点的最短距离，如果无法从1号点走到n号点，则输出-1。

#### 输入格式

第一行包含整数n和m。

接下来m行每行包含三个整数x，y，z，表示存在一条从点x到点y的有向边，边长为z。

#### 输出格式

输出一个整数，表示1号点到n号点的最短距离。

如果路径不存在，则输出-1。

#### 数据范围

1≤n≤5001≤n≤500,
1≤m≤1051≤m≤105,
图中涉及边长均不超过10000。

#### 输入样例：

```
3 3
1 2 2
2 3 1
1 3 4
```

#### 输出样例：

```
3
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
            graph[a][b] = Math.min(graph[a][b], v);
        }
        int res = dijkstra();
        out.println(res);
        out.flush();
        out.close();
    }

    private static int dijkstra() {
        dist[1] = 0;
        for (int i = 1; i < n + 1; i++) {
            int idx = -1;
            for (int j = 1; j < n + 1; j++) {
                if (!st[j] && (idx == -1 || dist[idx] > dist[j])) {
                    idx = j;
                }
            }
            st[idx] = true;
            for (int j = 1; j < n + 1; j++) {
                dist[j] = Math.min(dist[j], dist[idx] + graph[idx][j]);
            }
        }
        if (dist[n] == Integer.MAX_VALUE / 2) {
            return -1;
        }
        return dist[n];
    }
```

## AcWing 850. Dijkstra求最短路 II   [原题链接](https://www.acwing.com/problem/content/852/)

给定一个n个点m条边的有向图，图中可能存在重边和自环，所有边权均为非负值。

请你求出1号点到n号点的最短距离，如果无法从1号点走到n号点，则输出-1。

#### 输入格式

第一行包含整数n和m。

接下来m行每行包含三个整数x，y，z，表示存在一条从点x到点y的有向边，边长为z。

#### 输出格式

输出一个整数，表示1号点到n号点的最短距离。

如果路径不存在，则输出-1。

#### 数据范围

1≤n,m≤1.5×1051≤n,m≤1.5×105,
图中涉及边长均不小于0，且不超过10000。

#### 输入样例：

```
3 3
1 2 2
2 3 1
1 3 4
```

#### 输出样例：

```
3
```

```java
static int n;
    static int m;
    static List<List<Pair>> graph = new ArrayList<>();
    static int[] dist;
    static boolean[] st;
    private static class Pair{
        private int weight;
        private int position;

        public Pair(int weight, int position) {
            this.weight = weight;
            this.position = position;
        }
    }
    private static void add(int a, int b, int v) {
        graph.get(a).add(new Pair(v, b));
    }
    public static void main(String[] args) {
        n = in.nextInt();
        m = in.nextInt();
        for (int i = 0; i < n + 1; i++) graph.add(new ArrayList<>());
        dist = new int[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE / 2);
        st = new boolean[n + 1];
        for (int i = 0; i < m; i++) add(in.nextInt(), in.nextInt(), in.nextInt());
        int res = dijkstra();
        out.println(res);
        out.flush();
        out.close();
    }
    private static int dijkstra() {
        PriorityQueue<Pair> heap = new PriorityQueue<>(Comparator.comparingInt(a -> a.weight));
        heap.add(new Pair(0, 1));
        dist[1] = 0;
        while (!heap.isEmpty()) {
            Pair p = heap.poll();
            if (st[p.position]) continue;
            st[p.position] = true;
            List<Pair> list = graph.get(p.position);
            for (Pair pp : list) {
                if (dist[pp.position] > dist[p.position] + pp.weight) {
                    dist[pp.position] = dist[p.position] + pp.weight;
                    pp.weight = dist[p.position] + pp.weight;
                    heap.offer(pp);
                }
            }
        }
        if (dist[n] == Integer.MAX_VALUE / 2) return -1;
        return dist[n];
    }
```

