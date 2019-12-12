---
title: Codeforces Subsequences
date: 2019-12-02 21:12:07
category: algorithm
---

> https://codeforces.com/contest/597/problem/C

### 문제 :disappointed_relieved:

### 풀이

> https://stackoverflow.com/questions/15057591/how-to-find-the-total-number-of-increasing-sub-sequences-of-certain-length-with

왜 맨날해도 모르겠지... 

`dp[i, j] = number of increasing subsequnces of length j that end at i`

동적계획법을 통해 $O(n^2\times k)$ 에 해결할 수 있다. 

```pseudocode
for i = 1 to n 
  dp[i, 1] = 1

for i = 1 to n
	for j = 1 to i - 1 
		if array[i] > arr[j]
			for p = 2 to k 
				dp[i, p] += dp[j, p - 1]
```

그러나 $O(n^2 \times k)$ 면 시간초과일 수 밖에 

> https://codeforces.com/blog/entry/52716?locale=en

