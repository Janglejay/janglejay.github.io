---
layout: post
title: 线性DP
tag: 动态规划
---

# 数字三角形

![y6owrR.png](https://s3.ax1x.com/2021/02/15/y6owrR.png)

# 最长上升子序列

![y6o8aV.png](https://s3.ax1x.com/2021/02/15/y6o8aV.png)

# 最长公共子序列

![y6oYPU.png](https://s3.ax1x.com/2021/02/15/y6oYPU.png)


# 最短编辑距离

![y6ojLn.md.png](https://s3.ax1x.com/2021/02/15/y6ojLn.md.png)

## AcWing 898. 数字三角形   [原题链接](https://www.acwing.com/problem/content/900/)

给定一个如下图所示的数字三角形，从顶部出发，在每一结点可以选择移动至其左下方的结点或移动至其右下方的结点，一直走到底层，要求找出一条路径，使路径上的数字的和最大。

```
        7
      3   8
    8   1   0
  2   7   4   4
4   5   2   6   5
```

#### 输入格式

第一行包含整数n，表示数字三角形的层数。

接下来n行，每行包含若干整数，其中第 i 行表示数字三角形第 i 层包含的整数。

#### 输出格式

输出一个整数，表示最大的路径数字和。

#### 数据范围

1≤n≤5001≤n≤500,
−10000≤三角形中的整数≤10000−10000≤三角形中的整数≤10000

#### 输入样例：

```
5
7
3 8
8 1 0 
2 7 4 4
4 5 2 6 5
```

#### 输出样例：

```
30
```

朴素写法：

```java
    public static void main(String[] args) {
        int n = in.nextInt();
        int res = Integer.MIN_VALUE;
        int[][] arr = new int[n + 1][n + 1];
        int[][] dp = new int[n + 1][n + 2];
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < i + 1; j++) {
                arr[i][j] = in.nextInt();
            }
        }
        for (int i = 0; i < n + 1; i++) {
            Arrays.fill(dp[i], Integer.MIN_VALUE);
        }
        dp[0][1] = 0;
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < i + 1; j++) {
                dp[i][j] = Math.max(dp[i - 1][j - 1], dp[i - 1][j]) + arr[i][j];
            }
        }
        for (int i = 1; i < n + 1; i++) {
            res = Math.max(res, dp[n][i]);
        }
        out.println(res);
        out.flush();
        out.close();
    }
```

空间优化写法：

```java
    private static void function2() {
        int n = in.nextInt();
        int res = Integer.MIN_VALUE;
        int[][] arr = new int[n + 1][n + 1];
        int[] dp = new int[n + 2];
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < i + 1; j++) {
                arr[i][j] = in.nextInt();
            }
        }
        Arrays.fill(dp, Integer.MIN_VALUE);
        dp[1] = 0;
        for (int i = 1; i < n + 1; i++) {
            for (int j = i; j >= 1; j--) {
                dp[j] = Math.max(dp[j - 1], dp[j]) + arr[i][j];
            }
        }
        for (int i = 1; i < n + 1; i++) {
            res = Math.max(res, dp[i]);
        }
        out.println(res);
        out.flush();
        out.close();
    }
```

## AcWing 895. 最长上升子序列   [原题链接](https://www.acwing.com/problem/content/897/)

给定一个长度为N的数列，求数值严格单调递增的子序列的长度最长是多少。

#### 输入格式

第一行包含整数N。

第二行包含N个整数，表示完整序列。

#### 输出格式

输出一个整数，表示最大长度。

#### 数据范围

1≤N≤10001≤N≤1000，
−109≤数列中的数≤109−109≤数列中的数≤109

#### 输入样例：

```
7
3 1 2 1 8 5 6
```

#### 输出样例：

```
4
```

朴素写法：

```java
    private static void function1() {
        int n = in.nextInt();
        int[] arr = new int[n];
        in.nextIntegerArray(arr);
        int[] dp = new int[n + 1];
        int res = 0;
        for (int i = 1; i < n + 1; i++) {
            dp[i] = 1;
            for (int j = 1; j < i; j++) {
                if (arr[i - 1] > arr[j - 1])
                    dp[i] = Math.max(dp[i], dp[j] + 1);
            }
            res = Math.max(dp[i], res);
        }
        out.println(res);
        out.flush();
        out.close();
    }
```

## AcWing 896. 最长上升子序列 II   [原题链接](https://www.acwing.com/problem/content/898/)

给定一个长度为N的数列，求数值严格单调递增的子序列的长度最长是多少。

#### 输入格式

第一行包含整数N。

第二行包含N个整数，表示完整序列。

#### 输出格式

输出一个整数，表示最大长度。

#### 数据范围

1≤N≤1000001≤N≤100000，
−109≤数列中的数≤109−109≤数列中的数≤109

#### 输入样例：

```
7
3 1 2 1 8 5 6
```

#### 输出样例：

```
4
```

```java
    private static void function2() {
        int n = in.nextInt();
        int[] arr = new int[n + 1];
        for (int i = 1; i < n + 1; i++) arr[i] = in.nextInt();
        int[] q = new int[n + 1];
        int len = 0;
        int res = 0;
        q[0] = Integer.MIN_VALUE;
        for (int i = 1; i < n + 1; i++) {
            int l = 0;
            int r = len;
            while (l < r) {
                int mid = l + r + 1 >> 1;
                if (q[mid] < arr[i]) l = mid;
                else r = mid - 1;
            }
            len = Math.max(len, ++l);
            q[l] = arr[i];
            res = Math.max(res, len);
        }
        out.println(res);
        out.flush();
        out.close();
    }
```

## AcWing 897. 最长公共子序列   [原题链接](https://www.acwing.com/problem/content/899/)

给定两个长度分别为N和M的字符串A和B，求既是A的子序列又是B的子序列的字符串长度最长是多少。

#### 输入格式

第一行包含两个整数N和M。

第二行包含一个长度为N的字符串，表示字符串A。

第三行包含一个长度为M的字符串，表示字符串B。

字符串均由小写字母构成。

#### 输出格式

输出一个整数，表示最大长度。

#### 数据范围

1≤N,M≤10001≤N,M≤1000

#### 输入样例：

```
4 5
acbd
abedc
```

#### 输出样例：

```
3
```

```java
    public static void main(String[] args) {
        int n = in.nextInt();
        int m = in.nextInt();
        String a = in.next();
        String b = in.next();
        int[][] dp = new int[n + 1][m + 1];
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < m + 1; j++) {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                if (a.charAt(i - 1) == b.charAt(j - 1))
                    dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - 1] + 1);
            }
        }
        out.println(dp[n][m]);
        out.flush();
        out.close();
    }
```

## AcWing 902. 最短编辑距离   [原题链接](https://www.acwing.com/problem/content/904/)

给定两个字符串A和B，现在要将A经过若干操作变为B，可进行的操作有：

1. 删除–将字符串A中的某个字符删除。
2. 插入–在字符串A的某个位置插入某个字符。
3. 替换–将字符串A中的某个字符替换为另一个字符。

现在请你求出，将A变为B至少需要进行多少次操作。

#### 输入格式

第一行包含整数n，表示字符串A的长度。

第二行包含一个长度为n的字符串A。

第三行包含整数m，表示字符串B的长度。

第四行包含一个长度为m的字符串B。

字符串中均只包含大写字母。

#### 输出格式

输出一个整数，表示最少操作次数。

#### 数据范围

1≤n,m≤10001≤n,m≤1000

#### 输入样例：

```
10 
AGTCTGACGC
11 
AGTAAGTAGGC
```

#### 输出样例：

```
4
```

```java
    public static void main(String[] args) {
        int n = in.nextInt();
        String a = in.next();
        int m = in.nextInt();
        String b = in.next();
        int[][] dp = new int[n + 1][m + 1];
        for (int i = 0; i < n + 1; i++) dp[i][0] = i;
        for (int i = 0; i < m + 1; i++) dp[0][i] = i;
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < m + 1; j++) {
                dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + 1;
                dp[i][j] = Math.min(dp[i][j], dp[i - 1][j - 1] + (a.charAt(i - 1) == b.charAt(j - 1) ? 0 : 1));
            }
        }
        out.println(dp[n][m]);
        out.flush();
        out.close();
    }
```

## AcWing 899. 编辑距离   [原题链接](https://www.acwing.com/problem/content/901/)

给定n个长度不超过10的字符串以及m次询问，每次询问给出一个字符串和一个操作次数上限。

对于每次询问，请你求出给定的n个字符串中有多少个字符串可以在上限操作次数内经过操作变成询问给出的字符串。

每个对字符串进行的单个字符的插入、删除或替换算作一次操作。

#### 输入格式

第一行包含两个整数n和m。

接下来n行，每行包含一个字符串，表示给定的字符串。

再接下来m行，每行包含一个字符串和一个整数，表示一次询问。

字符串中只包含小写字母，且长度均不超过10。

#### 输出格式

输出共m行，每行输出一个整数作为结果，表示一次询问中满足条件的字符串个数。

#### 数据范围

1≤n,m≤10001≤n,m≤1000,

#### 输入样例：

```
3 2
abc
acd
bcd
ab 1
acbd 2
```

#### 输出样例：

```
1
3
```

```java
    private static int shortestEditScript(String a, String b) {
        int n = a.length();
        int m = b.length();
        int[][] dp = new int[n + 1][m + 1];
        for (int i = 0; i < n + 1; i++) dp[i][0] = i;
        for (int i = 0; i < m + 1; i++) dp[0][i] = i;
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < m + 1; j++) {
                dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + 1;
                dp[i][j] = Math.min(dp[i][j], dp[i - 1][j - 1] + (a.charAt(i - 1) == b.charAt(j - 1) ? 0 : 1));
            }
        }
        return dp[n][m];
    }
    public static void main(String[] args) {
        int n = in.nextInt();
        int m = in.nextInt();
        String[] ss = new String[n];
        for (int i = 0; i < n; i++) ss[i] = in.next();
        for (int i = 0; i < m; i++) {
            String a = in.next();
            int limit = in.nextInt();
            int count = 0;
            for (int j = 0; j < n; j++) {
                if (shortestEditScript(ss[j], a) <= limit) count++;
            }
            out.println(count);
        }
        out.flush();
        out.close();
    }
```

