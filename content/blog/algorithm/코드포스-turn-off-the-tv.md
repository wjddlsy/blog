---
title: 코드포스 Turn off the TV
date: 2019-10-23 21:10:36
category: algorithm
---

## 문제 

> https://codeforces.com/contest/863/problem/E

티비를 끌건데 어떤 티비를 꺼야 최소 하나의 티비는 작동하고 있을 것인가? 

## 풀이 

> 너무 어려워 :disappointed:

문제를 푸는 방법은 다음과 같다. 

1. 각 티비의 [l, r]에 대하여 

   ```cpp 
   const int MAX = 10e9;
   int a[MAX+1];
   
   for(auto& tv:tvs) {
     a[tv.l]++;
     a[tv.r+1]--;
   }
   ```

2. 배열 `a`에 대하여 `psum` 을 구할 수 있다.

   ```cpp
   int psum[MAX+1];
   
   psum[0] = a[0];
   for(int i=1; i<=MAX; ++i) {
     psum[i]+=psum[i-1];
   }
   
   ```

   이렇게 구한 `psum[i]` 는 `i` 순간에 켜져있는 tv의 개수가 된다. 

   이때, `pref[i]` 를  `i` 번째 순간까지 봤을 때 켜져 있는 tv가 하나였던 순간의 개수라고 하자. 그러면 `pref[i]` 는 다음과 같이 구할 수 있다. 

3. 