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

- 모든 부분 집합 순회하기 
- 