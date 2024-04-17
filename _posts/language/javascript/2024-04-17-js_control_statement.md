---
title: JS - 제어문
date: 2024-04-17 11:00:00 +09:00
categories: [Programming Language, Javascript]
tags:
  [
    프로그래밍 언어,
    Javascript,
    제어문
  ]
---

## Ⅰ. 조건문

###  1. if

> 주어진 조건이 참(true)인 경우에만 특정한 코드 블록을 실행

```js
if (condition) {
  // condition이 참일 때 실행되는 코드
} else if (anotherCondition) {
  // anotherCondition이 참일 때 실행되는 코드
} else {
  // 모든 조건이 거짓일 때 실행되는 코드
}
```

`if`
- if 문의 시작
- `condition`이 참이면 블록 안 코드 실행 후 조건문 전체 탈출
- 거짓이면 아래 `else if` 혹은 `else` 문 연산 진행

`else if`
- 위의 `if` 혹은 `else if` 문이 거짓이면 참 거짓 판단

`else`
- 모든 조건이 거짓이면 실행
- 선택적으로 사용 가능

```javascript
const name = 'manager'
if (name==='admin'){
  console.log('관리자님 환영합니다')
}  else {
  console.log(`${name}님 환영합니다`)    
}
```

---

### 2. switch

> 하나의 변수나 표현식의 값을 여러 가지 경우(case)와 비교 일치하는 경우에 해당하는 코드 블록을 실행

```js
switch (expression) {
  case value1:
    // expression이 value1과 일치할 때 실행되는 코드
    break;
  case value2:
    // expression이 value2와 일치할 때 실행되는 코드
    break;
  default:
    // expression과 일치하는 값이 없을 때 실행되는 코드
}
```

`expression`
- 비교할 변수나 표현식
  
`case` : 비교할 값
- 만약 `expression`의 값이 `case` 중 하나와 일치하면, 해당 `case`의 코드 블록이 실행

`break`
- `switch` 블록을 빠져나가기 위해 사용

`default`
- `expression`과 일치하는 `case`가 없을 때 실행될 기본적인 코드 블록을 지정

```js
let fruit = "apple";

switch (fruit) {
  case "banana":
    console.log("바나나를 선택했습니다.");
    break;
  case "apple":
    console.log("사과를 선택했습니다.");
    break;
  case "orange":
    console.log("오렌지를 선택했습니다.");
    break;
  default:
    console.log("다른 과일을 선택했습니다.");
}
```

---
<br>

## Ⅱ. 반복문

###  1. while

> 조건문이 참이기만 하면 문장을 계속해서 수행

```javascript
let i = 0
while (i < 6){
  console.log(i)
  i += 1
}
```

---

### 2. for

> 특정한 조건이 거짓으로 판별될 때까지 반복

보통 특정 횟수 동안 실행문을 반복시키기 위하여 사용

```javascript
for (let i=0; i<6, i++){
  console.log(i)
}
```

---

## 3. for...in

- 객체의 속성을 순회할 때 사용
- 배열도 순회 가능하지만 인덱스 순으로 순회한다는 보장이 없음
- 속성 이름(key)을 통해 반복

```javascript
const fruits = {a:'apple', b:'banana'}
for (const key in fruits){
  console.log(key)
  console.log(fruits[key])
}
// a
// apple
// b
// banana
```

---

## 4. for...of

- 반복가능한 객체를 순회할 때 사용
- 반복가능한(iterable) 객체의 종류 : Array, Set, String 등
- 속성 값(value)을 통해 반복
  
```javascript
const numbers = ['a','b','c']
for (const n in numbers){
  console.log(n)
}
// a
// b
// c
```

---

### 5. Array.forEach()

- 배열의 메서드들 중 하나
- 배열이 가진 각 요소를 순회하면서 함수를 실행
  
```javascript
const numbers = [1,2,3,4,5]
numbers.forEach(function(element){
  console.log(element)  
})
// 1
// 2
// 3
// 4
// 5
```