---
title: SPOJ DQUERY
date: 2019-11-24 10:11:41
category: algorithm
---

> :disappointed:

### 문제 



### Code 

```cpp
#include <iostream>
#include <vector>
#include <map>
#include <tuple>
#include <algorithm>
using namespace std;

const int MAX = 300001;

struct FenwickTree {
    vector<int> bit;  // binary indexed tree
    int n;

    FenwickTree(int n) {
        this->n = n;
        bit.assign(n, 0);
    }

    FenwickTree(vector<int> a) : FenwickTree(a.size()) {
        for (size_t i = 0; i < a.size(); i++)
            add(i, a[i]);
    }

    int sum(int r) {
        int ret = 0;
        for (; r >= 0; r = (r & (r + 1)) - 1)
            ret += bit[r];
        return ret;
    }

    int sum(int l, int r) {
        return sum(r) - sum(l - 1);
    }

    void add(int idx, int delta) {
        for (; idx < n; idx = idx | (idx + 1))
            bit[idx] += delta;
    }
};

int main() {
    int n;
    cin >> n;
    vector<int> arr(n);
    for (auto &a:arr) {
        cin >> a;
    }
    int q;
    cin >> q;
    vector<tuple<int, int, int>> queries(q);
    for(int i=0; i<q; ++i) {
        int x, y;
        cin>>x>>y;
        queries[i] = {y-1, x-1, i};
    }
    sort(queries.begin(), queries.end());
    FenwickTree ft(n);
    map<int, int> m;
    vector<int> ans(q);

    int idx = 0;
    for(int i=0; i<n; ++i) {
        if(m.find(arr[i]) != m.end())
            ft.add(m[arr[i]]-1, -1);
        m[arr[i]] = i + 1;
        ft.add(i, 1);
        while(idx<q && get<0>(queries[idx])==i) {
            ans[get<2>(queries[idx])] = ft.sum(get<1>(queries[idx]), get<0>(queries[idx]));
            idx++;
        }
    }

    for(auto &a:ans) {
        cout << a<< '\n';
    }
    //std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

