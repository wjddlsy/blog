---
title: SPOJ CRAYON
date: 2019-12-01 18:12:53
category: algorithm
---

> https://www.spoj.com/problems/CRAYON/

### 문제 :disappointed_relieved:

Mary는 그림 그리는 걸 좋아하긴하는데 잘 그리진 못한다. 

Mary가 하는 짓은 세 가지다. 

1. `D L R` : [L, R]에 segment를 그린다. 
2. `C I`: i번째에 있는 segment를 지운다. 즉, i번째에 올라와있는 모든 segment는 제거된다. 
3. `Q L R`: [L, R]에서 최소 하나의 공통 포인트를 공유하는 segment의 개수를 구한다. (즉, 겹치는)

### 풀이

> https://github.com/tr0j4n034/SPOJ/blob/master/CRAYON.cpp

펜윅트리를 이용하여 $O(N \times logN)$ 시간에 풀 수 있다. 

일단 수의 범위가 매우 크기 때문에 데이터를 압축해주어야 한다. 

#### 데이터 압축하기 

Offline 방법을 사용한다.

```cpp
int main() {
  vector<char> type(N); 
  vector<int> l(N), r(N), who(N), c(N); 
  vector<int> data; 
  
  int id = 0;
  for (int i = 0; i < N; ++i) {
      cin >> type[i];
      if (type[i] == 'C') {
          cin >> c[i];
      } else if (type[i] == 'D') {
          cin >> l[i] >> r[i];
          who[++id] = i;
      } else {
          cin >> l[i] >> r[i];
      }
  }
  
  data = vector<int>(l.begin(), l.end()); 
  data.insert(data.end(), r.begin(), r.end()); 
  sort(data.begin(), data.end());
  data.erase(unique(data.begin(), data.end()), data.end());
}
```

:smile: `std::unique`

> Remove consecutive duplicates in range. 범위 내 중복된 원소를 제거한다. 
>
> 실제로 제거하는 것은 아님에 주의. 앞에 유일한 값들을 채워 넣고, 정렬되지 않은 시작 포인터를 반환한다. 또한 unique 함수는 앞과 뒤의 원소들을 비교하기 때문에 반드시 입력 벡터가 정렬된 상태이어야 한다. 
>
> ```cpp
> template <class FowardIterator> ForwardIterator unique (ForwardIterator first, ForwardIterator last) {
>   if (first == last) return last; 
>   
>   ForwardIterator result = first; 
>   while(++first != last) {
>     if(!(*result == *first))
>       *(++result) = *first;
>   }
>   return ++result;
> }
> ```

#### Segment 세기 

문제의 조건은 교차하는 segment의 개수를 구하라는 것이다. 그런데, 교차하는 segment가 아닌 교차하지 않는 segment의 개수를 구한다면 어떨까? 

예를 들어, [l ,r]에서 교차하는 segment의 개수를 구한다고 하자. 그러면 필요한 것은 다음과 같다. 

- 시작점이 r보다 큰 세그먼트의 개수 
- 마지막점이 l보다 작은 세그먼트의 개수 

전체 세그먼트의 개수에서 위의 두 개를 빼주자. :eyes: 오~ 똑똑하다.





