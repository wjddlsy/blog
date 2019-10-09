---
title: Minimum Stack, Queue
date: 2019-10-09 19:10:40
category: algorithm
---

> https://cp-algorithms.com/data_structures/stack_queue_modification.html

- 스택에서 최솟값을 무려 $O(1)$ 에 찾을 수 있도록 스택을 수정할 수 있다. 

- 큐에서도 가능하다. 

## Stack modification

생각보다 간단하다. 스택에 인자를 `pair` 로 두고 `second`에 최솟값을 넣어둔다. 

```cpp
stack<pair<int, int>> st; 

// add 
int new_min = st.empty()?new_elem:min(new_elem, st.top().second);
st.push({new_elem, new_min});

// remove
int removed_element = st.top().first;
st.pop();

// find 
int minimum = st.top().second;
```



## Queue modification 

### 1

기본 아이디어는 최솟값을 결정하는 데 필요한 원소만 큐에 저장하는 것이다. 즉, 큐에 감소하지 않는 순서대로 저장이 되어있다. 주의해야 하는 건 삭제할 때인데 맨 앞에 있는 원소가 실제로 삭제하고 싶은 원소가 아닐 수 있다.

이 방법의 문제점은 실제 원소를 저장하지 않는다는 점이다. 

```cpp
deque<int> q; 

//find
int minimum = q.front();

//add 
while(!q.empty() && q.back() > new_element) 
  q.pop_back();
q.push_back(new_element);

//remove
if(!q.empty() && q.front() == remove_element)
  q.pop_front();
```

### 2

어떤 걸 지워야 할지 알 수가 없기 때문에 큐의 각 원소에 인덱스까지 저장한다. 또한 몇 개의 원소가 추가되고 삭제되었는지 기록해 놓는다. 

```cpp
deque<pair<int, int>> q;
int cnt_added = 0;
int cnt_removed = 0;

//find
int minimum = q.front().first;

//add
while(!q.empty() && q.back().first > new_element)
  q.pop_back();
q.push({new_element, cnt_added});
cnt_added++;

//remove
if(!q.empty() && q.front().second == cnt_removed)
  q.pop_front();
cnt_removed++;
```

### 3

이 방법은 실제 원소를 저장할 수 있다.

이 방법의 아이디어는 스택에서 썼던 방법을 기반으로 한다. 따라서 여기서 필요한 것은 두 개의 스택으로 큐를 표현하는 방법이다. 

스택 `s1`, `s2` 를 만들건데 이 스택은 최솟값을 $O(1)$ 에 찾을 수 있도록 수정한 스택이다. 새로운 원소는 `s1` 에 추가하고 `s2` 는 원소를 삭제한다. `s2` 가 비었으면 모든 원소를 `s1` 에서 `s2` 로 옮긴다. 이렇게 하면 순서가 뒤집힌다. 마지막으로 최솟값을 찾으려면 두 스택에서 최솟값을 구한다. 

```cpp
stack<pair<int, int>> s1, s2;

//find
//둘다 비었으면 어쩌려고
if(s1.empty() && s2.empty())
  minimum = .. 
if(s1.empty() || s2.empty())
  minimum = s1.empty() ? s2.top().second : s1.top().second;
else 
  minimum = min(s1.top().second, s2.top().second);

//add
int minimum = s1.empty() ? new_element : min(new_element, s1.top().second);
s1.push({new_element, minimum});

//remove
if(s2.empty()) {
  while(!s1.empty()) {
    int element = s1.top().first;
    s1.pop();
    int minimum = s2.empty() ? element : min(element, st2.top().second);
    s2.push({element, minimum});
  }
}
int remove_element = s2.top().first;
s2.pop();
```



## 고정된 길이의 subarray에서 최솟값 찾기 

​				$min_{0\le i \le M-1}A[i], min_{1 \le i \le M}A[i], ...$

위에 소개된 3개의 방법을 모두 사용할 수 있다. 시간 복잡도는 $O(n)$ 이 된다. 

슬라이딩 윈도우같은 기법. 