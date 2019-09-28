---
layout: post
title: "에라토스테네스의 체"
date: 2019-07-08
---



# 에라토스테네스의 체 

> 소수를 판별하는 알고리즘

에라토스테네스의 체는 소수 판별 알고리즘입니다. 

[https://ko.wikipedia.org/wiki/에라토스테네스의_체](https://ko.wikipedia.org/wiki/에라토스테네스의_체 ) 

```cpp 
void eratos(int n) {
  if(n<=1) 
    return; 
  vector<bool> isPrime(n+1, true);
  for(int i=2; i*i<=n; ++i) {
    if(isPrime[i]) {
      for(int j=i*2; j<=n; j+=i)
        isPrime[j]=false;
    }
  }
}
```

