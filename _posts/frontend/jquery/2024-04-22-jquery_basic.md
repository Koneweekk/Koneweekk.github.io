---
title: Jquery - 시작하기
date: 2024-04-22 11:00:00 +09:00
categories: [Front-End, Jquery]
tags: [Javascript, Jquery]
---

## Ⅰ. Jquery란?

### 1. Jquery란?

> JavaScript 라이브러리로 웹 개발을 보다 쉽게 만들어주는 도구

JavaScript로 수행할 수 있는 많은 작업들을 간단한 명령어로 처리 가능

- 이를 통해 DOM 조작, 이벤트 핸들링, 애니메이션 효과 적용, AJAX 요청 등을 보다 쉽게 처리
- jQuery는 크로스 브라우징을 지원하여, 다양한 웹 브라우저에서 일관된 동작을 보장

---

### 2. Jquery 시작

Jquery를 사용하기 위해선 js 소스 파일을 가져와야 한다.

파일을 다운로드하여 로컬에서 로드

```html
<head>
  ...
  <script src="jquery-3.7.1.min.js"></script>
</head>
```
cdn 방식(권장되는 방식)

```html
<head>
  ...
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
</head>

```

현재 버전에선 삭제된 기능이 필요한 경우 migrate 버전을 사용하여야 한다.

```html
<script src="https://code.jquery.com/jquery-migrate-3.4.1.js"></script>
```

---
<br>

## Ⅱ. Jquery 객체와 함수

### 1. Jquery 객체

jQuery를 사용하기 위해서는 먼저 jQuery 객체를 생성해야함
- 생성된 jQuery 객체는 다양한 메소드를 가짐
- jQuery를 학습한다고 하는 것은 대체로 이 메소드를 사용하는 방법을 익히는 것
- jQuery 객체를 생성하기 위해서는 jQuery 함수를 사용한다.

---

### 2. Jquery 함수

`jQuery()`
- `$()` : `jQuery()` 함수의 축약형(Shorthand)

jQuery() 함수는 전달되는 인수의 종류에 따라 다른 움직임을 하지만 결국 jQuery 객체를 반환한다.


---
<br>

## Ⅲ. Jquery 기본 문법

### 1. $()

`$()`
- 달러($) 기호는 제이쿼리를 의미하고, 제이쿼리에 접근할 수 있게 해주는 식별자
- 선택된 HTML 요소를 제이쿼리에서 이용할 수 있는 형태로 생성해 주는 역할
- 인수로는 HTML 태그 이름뿐만 아니라, CSS 선택자를 전달하여 특정 HTML 요소를 선택 가능
- 이러한 `$()` 함수를 통해 생성된 요소를 제이쿼리 객체(jQuery object)라고 한다.
- 제이쿼리는 이렇게 생성된 제이쿼리 객체의 메소드를 사용하여 여러 동작을 설정할 수 있습니다.

---

### 2. ready

- 문서가 완전히 로드되었을 때 실행될 콜백 함수를 등록하는 데 사용
- HTML 문서의 모든 요소가 브라우저에 의해 완전히 로드되고 구문 분석되었을 때 코드 실행을 보장

```js
// 실행할 코드
$(document).ready(function(){
});
```

- 아래와 같이 축약 표현도 가능

```js
$(function(){
    // 실행할 코드
});
```

--- 

### 3. 셀렉터

`$(선택자)`

- CSS 셀렉터와 매우 유사하며, jQuery를 사용하여 DOM 요소를 찾고 조작하는 데 사용
- 선택자를 이용하여 원하는 HTML 요소를 선택하고, 동작 함수를 정의하여 선택된 요소에 원하는 동작을 설정
- 만약 선택되는 요소가 여러 개라면 배열이 반환됨

---

### 4. 이벤트

```js
$("p").click(function(){
  $(this).hide();
});
```

- 이벤트를 처리하려면 다양한 내장 이벤트 메서드를 사용 가능
- 이러한 메서드를 사용하면 특정 이벤트가 발생했을 때 함수를 실행