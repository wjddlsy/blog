---
title: SPOJ THRBL
date: 2019-10-11 20:10:57
category: algorithm
---

## 문제 

구간의 최댓값을 찾는 문제 

## 풀이 

```cpp
#include <iostream>
#include <vector>
#include <math.h>

using namespace std;

int st[50001][20];

int main() {
    int N, M;
    cin >> N >> M;

    vector<int> h(N);
    for (auto &hh:h) {
        cin >> hh;
    }

    for (int i = 0; i < N; ++i) {
        st[i][0] = h[i];
    }

    for (int j=1; j<20; ++j) {
        for(int i=0; i+(1<<j)<=N; ++i) {
            st[i][j] = max(st[i][j-1], st[i+(1<<j-1)][j-1]);
        }
    }

    int ret = 0;
    while(M--) {
        int A, B;
        cin>>A>>B;
        A--; B--;

        int comp = h[A];
        if(B-A < 2) {
            ret++;
            continue;
        }
        A++; B--;
        int dist = (int)(log10(B-A+1)/log10(2));
        int maximum = max(st[A][dist], st[B-(1<<dist)+1][dist]);
        if(maximum <= comp)
            ret++;
    }
    cout << ret;
    //std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

문제에서 (A, B) 구간의 최댓값이 h[A]보다 크지 않으면 공을 던지는게 가능하다고 했고 $1\le A, B\le N$ 이다. 

처음에는 구간을 [A, B] 로 착각해서 틀렸다. 그리고 런타임에러가 나서 $A==B$ 일 때를 처리해줬는데 또 런타임에러였다. 다시 생각해보니 $B-A==1$ 일때 `A++; B--;` 에서 A와 B의 값이 역전되면서 `log10(B-A+1) = log10(0)` 이 되고 이는 로그에서 나올 수 없는 값이 된다. 

