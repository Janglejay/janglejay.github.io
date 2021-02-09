---
layout: post
title: KMP
tag: 数据结构
---

## AcWing 831. KMP字符串   [原题链接](https://www.acwing.com/problem/content/833/)

给定一个模式串S，以及一个模板串P，所有字符串中只包含大小写英文字母以及阿拉伯数字。

模板串P在模式串S中多次作为子串出现。

求出模板串P在模式串S中所有出现的位置的起始下标。

#### 输入格式

第一行输入整数N，表示字符串P的长度。

第二行输入字符串P。

第三行输入整数M，表示字符串S的长度。

第四行输入字符串S。

#### 输出格式

共一行，输出所有出现位置的起始下标（下标从0开始计数），整数之间用空格隔开。

#### 数据范围

1≤N≤1051≤N≤105
1≤M≤1061≤M≤106

#### 输入样例：

```
3
aba
5
ababa
```

#### 输出样例：

```
0 2
```

```java
public static void main(String[] args) {
        int n = in.nextInt();
        String pattern = in.next();
        char[] cp = new char[n + 1];
        for (int i = 1; i < cp.length; i++) {
            cp[i] = pattern.charAt(i - 1);
        }
        int m = in.nextInt();
        String str = in.next();
        char[] cs = new char[m + 1];
        for (int i = 1; i < cs.length; i++) {
            cs[i] = str.charAt(i - 1);
        }
        int[] nextArray = getNextArray(cp);
        for (int i = 1, j = 0; i < cs.length; i++) {
            while (j != 0 && cs[i] != cp[j + 1]) {
                j = nextArray[j];
            }
            if (cs[i] == cp[j + 1]) {
                j++;
            }
            if (j == n) {
                out.print(i - n + " ");
                j = nextArray[j];
            }
        }
        out.flush();
        out.close();
    }
    private static int[] getNextArray(char[] cs) {
        int[] next = new int[cs.length];
        for (int i = 2, j = 0; i < cs.length; i++) {
            while (j != 0 && cs[i] != cs[j + 1]) {
                j = next[j];
            }
            if (cs[i] == cs[j + 1]) {
                j++;
            }
            next[i] = j;
        }
        return next;
    }
```

