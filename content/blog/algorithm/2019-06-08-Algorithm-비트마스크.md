---
layout: post
author: hamster
date: 2019-06-08
title: "비트마스크"
category: algorithm
---

> C/C++에서 사용할 수 있는 비트마스크 기법 

#### 1. 공집합과 꽉 찬 집합 구하기 

공집합은 당연하게도 0으로 표현가능하다. 그렇다면 꽉 찬 집합은?

```cpp
int full = (1<<n)-1;
```

$1111_2 = 10000_2 - 1$



#### 2. 원소 추가 

```cpp 
mask |= (1<<p);
```

#### 3. 원소의 포함 여부 확인 

```cpp
bool include= (mask & (1<<p));
```

#### 4. 원소의 삭제 

```cpp
mask &= ~(1<<p);
```

#### 5. 원소의 토글 

```cpp
mask ^= (1<<p);
```

#### 6. 집합 연산 

```cpp 
int added = (a|b);
int intersection = (a&b);
int removed = (a&~b);
int toggled = (a^b);
```



#### 7. 집합의 크기 구하기 

```cpp
int bitCount(int x) {
  if(x==0) return 0; 
  return x%2 + bitCount(x/2);
}

// 또는 gcc 에서 제공하는 함수 사용 
int bitCount(int x) {
  return __builtin_popcount(x);
}
```

#### 8. 최소 원소 찾기 

```cpp
int minimum = __builtin_ctz(x);

// 최하위 비트 직접 구하기 
int firstTopping = (topping & -toppings);
```

#### 9. 최소 원소 지우기 

```cpp
toppings &= (toppings - 1);
```



#### 10. 모든 부분 집합 순회하기 

```cpp
for(int subset = pizza; subset; subset=((subset-1) & pizza))
```

