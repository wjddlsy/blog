---
title: SPOJ TRIPINV
date: 2019-11-30 12:11:69
category: algorithm
---

> https://www.spoj.com/problems/TRIPINV/

### 문제 

한 수열이 주어졌을 때, ai > aj > ak ( i < j < k ) 인 (i, j, k)의 개수를 구하라. 

### 풀이

> :disappointed_relieved: http://codeforces.com/blog/entry/18525

2개의 펜윅트리를 사용해서 풀 수 있다. 

펜윅트리 ft1[x]는 x보다 큰 수의 개수를 저장하고 펜윅트리 ft2[x]는 $i<j, A[i]>A[j]$ 인 개수를 기록한다. 

