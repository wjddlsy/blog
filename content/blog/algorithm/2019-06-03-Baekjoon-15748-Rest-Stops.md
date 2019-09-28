---
layout: post
author: hamster
title: "백준 15984 Rest Stops"
date: 2019-06-03
category: algorithm
---

# 15984 Rest Stops

https://www.acmicpc.net/problem/15984

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int L, N, Rf, Rb;
    cin>>L>>N>>Rf>>Rb;
    vector<pair<int, int>> stops(N);
    for(int i=0; i<N; ++i){
        int x, c;
        cin>>x>>c;
        stops[i]={c, x};
    }
    
    sort(stops.begin(), stops.end(), greater<>());

    long long ret=0, pos=0;
    for(auto &stop:stops) {
        if(pos>stop.second) {
            continue;
        }
        else {
            ret+=(Rf-Rb)*(stop.second-pos)*stop.first;
            pos=stop.second;
        }
    }
    cout<<ret;
    //std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

