---
layout: post
title: Trie树
tag: 数据结构
---

# Trie树

用来快速存储 和 查找 字符串集合 的数据结构

## 如何存储？

```java
son[N][26]: 每个节点最多有26条边 （单词只由小写字母组成）
count[N]: 以当前节点为结尾的单词有多少个
idx：下标用到了哪， 0 又是根节点 又是 空节点
```

```
insert \ find 方法
insert 一个字符串
```

![ywpuh8.png](https://s3.ax1x.com/2021/02/10/ywpuh8.png)

## 如何查找？

一个一个字符去查找，到结尾的时候需要有**标记**

count 数组就能 作为标记

## 数据结构

```java
class Trie{
    public TrieNode root;
    public Trie(){
        root = new TrieNode();
    }
    public class TrieNode{
        boolean isEnd;
        TrieNode[] son;
        int number;
        public TrieNode(){
            isEnd = false;
            son = new TrieNode[26];
            number = 0;
        }
    }
    public void insert(String word) {
        char[] w = word.toCharArray();
        TrieNode p = root;
        for (char c : w) {
            int u = c - 'a';
            if (p.son[u] == null) {
                p.son[u] = new TrieNode();
            }
            p = p.son[u];
        }
        p.isEnd = true;
        p.number++;
    }
    public int query(String word) {
        char[] w = word.toCharArray();
        TrieNode p = root;
        for (char c : w) {
            int u = c - 'a';
            if (p.son[u] == null) {
                return 0;
            }
            p = p.son[u];
        }
        if (p.isEnd == true) {
            return p.number;
        }
        return 0;
    }
}
```



## AcWing 835. Trie字符串统计   [原题链接](https://www.acwing.com/problem/content/837/)

维护一个字符串集合，支持两种操作：

1. “I x”向集合中插入一个字符串x；
2. “Q x”询问一个字符串在集合中出现了多少次。

共有N个操作，输入的字符串总长度不超过 105105，字符串仅包含小写英文字母。

#### 输入格式

第一行包含整数N，表示操作数。

接下来N行，每行包含一个操作指令，指令为”I x”或”Q x”中的一种。

#### 输出格式

对于每个询问指令”Q x”，都要输出一个整数作为结果，表示x在集合中出现的次数。

每个结果占一行。

#### 数据范围

1≤N≤2∗1041≤N≤2∗104

#### 输入样例：

```
5
I abc
Q abc
Q ab
I ab
Q ab
```

#### 输出样例：

```
1
0
1
```

```java
	static final int N = (int) (2 * 1e4 + 10);
    static int[][] son = new int[N][26];
    static int[] count = new int[N];
    //root and null
    static int idx = 0;
    private static void insert(String str) {
        int p = 0;
        for (int i = 0; i < str.length(); i++) {
            int index = str.charAt(i) - 'a';
            if (son[p][index] == 0) {
                son[p][index] = ++idx;
            }
            p = son[p][index];
        }
        count[p]++;
    }
    private static int query(String str) {
        int p = 0;
        for (int i = 0; i < str.length(); i++) {
            int index = str.charAt(i) - 'a';
            if (son[p][index] == 0) {
                return 0;
            }
            p = son[p][index];
        }
        return count[p];
    }
```



## AcWing 143. 最大异或对   [原题链接](https://www.acwing.com/problem/content/145/)

```java
class MyTrie {
    public TrieNode root;

    public MyTrie() {
        root = new TrieNode();
    }

    public class TrieNode {
        boolean isEnd;
        TrieNode[] son;
        int number;

        public TrieNode() {
            isEnd = false;
            son = new TrieNode[2];
            number = 0;
        }
    }

    public void insert(int x) {
        TrieNode p = root;
        for (int i = 30; i >= 0; i--) {
            int u = (x >> i) & 1;
            if (p.son[u] == null) {
                p.son[u] = new TrieNode();
            }
            p = p.son[u];
        }
        p.isEnd = true;
        p.number++;
    }

    public int query(int x) {
        int res = 0;
        TrieNode p = root;
        for (int i = 30; i >= 0; i--) {
            int u = (x >> i) & 1;
            if (p.son[u ^ 1] != null) {
                res += (1 << i);
                p = p.son[u ^ 1];
            } else {
                p = p.son[u];
            }
        }
        return res;
    }
}

public static void main(String[] args) {
        int n = in.nextInt();
        int[] arr = new int[n];
        in.nextIntegerArray(arr);
        MyTrie trie = new MyTrie();
        for (int x : arr) {
            trie.insert(x);
        }
        int res = 0;
        for (int x : arr) {
            res = Math.max(trie.query(x), res);
        }
        out.println(res);
        out.flush();
        out.close();
    }
```

