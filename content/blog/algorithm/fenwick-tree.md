---
title: Fenwick Tree
date: 2019-11-07 21:11:38
category: algorithm
---

> 출처: https://cp-algorithms.com/data_structures/fenwick.html

Let, $f$ be some reversible function and A be an array of integers of length N. 

펜윅트리는 다음과 같은 자료구조를 말한다.

- $[l:r]$ 에 대해 함수 $f$ 를 $O(logn)$에 계산할 수 있다. 
- $A$ 의 한 요소를 $O(logn)$에 갱신할 수 있다.
- $O(N)$ 의 메모리를 요구한다. :star: 
- 코드가 짧고 좋다. (multidimensional array에 대해서도)

펜윅트리는 `Binary Indexed Tree` 라고도 부른다. 보통 range sum을 구할 때 많이 사용한다. 

### HOW 

배열 $A[0...N-1]$가 있다고 하자. 펜윅 트리는 배열 $T[0...N-1]$이고 각 요소는 배열 $A$ 의 $[g(i):i]$ 의 합에 대응된다. 

​																	$T_i = \sum^i_{j=g(i)}A_j$  

배열 $T$ 는 배열이지만 매우 :clap: nice 하게 트리로 표현할 수 있다. 펜윅트리에서 필요한 것은 $T$ 뿐이고 모든 쿼리를 처리할 수 있다. 

```pseudocode
// [0:r]까지 sum 
def sum(int r):
	result = 0
	while (r>=0)
		res += t[r]
		r = g(r) - 1
	return res
	
// update A[i] = A[i] + delta
def increase(int i, int delta):
	for all j with g(j) <= i <= j:
		t[j] += delta
```

함수 `sum` 은 다음과 같이 동작한다:

