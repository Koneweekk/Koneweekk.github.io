---
title: JS - 함수
date: 2024-04-17 11:40:00 +09:00
categories: [Programming Language, Javascript]
tags:
  [
    프로그래밍 언어,
    Javascript,
    함수
  ]
---

## Ⅰ. 함수의 정의

### 1. 함수 선언식(Function declaration)

> 일반적인 프로그래밍 언어의 함수 정의 방식

```javascript
function add(n1, n2){
  return n1 + n2
}
```
---

### 2. 함수 표현식(Function expression)

> 표현식 내에서 함수를 정의하는 방식

- 함수 표현식은 함수의 이름을 생략한 익명 함수로 정의 가능
- 표현식에서 함수 이름을 명시하는 것도 가능 (디버깅 용도로 사용)

```javascript
const sub = function(n1, n2) {
  return n1 + n2
}
```

---

### 3. 화살표 함수 (Arrow Function)

- 함수를 간결하게 정의할 수 있는 문법
- function 키워드와 중괄호를 이용한 구문을 짧게 사용하기 위해 탄생
- 화살표 함수는 항상 익명 함수

```javascript
// 1. function 키워드 삭제
const arrow1 = (name, age) => {return `hello ${name} ${age}세`}
// 2. () 생략 가능
const arrow2 = name => {return `hello ${name}`}
// 3. 바디가 표현식 1개일 경우 {} & return 생략 가능
const arrow2 = name => `hello ${name}`
// 4. 인자가 없다면 () or _로 표시 가능
let noArgs = () => 'No args'
let noArgs = _ => 'No args'
// 5-1. object를 return할 때
let returnObject = () => {return {key: 'value'}}
// 5-2. return을 적지 않으려면 () 사용
let returnObject = () => ({key: 'value'})
```

---

### 4. 선언식과 표현식 정리

||선언식|표현식|
|---|---|---|
|타입|function 타입|function 타입|
|구성요소|이름, 매개변수, 바디|이름, 매개변수, 바디|
익명함수|불가능|가능|
호이스팅|있음|없음|

---
<br>

## Ⅱ. 함수의 매개변수

### 1. 기본 인자(Default arguments)

- 인자 작성시 `=` 문자 뒤 기본 인자 선언 가능
  
```javascript
const greeting = function(name='Anonymous') {
  return `Hi ${name}`
}
```

---

### 2. 매개변수와 인자 개수 불일치 허용

매개변수보다 인자의 개수가 많을 경우
  
```javascript
const noArgs = function() {
  return 0
}
noArgs(1, 2, 3) // 0
```

매개변수보다 인자의 개수가 적을 경우

```javascript
const noArgs = function(n1, n2, n3) {
  return [n1, n2, n3]
}
noArgs() // [undefined, undefined, undefined]
```

---

### 3. 전개 구문(Spread syntax)

- 배열이나 문자열과 같이 반복 가능한 객체를 배열의 경우는 요소, 함수의 경우는 인자로 확장할 수 있음
- 배열과의 사용하는 경우가 많다.

```javascript
const restOpr = function(arg1, arg2, ...restArgs) {
  return [arg1, arg2, restArgs]
}
restOpr(1 ,2, 3, 4, 5)
//[1, 2, [3, 4, 5]]
```

---
<br>


## Ⅲ. this

### 1. `this` 란?

어떠한 object를 가리키는 키워드
- javascript의 함수는 호출될 때 this를 암묵적으로 전달받음
  
javascript에서의 this는 일반적인 프로그래밍 언어에서의 this와 조금 다르게 동작
- javascript는 해당 함수 호출 방식에 따라 this에 바인딩되는 객체가 달라짐
- 즉, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 동적으로 결정됨

---

### 2. 전역 문맥에서의 `this`

- 브라우저의 전역객체인 `window`를 가리킴
- 전역객체는 모든 객체의 유일한 최상위 객체를 의미

```javascript
cosole.log(this) // window
```

---

### 3. 함수 문맥에서의 `this` - 단순호출

- 전역 객체를 가리킴
- 브라우저에서 전역은 `window`를 의미함

```javascript
const myFunc = function(){
  cosole.log(this)
}

myFunc() // window
```

---

### 4. 함수 문맥에서의 `this` - Method(객체의 메서드)

메서드로 선언하고 호출한다면, 객체의 메서드이므로 해당 객체가 바인딩

```javascript
const myObj = {
  data: 1,
  myFunc() {
    console.log(this)  // myObj
    console.log(this.data) // 1
  }
}
```

---

### 5. 함수 문맥에서의 `this` - Nested(Function 키워드)

일반함수
- `forEach`의 콜백함수에서의 `this`가 메서드의 객체를 가리키지 못하고 전역객체 `window`를 가리킴
- 단순 호출 방식으로 사용되었기 때문

```javascript
const myObj = {
  data: [1],
  myFunc() {
    console.log(this)  // myObj
    this.data.forEach(function (num){
      console.log(num) // 1
      console.log(this) // window
    })
  }
}
```

화살표 함수
- 이를 해결하기 위해 등장한 함수 표현식이 화살표 함수
- 일반 `function` 키워드와 달리 메서드의 객체를 잘 가리킴
- 화살표 함수에서 `this`는 자신을 감싼 정적 범위
- 자동으로 한 단계 상위의 scope의 context를 바인딩

```javascript
const myObj = {
  data: [1],
  myFunc() {
    console.log(this)  // myObj
    this.data.forEach((number) => {
      console.log(num) // 1
      console.log(this) // myObj
    })
  }
}
```

---

### 6. Lexical Scope

- 화살표 함수는 호출의 위치와 상관없이 상위 스코프를 가리킴(Lexical scope this)
- 함수를 어디서 호출하는지가 아니라 어디에 선언하였는지에 따라 결정
- Static scope 라고도 하며 대부분의 프로그래밍 언어에서 따르는 방식

```javascript
let x = 1 // global

funcion first() {
  let x = 10
  second()
}

fuction second() {
  console.log(x)
}

first() // 1
second() // 1
```