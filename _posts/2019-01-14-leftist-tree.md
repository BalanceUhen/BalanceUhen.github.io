---
layout: post
title: 左偏树
subtitle: 
author: When
date: 2019-01-14 16:33:51 +0800
categories: Leftist-Tree Template
tag: Data structure
---

左偏树（*Leftist tree*）是可并堆的一种。可并堆，顾名思义，是可以合并的堆。

# 简介

可并堆有两种比较常用的实现，一种是斜堆，代码简单，但是时间复杂度不稳定，另外一种则是左偏树。

## 约定

- 假设堆的大小为$n$
- 默认堆为小根堆
- 一个节点的值为$val_i$，左子树为$l_i$，右子树为$r_i$

# 定义

- 一个节点为*外节点*当且仅当他的一个儿子是空节点
- 一个节点的$dis_i$为这个节点到最近的外节点的距离
  - 特别的，空节点的$dis​$值为$-1​$
  - 左偏树要求$dis_{l_i}\ge dis_{r_i}$

## 性质

- 一个节点的$dis_i$会等于$dis_{r_i}+1$
- 根的$dis_{root}=O({\log n})$
  - 考虑$\forall i\in n,dis_{l_i}=dis_{r_i}$的情况，这个时候$dis_{root}$最大，为$\log n$

## $Merge$操作

这个操作是可并堆的核心操作，只要保证这个操作能够在$O(\log n)$内实现，我们就能够在$O(\log n)$的时间内实现$push$和$pop$操作

假设我们要合并节点$a$和$b$$(val_a<val_b)$，并且返回一棵新的树，那么：

- 假如$a$为空或者$b$为空，返回不为空的节点
- 将$a$的右子树置为$Merge(r_i,b)$，为了维护左偏性质可能要交换左右子树

考虑时间复杂度，每次都是合并右子树，合并的时间复杂度自然是$O(\log n)$的

# 斜堆

考虑到斜堆也是可并堆的一种，并且也十分好理解，就把斜堆的使用一起发出来吧

## $Merge$操作

类比左偏树的$Merge$操作，同样都是把右儿子拿去合并，这样右儿子的$dis$自然会变大，所以我们$Merge$完之后$swap$左儿子和右儿子即可

时间复杂度似乎是均摊$O(\log n)$的，但是**据说**常数比较大，而且还有可能被卡，但是写起来十分的爽就好了

# 代码实现

题目是洛谷的可并堆模板题

## 左偏树代码

```cpp
// It's not logic, but magic ~~
#include <bits/stdc++.h>
typedef long long ll;

ll read() {
    static char c;
    static ll x;
    int flag = 1;
    while (c = getchar(), !isdigit(c)) if (c == '-') flag = 0;
    x = c - '0';
    while (c = getchar(), isdigit(c))x = x * 10 + c - '0';
    return flag ? x : -x;
}

#define N 100100
int n, m;
int ns[N], fa[N], lc[N], rc[N], dis[N];

int find(int x) {return fa[x] ? find(fa[x]) : x;}
int merge(int x, int y) {
    if (!(x && y)) return x | y;
    if (ns[x] > ns[y] || (ns[x] == ns[y] && x > y)) std::swap(x, y);
    rc[x] = merge(rc[x], y);
    if (dis[rc[x]] > dis[lc[x]]) std::swap(lc[x], rc[x]);
    dis[x] = dis[rc[x]] + 1;
    fa[lc[x]] = fa[rc[x]] = x;
    return x;
}
void pop(int x) {
    ns[x] = -1;
    fa[lc[x]] = fa[rc[x]] = 0;
    merge(lc[x], rc[x]);
    lc[x] = rc[x] = 0;
    fa[x] = 0;
}

signed main() {
    n = read(), m = read();
    for (int i = 1; i <= n; ++i) ns[i] = read();
    for (int i = 1; i <= m; ++i) {
        if (read() == 1) {
            int x = read(), y = read();
            int fx = find(x), fy = find(y);
            if (ns[fx] == -1 || ns[fy] == -1) continue;
            if (fx == fy) continue;
            merge(fx, fy);
        } else {
            int x = read();
            x = find(x);
            printf("%d\n", ns[x]);
            pop(x);
        }
    }
}

```

可以看到核心函数$pop()$和$merge()$还是比较简短的

## 斜堆代码

```c++
// It's not logic, but magic ~~
#include <bits/stdc++.h>
typedef long long ll;

ll read() {
    static char c;
    static ll x;
    int flag = 1;
    while (c = getchar(), !isdigit(c)) if (c == '-') flag = 0;
    x = c - '0';
    while (c = getchar(), isdigit(c))x = x * 10 + c - '0';
    return flag ? x : -x;
}

#define N 100100
int n, m;
int ns[N], fa[N], lc[N], rc[N];

int find(int x) {return fa[x] ? find(fa[x]) : x;}
int merge(int x, int y) {
    if (!(x && y)) return x | y;
    if (ns[x] > ns[y] || (ns[x] == ns[y] && x > y)) std::swap(x, y);
    rc[x] = merge(rc[x], y);
    std::swap(lc[x], rc[x]);
    fa[lc[x]] = fa[rc[x]] = x;
    return x;
}
void pop(int x) {
    ns[x] = -1;
    fa[lc[x]] = fa[rc[x]] = 0;
    merge(lc[x], rc[x]);
    lc[x] = rc[x] = 0;
    fa[x] = 0;
}

signed main() {
    n = read(), m = read();
    for (int i = 1; i <= n; ++i) ns[i] = read();
    for (int i = 1; i <= m; ++i) {
        if (read() == 1) {
            int x = read(), y = read();
            int fx = find(x), fy = find(y);
            if (ns[fx] == -1 || ns[fy] == -1) continue;
            if (fx == fy) continue;
            merge(fx, fy);
        } else {
            int x = read();
            x = find(x);
            printf("%d\n", ns[x]);
            pop(x);
        }
    }
}

```

感觉代码并没有省太多，但是节省了一个数组的感觉还会很不错的