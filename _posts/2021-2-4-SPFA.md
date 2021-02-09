---
layout: post
title: SPFA
tag: 搜索与图论
---

# SPFA

形式上很像`Dijkstra`，是`Bellman-ford`算法的优化

利用宽搜的方式来优化，定义一个队列`queue`,其中存的是变化过的点，如果一个点没有变化（即没有变小）就不会入队。

可以利用这个算法来判断是否存在`负环`

- 维护一个`count`数组，记录边数，当`count[i] >= n`时，说明有`n`条边，经过了`n + 1`个点，所以存在`负环`



## AcWing 851. spfa求最短路   [原题链接](https://www.acwing.com/problem/content/853/)

给定一个n个点m条边的有向图，图中可能存在重边和自环， **边权可能为负数**。

请你求出1号点到n号点的最短距离，如果无法从1号点走到n号点，则输出impossible。

数据保证不存在负权回路。

#### 输入格式

第一行包含整数n和m。

接下来m行每行包含三个整数x，y，z，表示存在一条从点x到点y的有向边，边长为z。

#### 输出格式

输出一个整数，表示1号点到n号点的最短距离。

如果路径不存在，则输出”impossible”。

#### 数据范围

1≤n,m≤1051≤n,m≤105,
图中涉及边长绝对值均不超过10000。

#### 输入样例：

```
3 3
1 2 5
2 3 -3
1 3 4
```

#### 输出样例：

```
2
```

```java
static int n;
    static int m;
    static int[] dist;
    static boolean[] inQueue;
    static List<List<Pair>> graph = new ArrayList<>();
    private static void add(int a, int b, int v) {
        graph.get(a).add(new Pair(b, v));
    }
    private static class Pair{
        private int position;
        private int weight;

        public Pair(int position, int weight) {
            this.position = position;
            this.weight = weight;
        }
    }
    public static void main(String[] args) {
        n = in.nextInt();
        m = in.nextInt();
        dist = new int[n + 1];
        inQueue = new boolean[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE / 2);
        for (int i = 0; i < n + 1; i++) graph.add(new ArrayList<>());
        for (int i = 0; i < m; i++) {
            add(in.nextInt(), in.nextInt(), in.nextInt());
        }
        int res = spfa();
        if (res == -1) {
            out.println("impossible");
        }else {
            out.println(res);
        }
        out.flush();
        out.close();
    }
    private static int spfa() {
        dist[1] = 0;
        Queue<Pair> queue = new ArrayDeque<>();
        queue.add(new Pair(1, 0));
        while (!queue.isEmpty()) {
            Pair p = queue.poll();
            inQueue[p.position] = false;
            List<Pair> list = graph.get(p.position);
            for (Pair pp : list) {
                if (dist[pp.position] > dist[p.position] + pp.weight) {
                    dist[pp.position] = dist[p.position] + pp.weight;
                    queue.add(pp);
                    inQueue[pp.position] = true;
                }
            }
        }
        if (dist[n] >= Integer.MAX_VALUE / 4) return -1;
        return dist[n];
    }
```

## AcWing 852. spfa判断负环   [原题链接](https://www.acwing.com/problem/content/854/)

给定一个n个点m条边的有向图，图中可能存在重边和自环， **边权可能为负数**。

请你判断图中是否存在负权回路。

#### 输入格式

第一行包含整数n和m。

接下来m行每行包含三个整数x，y，z，表示存在一条从点x到点y的有向边，边长为z。

#### 输出格式

如果图中**存在**负权回路，则输出“Yes”，否则输出“No”。

#### 数据范围

1≤n≤20001≤n≤2000,
1≤m≤100001≤m≤10000,
图中涉及边长绝对值均不超过10000。

#### 输入样例：

```
3 3
1 2 -1
2 3 4
3 1 -4
```

#### 输出样例：

```
Yes
```

```java
static int n;
    static int[] dist;
    static int[] count;
    static boolean[] isInQueue;
    static List<List<Pair>> graph = new ArrayList<>();

    static class Pair {
        int position;
        int weight;

        public Pair(int position, int weight) {
            this.position = position;
            this.weight = weight;
        }
    }

    private static void add(int a, int b, int c) {
        graph.get(a).add(new Pair(b, c));
    }

    private static boolean spfa() {
        Queue<Pair> queue = new ArrayDeque<>();
        for (int i = 1; i <= n; i++)
            queue.add(new Pair(i, 0));
        while (!queue.isEmpty()) {
            Pair p = queue.poll();
            isInQueue[p.position] = false;
            List<Pair> list = graph.get(p.position);
            for (Pair pp : list) {
                if (dist[pp.position] > p.weight + pp.weight) {
                    dist[pp.position] = p.weight + pp.weight;
                    count[pp.position] = count[p.position] + 1;
                    if (count[pp.position] >= n) {
                        return true;
                    }
                    if (!isInQueue[pp.position])
                        queue.add(new Pair(pp.position, dist[pp.position]));
                }
            }
        }
        return false;
    }

    public static void main(String[] args) {
        n = in.nextInt();
        int m = in.nextInt();
        for (int i = 0; i <= n; i++) {
            graph.add(new ArrayList<>());
        }
        dist = new int[n + 1];
        count = new int[n + 1];
        isInQueue = new boolean[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE / 2);
        while (m-- > 0) {
            add(in.nextInt(), in.nextInt(), in.nextInt());
        }
        boolean res = spfa();
        if (res) {
            out.println("Yes");
        } else
            out.println("No");
        out.flush();
        out.close();
    }
```

