---
layout: post
author: hamtser
title: "Algospot KAKURO2"
date: 2019-06-07
category: algorithm
---

## 문제 

https://algospot.com/judge/problem/read/KAKURO2



## 풀이 

### 제약 충족 문제 (Constraint Satisfaction Problem)

> 카쿠로와 같이 특정한 제약에 해당하는 답을 찾는 문제들을 제약 충족 문제라고 함 

- 이러한 문제의 경우 더 좋은 답과 더 나쁜 답의 개념이 없기 때문에 최적화 문제를 풀 때 썼던 많은 기법들은 해당하지 않음 
- 크게 두 가지를 신경써서 풀게 된다. 

#### 1. 제약 전파 

> 답의 일부를 생성하고 나면 문제의 조건에 의해 다른 조각의 답에 대해 알게 되는 것 

이를 이용하면 실제 탐색해야 할 탐색 공간의 수를 크게 줄일 수 있다. 

#### 2. 채울 순서 정하기 

채울 순서를 잘 정하면 제약 전파를 더 용이하게 할 수 있을 뿐만 아니라 답을 더 빨리 찾을 확률을 높여준다. 채울 순서 정하기는 사실 상 변수 순서 정하기와 값 순서 정하기를 합쳐 부르는 말이다. 

- 변수 순서 정하기: 어느 빈 칸부터 채워나갈지 결정하는 문제 
- 값 순서 정하기: 이 빈 칸에 어떤 숫자부터 채워나갈 것인지 



### 후보의 수 계산하기 

변수 순서 정하기를 위해 해당 칸에 들어갈 수 있는 후보의 수가 가장 작은 칸부터 채워나가자. 

- `getCandidates(len, sum, known[])`

  : 연속된 len개의 흰 칸에 적힌 수 합이 sum이어야 하는데, 그중 이미 적힌 숫자의 목록이 known[]일 때, 남은 칸에 들어갈 수 있는 숫자의 집합 

`getCandidates` 는 어떻게 구현할 수 있을까? 

==[1,2,…,9]의 부분 집합 중 크기가 len인 집합을 모두 생성해 보고 이 중 known을 모두 포함하며, 그 합이 sum인 집합을 모두 생성해 본다. 그리고 이 집합들의 합집합에서 known에 속한 원소들을 제외하자.==

```cpp
// mask에 속한 원소들의 개수를 반환한다. 
int getSize(int mask); 
// mask에 속한 원소들의 합을 반환한다. 
int getSum(int mask);
// len 칸의 합이 sum이고, 이 칸들에 이미 쓰인 수의 집합이 known일 때 
// 나머지 칸에 들어갈 수 있는 숫자의 집합을 반환 
int getCandidates(int len, int sum, int known) {
  // 조건에 부합하는 집합들의 교집합
  int allSets=0; 
  // 1~9의 부분집합(즉 모두 짝수)을 모두 생성하고 그 중 
  for(int set=0; set<1024; set+=2) {
    // known을 포함하고, 크기가 len이며 합이 sum인 집합을 모두 찾는다 
    if((set&known)==known && getSize(set)==len && getSum(set)==sum) {
      allSets |= set;
    }
  }  
  return allSets & ~known;
}
```

### 후보의 수 빠르게 계산하기 

탐색 중간에 `getCandidates` 를 호출하기에는 너무 많은 연산이 필요하다. 이런 경우 `getCandidates` 의 같은 값을 두 번 이상 계산하지 않도록 메모이제이션하거나, 탐색을 수행하기 전에 미리 계산해 두면 아주 유리하다.

