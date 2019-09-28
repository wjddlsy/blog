---
layout: post
title: "Algospot DARPA Grand Challenge"
author: hamster
date: 2019-05-29
category: algorithm
tags: [algospot, algorithm, greedy]
---
## 코드 

### 1. 이분탐색 

``` cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// 결정문제: 정렬되어 있는 locations 중 cameras를 선택해 모든 카메라 간의 간격이
// gap 이상이 되는 방법이 있는지를 반환한다.
bool decision(const vector<double>& location, int cameras, double gap) {
    double limit=-1;
    int installed=0;
    for(int i=0; i<location.size(); ++i){
        if(limit<=location[i]) {
            ++installed;
            limit=location[i]+gap;
        }
    }
    return installed>=cameras;
}

double optimize(const vector<double>& location, int cameras) {
    double lo=0, hi=241;
    for(int it=0; it<100; ++it) {
        double mid=(lo+hi)/2.0;
        if(decision(location, cameras, mid))
            lo=mid;
        else
            hi=mid;
    }
    return lo;
}

int main() {
    int C;
    cin>>C;
    while(C--){
        int N, M;
        cin>>N>>M;
        vector<double> locations(M);
        for(auto &location:locations){
            cin>>location;
        }
        sort(locations.begin(), locations.end());
        printf("%.2f\n", optimize(locations, N));
    }
    //std::cout << "Hello, World!" << std::endl;
    return 0;
}
```



### 2. DP 

> 도대체 어떻게 dp로 푼단말임 

