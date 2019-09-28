---
title: "cpp File cin/cout"
category: algorithm
date: 2019-06-16
---

# File cin/cout

https://stackoverflow.com/questions/10150468/how-to-redirect-cin-and-cout-to-files

```cpp
#include <bits/stdcpp.h>

using namespace std;

int main() {
  ifstream cin("input.txt");
  ofstream cout("output.txt");
  
  int a, b;
  cin>>a>>b;
  cout<<a+b<<endl;
}
```

