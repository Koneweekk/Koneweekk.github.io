---
title: JS - 연산자
date: 2024-04-17 10:50:00 +09:00
categories: [Programming Language, Javascript]
tags:
  [
    프로그래밍 언어,
    Javascript,
    연산자
  ]
---

## Ⅰ. 할당 연산자

> 오른쪽에 있는 피연산자의 평가 결과를 왼쪽 피연산자에 할당하는 연산자

### 1. 일반 할당

`=`을 사용하여 왼쪽 변수에 오른쪽 값 할당

```js
let c = 0
```

---

### 2. 단축 연산자

다양한 연산에 대한 단축 연산자 지원
- `+=` : 증가 단축 연산자
- `-=` : 감소 단축 연산자
- `*=` : 곱셈 단축 연산자
- `/=` : 나누기 단축 연산자


```js
let c = 0;

c += 10; // 10
c -= 3; ;// 7
c *= 2; // 14
c /= 2; // 7
```

- `a++` : 피연산자 평가 결과 연산 후 1 증가
- `a--` : 피연산자 평가 결과 연산 후 1 감소
- `++a` : 피연산자 평가 결과 연산 전 1 증가
- `--a` : 피연산자 평가 결과 연산 전 1 감소

```js
let c = 0;

console.log(++c)  // 1
console.log(--c)  // 0
console.log(c++)  // 0
console.log(c--)  // 1
console.log(c)  // 0
```

---

### 3. 스프레드 연산자

- 배열이나 객체를 전개하여 각 요소를 개별적인 값으로 분리하는 연산자
- 주로 함수 호출 시 매개변수로 배열이나 객체를 전달할 때 사용
- 얕은 복사를 위해서도 활용 가능

```javascript
const numbers = [1, 2, 3]
const otherNumbers = [...numbers, 4, 5] // [1,2,3,4,5]

const obj = {a:1, b:2}
const otherObj = {c:3, ...obj} // {a:1, b:2, c:3}
```

---
<br>


## Ⅱ. 비교 연산자

> 피연산자들을 비교하고 결과값을 boolean으로 반환

### 1. 일반 비교 연산자

문자열은 유니코드 값을 사용하며 표준 사전순서를 기반으로 비교
- 알파벳 순서상 후순위가 더 큼
- 소문자가 대문자보다 더 큼

```javascript
3 > 2 // true
3 < 2 // false
'A'<'B' // true
'Z'<'a' // true
'가'<'나' // true
```

---

### 2. 동등 연산자(`==`)

- 두 피연산자가 같은 값으로 평가되는지 비교 후 boolean 값을 반환
- 비교할 때 암묵적 타입 변환 통해 타입을 이치시킨 후 같은 값인지 비교
- 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별
- 예상치 못한 결과가 발생할 수 있어 특별한 경우를 제외하고 사용 X
  
```javascript
const a = 1
const b = '1'
a==b // true
a==true // true
```

---

### 3. 일치 연산자(`===`)

- 두 피연산자의 값과 타입이 모두 같은 경우 true 반환
- 같은 객체를 가리키거나, 같은 타입이면서 같은 값인지 비교
- 엄격한 비교가 이뤄지며 암묵적 타입 변환이 발생하지 않음

```javascript
const a = 1
const b = '1'
a==b // false
a==Number(b) // true
```

---
<br>

## Ⅲ. 논리 연산자

### 1. 일반 논리 연산자

세가지 논리 연산자로 구성
- and : `&&`
- or : `||`
- not : `!`
  
단축 평가 지원
- `a && b` : `a`가 `false`이면 `b` 연산 진행 x
- `a || b` : `a`가 `true`이면 `b` 연산 진행 x

---

### 2. 삼항 연산자

> 3개의 피연산자를 사용하여 조건에 따라 값을 반환하는 연산자

- 가장 앞의 조건식이 참인 경우 `:` 앞의 값이 반환
- 그 반대의 경우 `:` 뒤의 값이 반환
- 변수에 할당 가능
  
```javascript
true ? 1:2 // 1
false ? 1:2 // 2
```