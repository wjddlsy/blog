---
title: Sqrt Decomposition
date: 2019-12-11 19:12:11
category: algorithm
---

> https://cp-algorithms.com/data_structures/sqrt_decomposition.html

`Sqrt Decomposition` 은 어떤 operation 을 $O(\sqrt N)$ 에 수행할 수 있는 자료구조이다. 

### Sqrt Decomposition based data structure 

배열 $a[0 ... n-1]$ 에서 $a[l...r]$ 의 합을 $O(\sqrt N)$ 에 찾을 수 있는 자료구조를 만들어보자. 

`sqrt decomposition` 의 기본적인 생각은 *전처리* 이다. 어떤 전처리를 할거냐면...

먼저, 배열을 $\sqrt N$ 개로 쪼개버린다. 그리고 각 블록을 $b[i]$ 라하고 미리 $b[i]$ 의 합을 구해 놓는다. 이렇게 하면 블록의 개수와 크기는 `ceil(sqrt(N))` 이다. 

#### 구현

```cpp
// preprocessing
int len = (int)sqrt(n + .0) + 1; 
vector<int> b(len); 
for(int i=0; i<n; ++i) {
  b[i/len] += a[i];
}

// query
int sum = 0;
int c_l = l/len, c_r = r/len; 
// case 1: l, r이 한 블록에 있는 경우
if(c_l == c_r) {
  for (int i=l; i<=r; ++i) {
    sum += a[i]; 
  }
} else {
  // case 2: 보통의 경우
  for (int i=l, end=(c_l+1)*len-1; i<=end; ++i)
    sum += a[i]; 
  for (int i=c_1+1; i<=c_r-1; ++i)
    sum += b[i];
  for (int i=c_r*len; i<=r; ++i) 
    sum += a[i]; 
}
```

#### Update operation 

이제 구간합을 $O(\sqrt N)$ 에 구할 수 있다는 것은 알았는데... 구간합만 잘 구하는 자료구조라면 부분합보다 좋을 게 없다. 당연히 배열의 값을 갱신할 수 있는 좋은 방법이 존재한다. 

$a[i]$ 의 값이 바뀌면 배열 $b$ 는 어떻게 바뀌어야 할까? $a[i]$ 가 속하는 $b[k]$ 의 값을 다음과 같이 변경해주면 된다. 

​															$b[k]+= a_{new}[i] - a_{old}[i]$ 

#### RMQ 

그렇다면 `sqrt decomposition` 을 이용해서 구간의 최대/최솟값을 구할 수 있을까? 

구간합을 구할 때처럼 RMQ는 쉽게 된다. 그런데 갱신은 어떻게 할까? 갱신할 때, 구간합처럼 $O(1)$ 에 할수는 없기 때문에 갱신된 원소가 속한 블록을 순회하며 업데이트해야한다. 따라서 $O(\sqrt N)$ 이 걸린다. 

#### Update array elements on intervals 





### Mo's algorithm 

