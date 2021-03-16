---
layout: post
title: RMQ
tag: 基础算法2
---

# RMQ

RMQ (Range Minimum/Maximum Query)问题是指：对于长度为n的数列A，回答若干询问RMQ(A,i,j)(i,j<=n)，返回数列A中下标在i,j里的最小(大）值，也就是说，RMQ问题是指求区间最值的问题。

## AcWing 1273. 天才的记忆   [原题链接](https://www.acwing.com/problem/content/1275/)

从前有个人名叫 WNB，他有着天才般的记忆力，他珍藏了许多许多的宝藏。

在他离世之后留给后人一个难题（专门考验记忆力的啊！），如果谁能轻松回答出这个问题，便可以继承他的宝藏。

题目是这样的：给你一大串数字（编号为 11 到 NN，大小可不一定哦！），在你看过一遍之后，它便消失在你面前，随后问题就出现了，给你 MM 个询问，每次询问就给你两个数字 A,BA,B，要求你瞬间就说出属于 AA 到 BB 这段区间内的最大数。

一天，一位美丽的姐姐从天上飞过，看到这个问题，感到很有意思（主要是据说那个宝藏里面藏着一种美容水，喝了可以让这美丽的姐姐更加迷人），于是她就竭尽全力想解决这个问题。

但是，她每次都以失败告终，因为这数字的个数是在太多了！

于是她请天才的你帮他解决。如果你帮她解决了这个问题，可是会得到很多甜头的哦！

#### 输入格式

第一行一个整数 NN 表示数字的个数。

接下来一行为 NN 个数，表示数字序列。

第三行读入一个 MM，表示你看完那串数后需要被提问的次数。

接下来 MM 行，每行都有两个整数 A,BA,B。

#### 输出格式

输出共 MM 行，每行输出一个数，表示对一个问题的回答。

#### 数据范围

1≤N≤2×1051≤N≤2×105,
1≤M≤1041≤M≤104

#### 输入样例：

```
6
34 1 8 123 3 2
4
1 2
1 5
3 4
2 3
```

#### 输出样例：

```
34
123
123
8
```

## 题目思路

![6sjrNT.png](https://s3.ax1x.com/2021/03/16/6sjrNT.png)

```java
    private static class skipTable {
        private int[][] dp;
        private int[] arr;
        public skipTable(int n, int[] arr) {
            int need = (int) (Math.log(n) / Math.log(2)) + 1;
            this.dp = new int[n + 1][need + 1];
            this.arr = arr;
            // 枚举区间长度 2^need
            for (int j = 0; j <= need; j++) {
                // 枚举左端点
                for (int i = 1; i + (1 << j) - 1 <= n; i++) {
                    // 区间里只有一个数，最大值就是它本身
                    if (j == 0) dp[i][j] = arr[i];
                    else dp[i][j] = Math.max(dp[i][j - 1], dp[i + (1 << (j - 1))][j - 1]);
                }
            }
        }
        public int query(int l, int r) {
            int len = r - l + 1;
            int k = (int) (Math.log(len) / Math.log(2));
            return Math.max(dp[l][k], dp[r - (1 << k) + 1][k]);
        }
    }

    public static void main(String[] args) {
        int n = in.nextInt();
        int[] arr = new int[n + 1];
        for (int i = 1; i < n + 1; i++) arr[i] = in.nextInt();
        skipTable st = new skipTable(n, arr);
        int m = in.nextInt();
        while (m-- > 0) {
            int l = in.nextInt();
            int r = in.nextInt();
            out.println(st.query(l, r));
        }
        out.flush();
        out.close();
    }

```

