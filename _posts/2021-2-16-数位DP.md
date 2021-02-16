---
layout: post
title: 数位DP
tag: 动态规划
---

# 计数问题

![ygutdH.png](https://s3.ax1x.com/2021/02/16/ygutdH.png)

## AcWing 338. 计数问题   [原题链接](https://www.acwing.com/problem/content/340/)

给定两个整数 a 和 b，求 a 和 b 之间的所有数字中0~9的出现次数。

例如，a=1024，b=1032，则 a 和 b 之间共有9个数如下：

1024 1025 1026 1027 1028 1029 1030 1031 1032

其中‘0’出现10次，‘1’出现10次，‘2’出现7次，‘3’出现3次等等…

#### 输入格式

输入包含多组测试数据。

每组测试数据占一行，包含两个整数 a 和 b。

当读入一行为0 0时，表示输入终止，且该行不作处理。

#### 输出格式

每组数据输出一个结果，每个结果占一行。

每个结果包含十个用空格隔开的数字，第一个数字表示‘0’出现的次数，第二个数字表示‘1’出现的次数，以此类推。

#### 数据范围

0<a,b<1000000000<a,b<100000000

#### 输入样例：

```
1 10
44 497
346 542
1199 1748
1496 1403
1004 503
1714 190
1317 854
1976 494
1001 1960
0 0
```

#### 输出样例：

```
1 2 1 1 1 1 1 1 1 1
85 185 185 185 190 96 96 96 95 93
40 40 40 93 136 82 40 40 40 40
115 666 215 215 214 205 205 154 105 106
16 113 19 20 114 20 20 19 19 16
107 105 100 101 101 197 200 200 200 200
413 1133 503 503 503 502 502 417 402 412
196 512 186 104 87 93 97 97 142 196
398 1375 398 398 405 499 499 495 488 471
294 1256 296 296 296 296 287 286 286 247
```

```java
    private static int get(List<Integer> num, int l, int r) {
        int res = 0;
        for (int i = l; i >= r; i--) {
            res = res * 10 + num.get(i);
        }
        return res;
    }

    private static int count(int n, int x) {
        // 题目中 n,x 不可能同时为 0
        if (n == 0) return 0;
        List<Integer> num = new ArrayList<>();
        while (n > 0) {
            num.add(n % 10);
            n /= 10;
        }
        n = num.size();
        int res = 0;
        // 当x=0时，首位不可能为0
        // 所以需要从第二位开始枚举，也就是num的倒数第二位
        for (int i = n - 1 - (x == 0 ? 1 : 0); i >= 0; i--) {
            if (i < n - 1) {
                res += get(num, n - 1, i + 1) * Math.pow(10, i);
                if (x == 0) res -= Math.pow(10, i);
            }
            if (num.get(i) == x) res += get(num, i - 1, 0) + 1;
            else if (num.get(i) > x) res += Math.pow(10, i);
        }
        return res;
    }

    public static void main(String[] args) {
        int n;
        int m;
        while ((n = in.nextInt()) != 0 && (m = in.nextInt()) != 0) {
            if (n > m) {
                int t = n;
                n = m;
                m = t;
            }
            for (int i = 0; i < 10; i++) {
                out.print(count(m, i) - count(n - 1, i) + " ");
            }
            out.println();
        }
        out.flush();
        out.close();
    }
```