```cpp
// candidates[len][sum][known]=getCandidates(len,sum,known)
int candidates[10][46][1024];
void generateCandidates() {
  memset(candidates, 0, sizeof(candidates));
  // 1~9의 부분집합을 모두 생성
  for(int set=0; set<1024; set+=2) {
    int l=getSize(set), s=getSum(set);
    int subset=set;
    while(true) {
      // 숫자 하나의 합이 s이고, 이미 subset의 숫자가 쓰여 있을 때 
      // 전체 숫자의 집합이 set이 되도록 나머지 숫자를 채워넣을 수 있다. 
      candidates[l][s][subset]|=(set & ~subset);
      if(subset==0) break; 
      subset = (subset-1) & set;
    }
  }
}
```

이제 칸의 좌표 (y, x)가 주어졌을 때 후보의 수를 어떻게 계산할까?

1. (y, x)에서 힌트 칸이 나올 때까지 위로 쭉 올라가서 힌트 칸을 찾은 뒤, 위 아래로 움직이면서 흰 칸의 수와 이미 쓰인 숫자들의 집합을 구한다. 

   -> 너무 오래걸림 

2. 각 칸은 가로 힌트와 세로 힌트 두 개에 해당되므로, ==각 칸마다 이 칸이 속하는 힌트 두 개의 번호를 배열에 미리 저장해둔다==. 그리고 각 힌트에 대해 해당되는 흰 칸의 수, 이미 쓰인 숫자들의 집합을 탐색하는 과정에서 유지 

```cpp 
// 게임판의 정보 
// color: 각 칸의 색깔 (0=검은 칸 혹은 힌트 칸, 1=흰칸)
// value: 각 흰 칸에 쓴 숫자 (아직 쓰지 않은 칸은 0)
// hint: 각 칸에 해당하는두 힌트의 번호 
int n, color[MAXN][MAXN], value[MAXN][MAXN], hint[MAXN][MAXN][2];
// 각 힌트의 정보 
// sum: 힌트 칸에 쓰인 숫자 
// length: 힌트 칸에 해당하는 흰 칸의 수 
// known: 힌트 칸에 해당하는 흰 칸에 쓰인 숫자들의 집합 
int q, sum[MAXN*MAXN], length[MAXN*MAXN], known[MAXN*MAXN];
// (y, x)에 val을 쓴다. 
void put(int y, int x, int val) {
  for(int h=0; h<2; ++h){
    known[hint[y][x][h]]+=(1<<val);
  }
  value[y][x]=val;
}
// (y, x)에 쓴 val을 지운다 
void remove(int y, int x, int val){
  for(int h=0; h<2; ++h){
    known[hint[y][x][h]]-=(1<<val);
  }
  value[y][x]=0;
}
// 힌트 번호가 주어질 때 후보의 집합을 반환 
int getCandHint(int hint){
  return candidates[length[hint]][sum[hint]][known[hint]];
}
// 좌표가 주어질 때 해당 칸에 들어갈 수 있는 후보의 집합을 반환 
int getCandCoord(int y, int x){
  return getCandHint(hint[y][x][0]) & getCandHint(hint[y][x][1]);
}
```

### 실제 탐색 

위와 같이 구현하면 실제 탐색 과정에서는 게임판에 대한 정보는 거의 필요가 없다. 

```cpp
void printSolution();
// 답을 찾았으면 true, 아니면 false
bool search() {
  // 아직 숫자를 쓰지 않은 흰 칸 중 후보의 수가 최소인 칸을 찾는다. 
  int y=-1, x=-1, minCands=1023;
  for(int i=0; i<n; ++i){
    for(int j=0; j<n; ++j){
      if(color[i][j]==WHITE && value[i][j]==0) {
        int cands=getCandCoord(i, j);
        if(getSize(minCands)>getSize(cands)){
          minCands=cands;
          y=i; x=j;
        }
      }
    }
  }
  // 이 칸에 들어갈 숫자가 없으면 실패 
  if(minCands==0) return false; 
  if(y==-1) {
    printSolution();
    return true; 
  }
  for(int val=1; val<=9; ++val){
    if(minCands & (1<<val)) {
      put(y, x, val);
      if(search()) return true;
      remove(y, x, val);
    }
  }
  return false; 
}
```

