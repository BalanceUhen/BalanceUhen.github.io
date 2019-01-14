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

