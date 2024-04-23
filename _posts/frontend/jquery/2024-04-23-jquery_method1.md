---
title: Jquery - 메서드1
date: 2024-04-23 09:30:00 +09:00
categories: [Front-End, Jquery]
tags: [Javascript, Jquery]
---

## Ⅰ. 이벤트 추가, 제거

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

## Ⅱ. 텍스트 접근

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

### 3. val

> 선택한 요소의 값(value)을 가져오거나 설정

- 주로 `<input>`, `<select>`, `<textarea>` 등의 폼 요소에서 사용

```js
// getter
var value = $('#myInputField').val();

// setter
$('#myInputField').val('New Value'); 
```

---
<br>

## Ⅲ. Add

### 1. append

선택한 요소의 끝에 새로운 내용을 추가
- `$(selector).append(content)`


```js
$('#btn1').click(function(){
  $('p').append("<b>append text</b>");
});
```

---

### 2. prepend

선택한 요소의 시작 부분에 새로운 내용을 추가
- `$(selector).prepend(content)`

```js
$('#btn2').click(function(){
  $('p').prepend("<b>append text</b>");
});
```

---

### 3. appendTo

선택한 요소의 끝 부분에 새로운 내용을 추가
- 문법이 `append`와 역순
- `$(content).appendTo(selector)`

```js
$('#btnmenu').click(function() {
  $('#menu').clone().appendTo('#submenu');
})
```

---

### 4. prependTo

선택한 요소의 시작 부분에 새로운 내용을 추가
- 문법이 `prependTo`와 역순
- `$(content).prependTo(selector)`

```js
$('#btnmenu').click(function() {
  $('#menu').clone().prependTo('#submenu');
})
```

---

### 5. after

선택한 요소 뒤에 새로운 내용을 추가
- `$(selector).after(content)`

---

### 6. before

선택한 요소 앞에 새로운 내용을 추가
- `$(selector).before(content)`


  
---
<br>

## Ⅳ. DOM 제거

### 1. remove

> 선택한 요소를 제거

- 선택한 요소와 해당 하위 요소들을 모두 제거

```js
$('#removeButton').click(function() {
  $('#container').remove();
});
```

---

### 2. empty

> 선택한 요소의 자식 요소들을 제거

- 선택한 요소의 내부를 비웁니다.

```js
$('#emptyButton').click(function() {
  $('#container').empty();
});
```