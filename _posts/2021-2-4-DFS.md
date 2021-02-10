---
layout: post
title: DFS
tag: 搜索与图论
---

# DFS

## AcWing 842. 排列数字   [原题链接](https://www.acwing.com/problem/content/844/)

给定一个整数n，将数字1~n排成一排，将会有很多种排列方法。

现在，请你按照字典序将所有的排列方法输出。

#### 输入格式

共一行，包含一个整数n。

#### 输出格式

按字典序输出所有排列方案，每个方案占一行。

#### 数据范围

1≤n≤71≤n≤7

#### 输入样例：

```
3
```

#### 输出样例：

```
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```

```java
 static List<Integer> res = new ArrayList<>();
    static boolean[] st;
    static int n;
    public static void main(String[] args) {
        n = in.nextInt();
        st = new boolean[n + 1];
        dfs(0, new ArrayList<>());
        out.flush();
        out.close();
    }
    private static void dfs(int num, List<Integer> list) {
        if (num == n) {
            for (int ii : list) out.println(ii + " ");
            out.println();
            return;
        }
        for (int i = 1; i <= n; i++) {
            if (!st[i]) {
                list.add(i);
                st[i] = true;
                dfs(num + 1, list);
                st[i] = false;
                list.remove(list.size() - 1);
            }
        }
    }
```

## AcWing 843. n-皇后问题   [原题链接](https://www.acwing.com/problem/content/845/)

n-皇后问题是指将 n 个皇后放在 n∗n 的国际象棋棋盘上，使得皇后不能相互攻击到，即任意两个皇后都不能处于同一行、同一列或同一斜线上。

![ywpV0I.png](https://s3.ax1x.com/2021/02/10/ywpV0I.png)
现在给定整数n，请你输出所有的满足条件的棋子摆法。

#### 输入格式

共一行，包含整数n。

#### 输出格式

每个解决方案占n行，每行输出一个长度为n的字符串，用来表示完整的棋盘状态。

其中”.”表示某一个位置的方格状态为空，”Q”表示某一个位置的方格上摆着皇后。

每个方案输出完成后，输出一个空行。

输出方案的顺序任意，只要不重复且没有遗漏即可。

#### 数据范围

1≤n≤9

#### 输入样例：

```
4
```

#### 输出样例：

```
.Q..
...Q
Q...
..Q.

..Q.
Q...
...Q
.Q..
```

```java
static int n;
    static boolean[] col;
    static boolean[] diagonal;
    static boolean[] backDiagonal;
    static char[][] map;

    public static void main(String[] args) {
        n = in.nextInt();
        col = new boolean[n];
        diagonal = new boolean[n << 1];
        backDiagonal = new boolean[n << 1];
        map = new char[n][n];
        for (int i = 0; i < n; i++) Arrays.fill(map[i], '.');
        dfs(0);
        out.flush();
        out.close();
    }

    private static void dfs(int num) {
        if (num == n) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++)
                    out.print(map[i][j]);
                out.println();
            }
            out.println();
            return;
        }
        for (int j = 0; j < n; j++) {
            if (!col[j] && !diagonal[num - j + n] && !backDiagonal[num + j]) {
                col[j] = diagonal[num - j + n] = backDiagonal[num + j] = true;
                map[num][j] = 'Q';
                dfs(num + 1);
                map[num][j] = '.';
                col[j] = diagonal[num - j + n] = backDiagonal[num + j] = false;
            }
        }
    }
```

## AcWing 846. 树的重心   [原题链接](https://www.acwing.com/problem/content/848/)

给定一颗树，树中包含n个结点（编号1~n）和n-1条无向边。

请你找到树的重心，并输出将重心删除后，剩余各个连通块中点数的最大值。

重心定义：重心是指树中的一个结点，如果将这个点删除后，剩余各个连通块中点数的最大值最小，那么这个节点被称为树的重心。

#### 输入格式

第一行包含整数n，表示树的结点数。

接下来n-1行，每行包含两个整数a和b，表示点a和点b之间存在一条边。

#### 输出格式

输出一个整数m，表示将重心删除后，剩余各个连通块中点数的最大值。

#### 数据范围

1≤n≤1051≤n≤105

#### 输入样例

```
9
1 2
1 7
1 4
2 8
2 5
4 3
3 9
4 6
```

#### 输出样例：

```
4
```

```java
static int n;
    static List<List<Integer>> graph = new ArrayList<>();
    static boolean[] st;
    static int res = Integer.MAX_VALUE;
    private static void add(int a, int b) {
        graph.get(a).add(b);
        graph.get(b).add(a);
    }

    public static void main(String[] args) {
        n = in.nextInt();
        st = new boolean[n + 1];  
        for (int i = 0; i < n + 1; i++) graph.add(new ArrayList<>());
        for (int i = 0; i < n - 1; i++) add(in.nextInt(), in.nextInt());
        dfs(1);
        out.println(res);
        out.flush();
        out.close();
    }
    private static int dfs(int cur) {
        st[cur] = true;
        List<Integer> list = graph.get(cur);
        int sum = 1;
        int max = 0;
        for (int x : list) {
            if (!st[x]) {
                int number = dfs(x);
                max = Math.max(number, max);
                sum += number;
                st[x] = false;
            }
        }
        max = Math.max(max, n - sum);
        res = Math.min(res, max);
        return sum;
    }
```

