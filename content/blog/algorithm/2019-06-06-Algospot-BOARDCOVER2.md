---
layout: post
author: hamster
date: 2019-06-06
title: "Algopsot BOARDCOVER2"
category: algorithm
---

## 문제 

https://algospot.com/judge/problem/read/BOARDCOVER2



## 풀이 

### 전처리 

- 블록의 네 가지 회전 형태를 미리 계산 

  (i, j) -> (j, n-i-1) 

  ```cpp
  vector<vector<pair<int, int>> rotations;
  // 2차원 배열 arr을 시계방향으로 90도 돌린 결과를 반환 
  vector<string> rotate(const vector<string>& arr) {
    vector<string> ret(arr[0].size(), string(arr.size(), ' '));
    for(int i=0; i<arr.size(); ++i){
      for(int j=0; j<arr[0].size(); ++j){
        ret[j][arr.size()-i-1]=arr[i][j];
      }
    }
    return ret; 
  }
  //block의 네 가지 회전 형태를 만들고 이들을 상대 좌표의 목록으로 변환 
  void generateRotation(vector<string> block) {
    rotations.clear();
    rotations.resize(4);
    for(int rot=0; rot<4; ++rot){
      int originY=-1, originX=-1;
      for(int i=0; i<block.size(); ++i){
        for(int j=0; j<block[i].size(); ++j){
          if(originY==-1) {
            originY=i;
            originX=j;
          }
          rotations[rot].emplace_back(i-originY, j-originX); 
        }
      }
    }
    sort(rotations.begin(), rotations.end());
    rotations.erase(unique(rotations.begin(), rotations.end()), rotations.end());
    blockSize=rotations[0].size();
  }
  ```

- 게임판 덮기 

  ```C++
  bool set(int y, int x, const vector<pair<int, int>>& block, int delta) {
    bool flag=true;
    for()
  }
  void search(int placed) {
    int y=-1, x=-1; 
    for(int i=0; i<H; ++i){
      for(int j=0; j<W; ++j){
        if(covered[i][j]==0) {
          y=i;
          x=j;
          break; 
        }
        if(y!=-1)
          break;
      }
    }
    // 기저사례: 게임의 모든 칸을 처리한 경우
    if(y==-1) {
      best=max(best, placed);
      return;
    }
    // 이 칸을 덮는다. 
    for(int i=0; i<rotations.size(); ++i){
      if(set(y, x, rotations[i], 1))
        search(placed+1);
      set(y, x, rotations[i], -1);
    }
    covered[y][x]=1;
    search(placed);
    covered[y][x]=0;
  }
  ```

  

- 최적의 답 찾기 

  ```cpp
  int solve() {
    best=0;
    for(int i=0; i<H; ++i){
      for(int j=0; j<W; ++j){
        covered[i][j]=(board[i][j]=='#'?1:0);
      }
    }
    search(0);
    return best;
  }
  ```

  