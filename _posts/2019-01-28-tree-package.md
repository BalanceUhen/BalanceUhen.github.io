---
layout: post
title: 树形背包问题的$O(nm)$做法
author: When
date: 2019-01-28 22:08:07 +0800
categories: Tree Algorithm
tag: Algorithm
---

问题：给定一棵$n$个节点的树，每个节点有一个代价$w_i$和一个价值$v_i$，给定最大花费$m$，最大化价值和

限制：对于一个节点，他的父亲选了他才能选

# 解法

这类问题有一个十分优秀的做法

首先求出树的$dfs$序

然后按照$dfs​$序做过去

设$f(i,j)$表示$dfs$序为$i$的节点$x$，最大花费为$j$时的最大价值

假如$i$选，那么得到一个价值为$f(i+1,j-1)+v_x$的方案

假如$i$不选，那么$i$的子树都不能选，那么可以得到一个价值为$f(i+size_x,j)$的方案

两个方案取一个$max$即可，时间复杂度为$O(nm)$

# 例题

- $RQNOJ 30$
  - 裸题，注意可能$0$号节点会有多个子节点

```cpp
// It's not logic, but magic ~~
#include <bits/stdc++.h>

typedef long long ll;

ll read() {
	static char c;
	static ll x;
	int flag = 1;
	while (c = getchar(), !isdigit(c))
		if (c == '-')
			flag = 0;
	x = c - '0';
	while (c = getchar(), isdigit(c))
		x = x * 10 + c - '0';
	return flag ? x : -x;
}

struct edge {
	int pre, to;
} gs[2020];
int head[1010], gc;
void add(int x, int y) {
	gs[++gc] = (edge) {head[x], y};
	head[x] = gc;
}

#warning 1 may not be the root!
int n, m, root, ids[1010], hash[1010], left[1010], dad[1010];
ll w[1010];
void dfs(int x, int fa = 0) {
	static int id = -1;
	ids[x] = ++id;
	hash[id] = x;
	dad[x] = fa;
	for (int i = head[x]; i; i = gs[i].pre) {
		if (gs[i].to == fa) continue;
		dfs(gs[i].to, x);
	}
	left[x] = id;
}

ll ans[1010][111];
ll f(int x, int rem) {
	if (x > n) return 0;
	ll &tmp = ans[x][rem];
	if (~tmp) return tmp;

	return tmp = std::max(f(left[hash[x]] + 1, rem), rem ? f(x + 1, rem - 1) + w[hash[x]] : 0);
}

signed main() {
	n = read(), m = read();
	for (int i = 1; i <= n; ++i) w[i] = read();
	for (int i = 1; i <= n; ++i) {
		int A = read(), B = read();
		add(A, B);
		add(B, A);
	}

	dfs(0);
	memset(ans, -1, sizeof ans);
	std::cout << f(1, m) << std::endl;
}

```

