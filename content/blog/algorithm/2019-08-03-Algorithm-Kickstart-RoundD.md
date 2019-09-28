---
title: "2019 Kickstart Round D"
category: algorithm
date: 2019-08-03
---
### X or What ? 

#### Test set 2 

*xor-even* and *xor-odd*

1. *xor-even* xor *xor-odd* = *xor-odd*
2. *xor-even* xor *xor-even* = *xor-even* and vice versa. 



**Proof.**

1. ***xor-even* xor *xor-odd* = *xor-odd***

   1) even > odd 

   - 1이 전부 겹치는 경우 

     : even - odd = odd 

   - 1이 짝수개 겹치는 경우 

     : even + odd = odd 

   - 1이 홀수개 겹치는 경우 

     : odd + even = odd 

   2) odd > even 

   - 1이 전부 겹치는 경우 

     : odd - even = odd 

   - 1이 짝수개 겹치는 경우 

     : odd + even = even 

   - 1이 홀수개 겹치는 경우 

     : even + odd = odd 

   3) odd = even

2. ***xor-odd* xor *xor-odd* = *xor-even* and vice versa. **

   - 1이 전부 겹치는 경우 

     : odd - odd = even 

   - 1이 홀수개 겹치는 경우 

     : even + even = even 

   - 1이 짝수개 겹치는 경우 

     : odd + odd = even 

따라서 *xor-odd* 가 구간에 짝수개 있다면 그 구간은 *xor-even* 이다. 반대도 마찬가지이다. 이는 배열에 xor-odd가 짝수개 있다면 전체 배열을 xor-even이 된다는 것을 의미

그렇지 않다면 (xor-odd가 홀수개 있다면) 전체 구간에서 두 부분을 생각할 수 있다. 

1) 가장 처음 나오는 odd 바로 뒤부터 마지막 원소까지의 구간 

2) 첫 번째 원소부터 가장 마지막으로 나오는 odd 바로 전까지 구간 

왜냐하면 odd가 짝수개여야 그 구간이 xor-even이 될 수 있기 때문이다. 그래서 이제 odd의 위치를 set에 저장하고 쿼리마다 set에 연산을 통해 처음 odd와 마지막 odd의 위치를 알 수 있다. 

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <set>
using namespace std;

int main() {
    int T;
    cin>>T;
    for(int t=1; t<=T; ++t){
        cout<<"Case #"<<t<<": ";
        int N, Q;
        cin>>N>>Q;
        vector<int> arr(N);
        set<int> odd;
        for(int i=0; i<N; ++i) {
            cin>>arr[i];
            if(__builtin_popcount(arr[i])%2==1)
                odd.insert(i);
        }
        for(int i=0; i<Q; ++i) {
            int p, v;
            cin>>p>>v;
            odd.erase(p);
            if(__builtin_popcount(v)%2==1) {
                odd.insert(p);
            }
            if(odd.size()%2==0)
                cout<<N<<" ";
            else
                cout<<max(*odd.rbegin(), N-*odd.begin()-1)<<" ";
        }
        cout<<'\n';
    }
    //std::cout << "Hello, World!" << std::endl;
    return 0;
}
```



