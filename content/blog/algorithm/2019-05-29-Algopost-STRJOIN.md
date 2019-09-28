---
title: "Algospot STRJOIN"
author: hamster
date: 2019-05-29
category: algorithm
tags: [algospot, algorithm, greedy]
---

## 문제 

https://algospot.com/judge/problem/read/STRJOIN

단어들의 길이가 주어지면 모두 합치는데 드는 비용을 최소화하는 문제이다. (순서는 상관없음)

예를 들어 {al, go, spot} 이 있으면  

1. ( al + go ) + ( spot ) 의 순서로 합치면 2+2+4+4=12 의 비용이 든다. 
2. ( al + spot ) + ( go ) 의 순서로 합치면 2+4+6+2=14 의 비용이 든다. 

이로 부터 한 가지 사실을 알 수 있는데 길이가 긴 단어가 먼저 합쳐져서 좋을 게 없다는 것이다. 그 이유는 긴 단어가 합쳐지면 더 긴 단어가 되는데 그 단어는 앞으로도 계속 합쳐지는데 비용이 소모되기 때문이다. 



간단하게 탐욕적 방법으로 해결할 수 있다. 어차피 단어를 합치는 횟수는 정해져있기 때문에 단어를 합칠 때 가장 짧은 단어 두 개를 골라 합치는 것이다. 문제의 예시로 있는 { 1, 1, 3, 3, 4 } 를 풀어보자. 

- 먼저 가장 짧은 길이인 1, 1 을 골라 2를 만든다. —> { 2, 3, 3, 4 }

- 2 + 3 = 5 —> { 5, 3, 4 } —> { 3, 4, 5 }
- 3 + 4 = 7 —> { 7 , 5 } —> { 5, 7 }
- 5 + 7 = 12 —> { 12 }

따라서 2 + 5 + 7 + 12 = 26 

## 코드 

```cpp
#include <iostream>
#include <vector>
#include <set>

using namespace std;
int main() {
    int C;
    cin>>C;
    while(C--){
        int n;
        cin>>n;
        multiset<int> words;
        long long ret=0;
        for(int i=0; i<n; ++i){
            int word; cin>>word;
            words.insert(word);
        }

        while(words.size()>1){
            int sum=0;
            sum += *words.begin();
            words.erase(words.begin());
            sum += *words.begin();
            words.erase(words.begin());

            ret+=sum;
            words.insert(sum);
        }
        cout<<ret<<'\n';
    }
    //std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

_priority queue 를 썼다면 더 좋았을 듯_