1. `result` 에 $[g(r);r]$ 까지의 합을 더한다. 
2. 그리고 다음 range를 더한다;$[g(g(r)-1):g(r)-1]$
3. 계속해서 $[0;g(g(...(g(r)-1...-1)=1)]$ 에서 $[g(-1);1]$ 까지 더한다. 

함수 `increase` 는 다음과 같이 동작한다. = `update` 

1. 다음을 만족하는 $[g(j);j]$ 까지의 합을 구한다. 

    $g(j) \le i \le j$  를 만족하는 $t[j]$ 를 `delta` 씩 증가시킨다. 즉, `t[j] += delta` . 

`sum` 과 `increase` 의 시간복잡도는 함수 `g` 에 영향을 받는다. 모든 $i$ 에 대하여 $0 \le g(i) \le i$ 를 만족한다면 함수 $g$ 를 선택하는 방법은 많다. $g(i)=i$ 라고 생각해보자. 그러면 $T = A$ 가 될 뿐이다. 그러므로 $g(i)=i$ 를 선택한다면 구간합을 구하는 시간은 여전히 느릴 것이다. 

$g(i) = 0$ 을 함수로 하면 어떨까? 이는 $[0;i]$ 까지 합을 구한다는 의미이므로 익히 알다시피 구간합을 $O(1)$ 에 처리할 수 있다. 하지만 갱신은 느리다. 

펜윅 알고리즘의 좋은 점이 여기 있다. 함수 $g$ 를 특별하게 정의함으로써 구간쿼리와 갱신을 $O(logN)$ 에 처리할수 있다.  

### $g(i)$ 의 정의 

$g(i)$ 는 다음과 같이 정의할 수 있다. 

> i를 2진수로 표현했을 때, 가장 마지막에 나오는 0의 뒤에 있는 1을 모두 0으로 바꾼다. 

$g(11) = g(1011_2) = 1000_2=8$

$g(12)=g(1100_2)=1100_2=12$

$g(13)=g(1101_2)=1100_2=12$

$g(14)=g(1110_2)=1110_2=14$

$g(15)=g(1111_2)=1111_2=0$

이러한 함수 $g$는 다음과 같이 정의할 수 있다. 

​													:star: $g(i)=i \ \&\  (i+1)$ :star:

이제 어떻게 하면 $g(j) \le i \le j$ 인 $j$ 를 어떻게 알고 순회할까? 

생각해보면 쉽다. 우선 $i$ 를 2진수로 표현한다. 그리고나서 차례대로 0인 비트를 1로 바꾼다. 이렇게 되면 0인 비트가 1이 되었기 때문에 $i \le j $ 는 명백하다. 또한 $g(j)$ 는 가장 마지막으로 나오는 0 뒤의 비트를 모두 0을 바꾸기 때문에 $g(j) \le i $ 이다. 

:smile: Case: $i = 10$ 

$\hspace{1.9cm}10 = 0001010_2 \\ h(10) = 11 =0001011_2 \\ h(11) = 15 =0001111_2 \\ h(15)=31=0011111_2 \\ h(31) = 63 = 0111111_2 $

그리고 이러한 $h$ 는 비트연산으로 간단하게 구할 수 있다. 

​													:star: $h(j) = j | (j+1)$ :star:

이러한 연산을 통해 $T$ 는 트리의 형태를 띄게 된다. 

![Binary Indexed Tree](https://raw.githubusercontent.com/e-maxx-eng/e-maxx-eng/master/img/binary_indexed_tree.png)



### Finding sum in one-dimesional array

```cpp
struct FenwickTree {
  vector<int> bit;
  int n;
  
  FenwickTree(int n) {
    this->n = n; 
    bit.assign(n, 0);
  }
  
  FenwickTree(vector<int> a):FenwickTree(a.size()) {
    for(size_t i=0; i<a.size(); ++i) {
      add(i, a[i]);
    }
  }
  
  int sum(int r) {
    int ret = 0;
    for(; r>=0; r = (r & (r+1)) - 1)
      ret+=bit[r];
    return ret; 
  }
  
  int sum(int l, int r) {
    return sum(r) - sum(l-1);
  }
  
  void add(int idx, int delta) {
    for(; idx<n; idx = idx | (idx+1))
      bit[idex] += delta; 
  }
};
```

### Finding minimum of [0;r] in one-dimensional array

펜윅트리를 이용해서 $[l,r]$ 에서 최솟값을 찾는 쉬운 방법은 없다. 펜윅트리는 $[0;r]$ 에 대해서만 알 수 있다. 

```cpp
struct FenwickTreeMin {
  vector<int> bit; 
  int n; 
  const int INF = (int)1e9; 
  
  FenwickTreeMin(int n) {
    this->n = n;
    bit.assing(n, INF); 
  }
  
  FenwickTree(vector<int> a) : FenwickTreeMin(a.size()) {
    for (size_t i = 0; i < a.size(); ++i) {
      update(i, a[i]);
    }
  }
  
  int getMin(int r) {
    int ret = INF;
    for(; r >= 0; r = (r & (r+1)) - 1) {
      ret = min(ret, bit[r]);
    }
    return ret; 
  }
  
  void update(int idx, int val) {
    for(; idx<n; idx = idx | (idx + 1)) {
      bit[idx] = min(bit[idx], val);
    }
  }
}
```

### Finding sum in two-dimensional array

```cpp 
struct FenwickTree2D {
  vector<vector<int>> bit; 
  int n, m; 
  
  FenwickTree2D(int n, m) {
    this->n = n; 
    this->m = m; 
    bit.assign(n, vector<int>(m, 0));
  }
  
  int sum(int x, int y) {
    int ret = 0;
    for(int i=x; i>=0; i = (i & (i+1))-1) {
      for(int j=y; j>=0; j = (j & (j+1))-1) 
        ret += bit[i][j];
    }
    return ret;
  }
  
  void add(int x, int y, int delta) {
    for(int i=x; i<n; i = i | (i+1)) {
      for(int j=y; j<m; j = j | (j+1)) 
        bit[i][j] += delta; 
    }
  }
}
```

### One-based indexing approch

$T[]$ 랑 $g()$ 를 조금 다르게 정의해보자. $T[i]$ 는 $[g(i)+1;i]$ 의 합을 저장한다. 왜 이렇게 할까? 그 이유는 $T$ 를 이렇게 정의하면 $g(i)$ 를 :clap: 하게 정의할 수 있다.

 ```pseudocode
def sum(int r):
	res = 0
	while(r>0):
		res+=t[r]
		r=g(r)
	return res;

def increase(int i, int delta):
	for all j with g(j) < i <= j:
		t[j] += delta
 ```

이제 $g(i)$ 를 다음과 같이 정의할 수 있다. 

> i를 2진수로 표현했을 때 가장 마지막의 1을 0으로 바꾼다. 

$g(7) = g(111_2) = 110_2 = 6$

$g(6) = g(110_2) = 100_2 =4$

$g(5)=g(100_2)=000_2=0$

가장 마지막 비트는 $i\&(-i)$ 로 표현할 수 있기 때문에 $g(i)$ 는 다음과 같이 표현할 수 있다. 

​												:star:$g(i) = i-(i\&(-i))$:star:

또한 $h(i)$ 도 다음과 같이 표현할 수 있다. 

​												:star: $h(i) = i + (i \&(-i))$ :star:

```cpp
struct FenwickTreeOneBasedIndexing {
  vector<int> bit;
  int n; 
  
  FenwickTreeOneBasedIndexing(int n) {
    this->n = n + 1;
    bit.assing(n + 1, 0);
  }
  
  FenwickTreeOneBasedIndexing(vector<int> a) : FenwickTreeOneBasedIndexing(a.size()) {
    init(a.size());
    for(size_t i = 0; i < a.size(); ++i) {
      add(i, a[i]);
    }
  }
  
  int sum(int idx) {
    int ret = 0;
    for(++idx; idx>0; idx -= idx & -idx) 
      ret += bit[idx];
    return ret;
  }
  
  int sum(int l, int r) {
    return sum(r) - sum(l-1);
  }
  
  void add(int idx, int delta) {
    for(++idx; idx<n; idx += idx & -idx)
      bit[idx] += delta; 
  }
};
```



> :confused: 어떻게 `i&(-i)` 가 마지막 비트가 되는지에 대하여 
>
> 비트에서 음수를 표현하는 방법은 [2의 보수](https://ko.wikipedia.org/wiki/2의_보수) 이다. 2의 보수는 어떻게 구할까요. 
>
> 보수란 *두 수의 합이 진법의 밑수가 되게 하는 수* 이다. 컴퓨터는 뺄셈의 개념이 없기 때문에 덧셈을 통해 음수를 구한다. 

### Range operations 

#### 1. Point update and range query 

지금까지 했던 것!

#### 2. Range Update and Point query

#### 3. Range Update and Range query 

