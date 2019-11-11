---
title: Fenwick Tree
date: 2019-11-07 21:11:38
category: algorithm
---

> https://cp-algorithms.com/data_structures/fenwick.html

Let, $f$ be some reversible function and A be an array of integers of length N. 

펜윅트리는 다음과 같은 자료구조를 말한다.

- $[l:r]$ 에 대해 함수 $f$ 를 $O(logn)$에 계산할 수 있다. 
- $A$ 의 한 요소를 $O(logn)$에 갱신할 수 있다.
- $O(N)$ 의 메모리를 요구한다. 
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



함수 `increase` 는 다음과 같이 동작한다. 

1. 



`sum` 과 `increase` 의 시간복잡도는 함수 `g` 에 영향을 받는다. 따라서, 

### $g(i)$ 의 정의 

$g(i)$ 는 다음과 같이 정의할 수 있다. 

> i를 2진수로 표현했을 때, 가장 마지막에 나오는 0의 뒤에 있는 1을 모두 0으로 바꾼다. 

$g(11) = g(1011_2) = 1000_2=8$

$g(12)=g(1100_2)=1100_2=12$

$g(13)=g(1101_2)=1100_2=12$

$g(14)=g(1110_2)=1110_2=14$

$g(15)=g(1111_2)=1111_2=0$

이러한 함수 $g$는 다음과 같이 정의할 수 있다. 

$g(i)=i \ \&\  (i+1)$



이제 $g$ 를 표현했으니 이제 어떻게 $j$ 를 순회할 수 있을지를 알아야한다. 



### 1차원 구현 

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

