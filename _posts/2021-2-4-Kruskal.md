---
layout: post
title: Kruskal
tag: 搜索与图论
---

# Kruskal

适用于稀疏图

## 算法思路

1. 将所有的边排序
2. 依次取出时判断这条边是否是冗余的（即加入这条边会有回路），利用并查集来实现

## AcWing 859. Kruskal算法求最小生成树   [原题链接](https://www.acwing.com/problem/content/861/)

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

1≤n≤1051≤n≤105,
1≤m≤2∗1051≤m≤2∗105,
图中涉及边的边权的绝对值均不超过1000。

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
    static UnionFind unionFind;
    static List<Edge> graph = new ArrayList<>();
    private static class UnionFind {
        int[] parent;

        public UnionFind(int n) {
            parent = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        public boolean union(int x, int y) {
            if (find(x) == find(y)) {
                return false;
            }
            parent[find(x)] = find(y);
            return true;
        }

        public int find(int index) {
            if (parent[index] != index) {
                parent[index] = find(parent[index]);
            }
            return parent[index];
        }
    }
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
        unionFind = new UnionFind(n + 1);
        for (int i = 0; i < m; i++) {
            add(in.nextInt(), in.nextInt(), in.nextInt());
        }
        int res = kruskal();
        if (res == Integer.MAX_VALUE / 2) {
            out.println("impossible");
        }else {
            out.println(res);
        }
        out.flush();
        out.close();
    }
    private static int kruskal() {
        int sum = 0;
        int count = 0;
        graph.sort(Comparator.comparingInt(a -> a.weight));
        for (Edge e : graph) {
            if (unionFind.union(e.start, e.end)) {
                sum += e.weight;
                count++;
            }
        }
        if (count < n - 1) {
            return Integer.MAX_VALUE / 2;
        }
        return sum;
    }
```

