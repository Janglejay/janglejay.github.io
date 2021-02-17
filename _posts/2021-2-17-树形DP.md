---
layout: post
title: 树形DP
tag: 动态规划
---

## AcWing 285. 没有上司的舞会   [原题链接](https://www.acwing.com/problem/content/287/)

Ural大学有N名职员，编号为1~N。

他们的关系就像一棵以校长为根的树，父节点就是子节点的直接上司。

每个职员有一个快乐指数，用整数 HiHi 给出，其中 1≤i≤N1≤i≤N。

现在要召开一场周年庆宴会，不过，没有职员愿意和直接上司一起参会。

在满足这个条件的前提下，主办方希望邀请一部分职员参会，使得所有参会职员的快乐指数总和最大，求这个最大值。

#### 输入格式

第一行一个整数N。

接下来N行，第 i 行表示 i 号职员的快乐指数HiHi。

接下来N-1行，每行输入一对整数L, K,表示K是L的直接上司。

#### 输出格式

输出最大的快乐指数。

#### 数据范围

1≤N≤60001≤N≤6000,
−128≤Hi≤127−128≤Hi≤127

#### 输入样例：

```
7
1
1
1
1
1
1
1
1 3
2 3
6 4
7 4
4 5
3 5
```

#### 输出样例：

```
5
```

## 题目思路

![ygsPYR.png](https://s3.ax1x.com/2021/02/17/ygsPYR.png)

```java
    private static List<List<Integer>> graph = new ArrayList<>();
    private static int[][] dp;
    private static int[] happy;
    private static boolean[] hasParent;
    private static void add(int a, int b) {
        graph.get(a).add(b);
    }
    private static void dfs(int u) {
        dp[u][1] = happy[u];
        for (Integer i : graph.get(u)) {
            // 到叶子节点就自动停止递归了
            dfs(i);
            dp[u][0] += Math.max(dp[i][0], dp[i][1]);
            dp[u][1] += dp[i][0];
        }
    }
    public static void main(String[] args) {
        int n = in.nextInt();
        happy = new int[n + 1];
        hasParent = new boolean[n + 1];
        dp = new int[n + 1][n + 1];
        for (int i = 0; i < n + 1; i++) graph.add(new ArrayList<>());
        for (int i = 1; i <= n; i++) happy[i] = in.nextInt();
        for (int i = 0; i < n - 1; i++) {
            int a = in.nextInt();
            int b = in.nextInt();
            hasParent[a] = true;
            // b是a的直接上司
            add(b, a);
        }
        int root = 1;
        // 找到根节点，即没有父节点的点
        while (hasParent[root]) root++;
        dfs(root);
        out.println(Math.max(dp[root][0], dp[root][1]));
        out.flush();
        out.close();
    }
```

