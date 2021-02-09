---
layout: post
title: BFS
tag: 搜索与图论
---

# BFS

需要自己写栈，当时可以用来求最短步数的问题，维护一个`dist`数组，表示遍历到的点到起始位置的距离

## AcWing 844. 走迷宫   [原题链接](https://www.acwing.com/problem/content/846/)

给定一个n*m的二维整数数组，用来表示一个迷宫，数组中只包含0或1，其中0表示可以走的路，1表示不可通过的墙壁。

最初，有一个人位于左上角(1, 1)处，已知该人每次可以向上、下、左、右任意一个方向移动一个位置。

请问，该人从左上角移动至右下角(n, m)处，至少需要移动多少次。

数据保证(1, 1)处和(n, m)处的数字为0，且一定至少存在一条通路。

#### 输入格式

第一行包含两个整数n和m。

接下来n行，每行包含m个整数（0或1），表示完整的二维数组迷宫。

#### 输出格式

输出一个整数，表示从左上角移动至右下角的最少移动次数。

#### 数据范围

1≤n,m≤1001≤n,m≤100

#### 输入样例：

```
5 5
0 1 0 0 0
0 1 0 1 0
0 0 0 0 0
0 1 1 1 0
0 0 0 1 0
```

#### 输出样例：

```
8
```

```java
    static int n;
    static int m;
    static int[][] map;
    static int[][] dist;
    static int[] dx = new int[]{-1, 0, 1, 0};
    static int[] dy = new int[]{0, 1, 0, -1};
    public static void main(String[] args) {
        n = in.nextInt();
        m = in.nextInt();
        map = new int[n + 1][m + 1];
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < m + 1; j++) {
                map[i][j] = in.nextInt();
            }
        }
        dist = new int[n + 1][m + 1];
        for (int i = 0; i < n + 1; i++) Arrays.fill(dist[i], -1);
        dist[1][1] = 0;
        int res = bfs();
        out.println(res);
        out.flush();
        out.close();
    }
    private static int bfs() {
        Queue<int[]> queue = new ArrayDeque<>();
        queue.add(new int[]{1, 1});
        while (!queue.isEmpty()) {
            int[] p = queue.poll();
            for (int i = 0; i < 4; i++) {
                int x = dx[i] + p[0];
                int y = dy[i] + p[1];
                if (x < 1 || y < 1 || x > n || y > m || map[x][y] == 1 || dist[x][y] != -1) {
                    continue;
                }
                dist[x][y] = dist[p[0]][p[1]] + 1;
                queue.offer(new int[]{x, y});
            }
        }
        return dist[n][m];
    }
```

## AcWing 845. 八数码   [原题链接](https://www.acwing.com/problem/content/847/)

在一个3×3的网格中，1~8这8个数字和一个“x”恰好不重不漏地分布在这3×3的网格中。

例如：

```
1 2 3
x 4 6
7 5 8
```

在游戏过程中，可以把“x”与其上、下、左、右四个方向之一的数字交换（如果存在）。

我们的目的是通过交换，使得网格变为如下排列（称为正确排列）：

```
1 2 3
4 5 6
7 8 x
```

例如，示例中图形就可以通过让“x”先后与右、下、右三个方向的数字交换成功得到正确排列。

交换过程如下：

```
1 2 3   1 2 3   1 2 3   1 2 3
x 4 6   4 x 6   4 5 6   4 5 6
7 5 8   7 5 8   7 x 8   7 8 x
```

现在，给你一个初始网格，请你求出得到正确排列至少需要进行多少次交换。

#### 输入格式

输入占一行，将3×3的初始网格描绘出来。

例如，如果初始网格如下所示：
1 2 3

x 4 6

7 5 8

则输入为：1 2 3 x 4 6 7 5 8

#### 输出格式

输出占一行，包含一个整数，表示最少交换次数。

如果不存在解决方案，则输出”-1”。

#### 输入样例：

```
2  3  4  1  5  x  7  6  8 
```

#### 输出样例

```
19
```

```java
 static String start = "";
    static Map<String, Integer> map = new HashMap<>();
    static int[] dx = new int[]{-1, 0, 1, 0};
    static int[] dy = new int[]{0, 1, 0, -1};

    public static void main(String[] args) {
        start = Arrays.stream(in.nextLine().split(" ")).collect(Collectors.joining());
        int res = bfs();
        out.println(res);
        out.flush();
        out.close();
    }

    private static int bfs() {
        String end = "12345678x";
        map.put(start, 0);
        Queue<String> queue = new ArrayDeque<>();
        queue.add(start);
        while (!queue.isEmpty()) {
            String str = queue.poll();
            int idx = str.indexOf("x");
            for (int i = 0; i < 4; i++) {
                int x = dx[i] + idx / 3;
                int y = dy[i] + idx % 3;
                if (x < 0 || y < 0 || x >= 3 || y >= 3) continue;
                char[] cs = str.toCharArray();
                cs[idx] = cs[x * 3 + y];
                cs[x * 3 + y] = 'x';
                String temp = new String(cs);
                if (map.containsKey(temp)) continue;
                map.put(temp, map.get(str) + 1);
                queue.offer(temp);
                if (temp.equals(end)) {
                    break;
                }
            }
        }
        return map.get(end) == null ? -1 : map.get(end);
    }
```

## AcWing 847. 图中点的层次   [原题链接](https://www.acwing.com/problem/content/849/)

给定一个n个点m条边的有向图，图中可能存在重边和自环。

所有边的长度都是1，点的编号为1~n。

请你求出1号点到n号点的最短距离，如果从1号点无法走到n号点，输出-1。

#### 输入格式

第一行包含两个整数n和m。

接下来m行，每行包含两个整数a和b，表示存在一条从a走到b的长度为1的边。

#### 输出格式

输出一个整数，表示1号点到n号点的最短距离。

#### 数据范围

1≤n,m≤1051≤n,m≤105

#### 输入样例：

```
4 5
1 2
2 3
3 4
1 3
1 4
```

#### 输出样例：

```
1
```

```java
static int n;
    static int m;
    static List<List<Integer>> graph = new ArrayList<>();
    static int[] dist;
    private static void add(int a, int b) {
        if (a == b) {
            return;
        }
        graph.get(a).add(b);
    }
    public static void main(String[] args) {
        n = in.nextInt();
        m = in.nextInt();
        dist = new int[n + 1];
        Arrays.fill(dist, -1);
        for (int i = 0; i < n + 1; i++) graph.add(new ArrayList<>());
        for (int i = 0; i < m; i++) add(in.nextInt(), in.nextInt());
        int res = bfs();
        out.println(res);
        out.flush();
        out.close();
    }
    private static int bfs() {
        Queue<Integer> queue = new ArrayDeque<>();
        queue.add(1);
        dist[1] = 0;
        while (!queue.isEmpty()) {
            Integer idx = queue.poll();
            List<Integer> list = graph.get(idx);
            for (int x : list) {
                if (dist[x] == -1) {
                    dist[x] = dist[idx] + 1;
                    queue.offer(x);
                }
            }
        }
        return dist[n];
    }
```

