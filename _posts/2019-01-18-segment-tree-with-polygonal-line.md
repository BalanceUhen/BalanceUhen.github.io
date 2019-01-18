---
layout: post
title: 线段树维护极长上升子序列
author: When
date: 2019-01-18 20:22:10 +0800
categories: Segment-tree Template Trick
tag: Data structure
---

发现线段树有好多奇妙的技巧:happy:，能学一点是一点

前天ct讲课的时候给出了线段树维护折线的一种方法，今天上午zzx出的题目第二题就是用线段树维护最长上升子序列来优化$dp$:cold_sweat:，虽然暴力蹭分就有$44pts$

大概介绍一下这个技巧吧

# 问题

维护区间的极长上升子序列，一般要求顺便维护额外的信息（比如长度（废话:rage:）、权值和）（但是合并起来不能太麻烦）之类的，这个是可以用线段树维护的，而且维护起来及其方便，代码也很好写，只是时间上要多一个$log$

# 解决

考虑如何维护这个子序列的信息，很明显的是只能维护某个特定的值，我们可以选择维护最大值来解决这个问题（*Question: 维护其他的值可以吗？*），当然了维护起来还是需要一些技巧的

## 更新操作

更新操作通过查询操作来正确的合并左右子树的值

考虑我们要$maintain$ $x$节点的值，并且$x$节点的$lc$和$rc$都已经处理完毕了，最大值直接简单的取一个$max$即可，关键是如何维护附加信息

首先$lc$的极长子序列一定会是$x$的极长子序列的一部分（废话:rage:），因此$lc$的子序列的附加信息可以直接对$x$做出贡献，那么我们就是要确定$rc$的子序列的附加信息对$x$的贡献

因此我们需要一个查询操作，查询某个节点，限制子序列中的值大于（我们是*上升*而不是*非降子序列*，因此$=$是否能取到是一个小细节）$min$的情况下，这个节点的附加信息是多少，这就需要我们来专门设计一个查询操作来处理了

## 查询操作

考虑我们要查询$x$节点的附加信息，同时要求子序列中的数字要大于$min$

分两种情况考虑：

1. $min\ge max_{lc}$，这个时候$lc$中的子序列无法对答案做出贡献，递归到$rc$继续进行查询即可
2. $min<max_{lc}$，我们需要递归到$lc$进行查询，所以我们需要快速的求出$rc$中会对答案产生贡献的那一部分的附加信息，假如附加信息满足可减性的话，$ans_x-ans_{lc}$就是$rc$对答案的贡献

## 时间复杂度分析

查询操作的时间复杂度是$O(\log n)$，更新操作每一个节点都有一次查询，最多访问$O(\log n)$个节点，因此更新操作的时间复杂度是$O(\log ^2 n)$

# 例题

- BZOJ 2957：裸题，只是要求额外维护极长上升子序列的长度即可，代码如下：

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

int n, m;
double max[400400];
int ans[400400];
#define lc x<<1
#define rc x<<1|1
int query(double min, int x = 1, int l = 1, int r = n) {
	if (l == r) return max[x] > min;
	int mid = l + r >> 1;
	if (min >= max[lc]) return query(min, rc, mid + 1, r);
	return query(min, lc, l, mid) + ans[x] - ans[lc];
}

void update(int p, double v, int x = 1, int l = 1, int r = n) {
	if (l == r) {
		max[x] = v;
		ans[x] = 1;
		return ;
	}
	int mid = l + r >> 1;
	if (p <= mid) update(p, v, lc, l, mid);
	else update(p, v, rc, mid + 1, r);

	max[x] = std::max(max[lc], max[rc]);
	ans[x] = ans[lc];
	ans[x] += query(max[lc], rc, mid + 1, r);
}

void output(int x = 1, int l = 1, int r = n) {
	printf("%d %d %d %lf %d\n", x, l, r, max[x], ans[x]);
	if (l == r) return;
	int mid = l + r >> 1;
	output(lc, l, mid);
	output(rc, mid + 1, r);
}

signed main() {
	n = read(), m = read();
	while (m--) {
		int x = read(), y = read();
		update(x, double(y) / x);
		printf("%d\n", ans[1]);
		// output();
	}
}

```

