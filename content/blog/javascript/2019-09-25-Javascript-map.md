---
title: "Javascript map"
author: hamster
date: 2019-09-25
category: javascript
---
### map 

> `map()` 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 ==새로운 배열을 반환==합니다. 



### 구문

```javascript
arr.map(callback(currentValue[, index[, array]])[, this.Arg])
```

#### 매개변수 

`callback`

새로운 배열 요소를 생성하는 함수로 다음의 세 가지 인수를 가진다. 

- `currentValue` : 처리할 현재 요소 (required)
- `index`: 처리할 현재 요소의 인덱스 (optional)
- `array`: `map()` 을 호출한 배열 (optional)

`thisArg` 

`callback` 을 실행할 때 `this` 로 사용되는 값 (optional)

#### 반환 값 

배열의 각 요소에 대해 실행한 `callback` 의 결과를 모은 새로운 배열 



### 예제 

