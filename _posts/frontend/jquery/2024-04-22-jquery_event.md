---
title: Jquery - 이벤트와 메서드
date: 2024-04-22 14:30:00 +09:00
categories: [Front-End, Jquery]
tags: [프로그래밍 언어, Javascript, Jquery]
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

## Ⅱ. 자주 쓰는 이벤트

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

## Ⅲ. On / Off

### 1. On

- 지정된 이벤트가 발생했을 때 실행할 함수를 바인딩
- on() 메서드를 사용하면 이벤트 핸들러를 여러 개 등록 가능

```js
$('p').on (
  {
    mouseleave:function(){ $(this).css('background-color','red') },
    click:function(){$(this).css('background-color','green')},
    mouseenter:function(){$(this).css('background-color','blue')}
  }	
);
```

---

### 2. Off

- 이벤트 핸들러를 제거

```js
$('button').click(function(){
    $('p').off('click'); //클릭이벤트 제거 
});
```

---
<br>

## Ⅳ. Text / Html

### 1. Text

> 순수한 텍스트를 가져오고 설정

- 선택한 요소의 텍스트 내용을 가져오거나 설정
- HTML 요소의 텍스트 내용만을 고려
- HTML 태그를 제외한 순수한 텍스트 내용만을 다룸
- 텍스트 그대로를 다룹니다. 
- 텍스트 내용을 설정할 때는 HTML 태그를 인식하지 않고 그대로 출력

```js
//getter
$('#btn1').click(function(){
  console.log("test",$('#test').text()); 
});

//setter 
$('#btn3').click(function(){
  $('#test').text("AAAAA"); 
});
```

---

### 2. html

> HTML을 가져오고 설정

- HTML 요소 자체와 그 안에 포함된 다른 요소들을 모두 포함
- HTML 내용을 설정할 때는 기존의 내용을 완전히 대체

```js
// getter
$('#btn2').click(function(){
  console.log("test",$('#test').html());
});

// setter
$('#btn3').click(function(){
  $('#test').html("<div>AAAAA</div>");
});
```

---
<br>

## Ⅴ. each

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


