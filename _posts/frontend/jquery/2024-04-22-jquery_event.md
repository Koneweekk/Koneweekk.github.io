---
title: Jquery - 이벤트
date: 2024-04-22 14:30:00 +09:00
categories: [Front-End, Jquery]
tags: [Javascript, Jquery]
---

## Ⅰ. 이벤트 객체에 접근하는 방법

### 1. this

javascript
- 현재 실행 중인 함수에서 "현재" 객체를 참조하는 데 사용

jQuery
- 특히 선택한 요소를 가리키는데 사용
- 일반적으로 jQuery에서 이벤트 핸들러 함수 내에서 this를 사용하면 현재 이벤트를 트리거한 요소를 가리킴

```js
$('input').focus(function(){
  $(this).css('background-color', 'gray');
})
```

---

### 2. 함수의 매개변수

이벤트 객체를 매개변수로도 전달하여 사용 가능

```js
$("input").blur(function (event) {
  $(event.target).css("background-color", "white");
});
```

---
<br>

## Ⅱ. 이벤트 추가 방법

### 1. 일반 방법

`$(selector).eventType(function(event){})`

- `$(selector)` : 이벤트를 추가하고자 하는 요소를 선택
- `eventType` : 감지하고자 하는 동작
- `function(event){}` : 동작 감지시 실행하고자 하는 함수

---

### 2. on 활용

`on` 함수를 활용하여 이벤트를 추가할 수도 있다.

```js
$('selector').on('eventType', function(){
  alert("이벤트 추가")
})
```

### 3. 동적으로 이벤트 구현

이벤트가 실행될 때 문서의 모든 부분을 탐색한 후 이벤트 추가

```js
$(document).on('change','selector', function(){
  alert("동적인 이벤트 추가")
})
```

---
<br>

## Ⅲ. 자주 쓰는 이벤트

### 1. click

웹 페이지에서 요소를 클릭했을 때 발생하는 이벤트를 처리

```js
$('#btncopy').click(function(){
  $('#txtcopyuserid').val($('#txtuserid').val());
});
```

---

### 2. change

- 입력 요소의 값이 변경되었을 때 발생하는 이벤트를 처리
- 주로 `<input>`, `<select>`, `<textarea>`와 같은 폼 요소에 적용

```js
$('#select_hobby').change(function(){
  $('#txtselect').val($('#select_hobby option:selected').val());
});
```

--- 

### 3. keyup

- 키보드의 키를 눌렀다가 놓을 때 발생하는 이벤트를 처리
- 주로 `<input>` 요소나 `<textarea>`와 같은 텍스트 입력 요소에서 사용
- 사용자가 텍스트를 입력할 때마다 발생하는 변화를 감지

```js
$('#txtpwd2').keyup(function(){
  if($('#txtpwd').val() != $('#txtpwd2').val()) {
    $('#message').text('암호가 일치하지않아요');
  } else {
    $('#message').text('암호가 일치합니다.');
  }
})
```

---

### 4. focus

- 입력 요소를 선택하거나 포커스를 주었을 때 발생

```js
$("input").focus(function () {
  $(this).css("background-color", "gray");
});
```

---

### 5. blur

- 입력 요소에서 포커스가 해제되었을 때 발생

```js
$("input").blur(function() {
  $(this).css("background-color", "white");
});
```

---

### 6. hover

- 마우스를 요소 위로 올렸을 때와 마우스를 요소 밖으로 이동했을 때 발생
-  mouseenter 이벤트 : 마우스가 요소 위로 올라갔을 때 발생
-  mouseleave 이벤트 : 마우스가 요소 밖으로 나갔을 때 발생

```js
$("p").hover(
  function () {  // mouseenter
    $(this).css("background-color", "gold");
  },
  function () {  // mouseleave
    $(this).css("background-color", "red");
  }
);
```

---
<br>

## ※ each

### 1. each의 역할

- 주어진 요소의 집합에 대해 반복 작업을 수행하는 데 사용
- 일반적인 반복문인 for나 forEach()와 유사한 역할

```js
$('li').each(function() {
  console.log(index + ": " + $(this).text());
});
```

---

### 2. each의 매개 변수

콜백함수의 매개변수를 통해 해당 요소 배열의 인덱스와 이벤트 객체에 접근 가능

`$(selector).each(function(index, element){})`

```js
$('p').each(function(index, element) {
  console.log(element);
  console.log(index);
});
```

매개변수가 없다면 `this`를 통해 접근

`$(selector).each(function(){ this 활용 })`

```js
$('p').each(function(){
  console.log($(this).text())
})
```

---

### 3. 배열의 each

배열의 각 요소에 대해 콜백 함수를 호출
- 첫 번째 매개변수 : 현재 요소의 인덱스
- 두 번째 매개변수 : 해당 요소의 값


`$.each(array, function(index,element){});`

---

### 4. 객체의 each

객체의 각 속성에 대해 콜백 함수를 호출
- 첫 번째 매개변수 : 현재 속성의 키
- 두 번째 매개변 : 해당 속성의 값

`$.each(obj, function(key, value){});`

---
<br>