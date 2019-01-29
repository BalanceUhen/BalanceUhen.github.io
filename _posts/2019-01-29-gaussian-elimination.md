---
layout: post
title: 高斯消元及简单应用
author: When
date: 2019-01-29 16:55:57 +0800
categories: Math Algorithm
tag: Algorithm
---

我们可以把一个线性方程组写成两个矩阵相乘的形式，然后对系数矩阵进行高斯消元，即可得到原方程组的一组解

# 初等行变换

- 对系数矩阵进行初等行变换不影响线性方程组的解

$3$种初等行变换：

1. 两行（列）互换
2. 把某行（列）乘以一非零常数
3. 把第$i$行（列）加上第$j$行（列）的$k$倍

应用初等行变换将系数矩阵变换为上三角矩阵就是高斯消元

# 例题

以BZOJ 1013为例

代码如下：

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

int n;
double first[15];
struct gaussian_elimitation {
	int n, m;
	double mat[15][15];
	void swap(int x, int y) {
		for (int i = 1; i <= m; ++i)
			std::swap(mat[x][i], mat[y][i]);
	}

	void times(int x, double k) {
		for (int i = 1; i <= m; ++i)
			mat[x][i] *= k;
	}

	void merge(int x, int y, double k) {
		for (int i = 1; i <= m; ++i)
			mat[x][i] += mat[y][i] * k;
	}

	void solve() {
		for (int i = 1; i <= n; ++i) {
			int now = i;
			for (int j = i + 1; j <= n; ++j)
				if (std::abs(mat[j][i]) > std::abs(mat[now][i]))
					now = j;
			if (i != now) swap(i, now);
			times(i, 1.0 / mat[i][i]);
			for (int j = i + 1; j <= n; ++j)
				merge(j, i, -mat[j][i]);
		}
	}

	void print() {
		for (int i = 1; i <= n; ++i) {
			for (int j = 1; j <= m; ++j)
				printf("%.3lf ", mat[i][j]);
			puts("");
		}
	}
} gauss;

double ans[15];
signed main() {
	n = read();
	for (int i = 1; i <= n; ++i)
		scanf("%lf", &first[i]);
	for (int i = 1; i <= n; ++i) {
		for (int j = 1; j <= n; ++j) {
			double x = 0;
			scanf("%lf", &x);
			gauss.mat[i][j] = 2 * (x - first[j]);
			gauss.mat[i][n + 1] += x * x - first[j] * first[j];
		}
	}
	// for (int i = 1; i <= n; ++i) {
	// 	for (int j = 1; j <= n + 1; ++j)
	// 		printf("%.3lf ", gauss.mat[i][j]);
	// 	puts("");
	// }
	gauss.n = n;
	gauss.m = n + 1;
	gauss.solve();
	for (int i = n; i; --i) {
		for (int j = n; j > i; --j)
			gauss.mat[i][n + 1] -= gauss.mat[i][j] * ans[j];
		ans[i] = gauss.mat[i][n + 1];
	}
	for (int i = 1; i <= n; ++i)
		printf("%.3lf ", ans[i]);
}

```

