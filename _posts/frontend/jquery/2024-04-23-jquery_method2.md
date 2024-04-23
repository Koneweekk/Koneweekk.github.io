---
title: Jquery - 메서드2
date: 2024-04-23 11:00:00 +09:00
categories: [Front-End, Jquery]
tags: [Javascript, Jquery]
---

## Ⅰ. 요소 생성

### 1. 새로운 요소 생성

`$` 키워드를 사용하여 간단하게 새로운 요소를 생성할 수 있다.

```js
const newDiv = $("<div>new element</div>");
```

---

### 2. 요소 속성 지정

`$`의 두번째 매개변수로 새로운 요소의 속성을 지정할 수 있다.

```js
let pTag = $("<p></p>", {
  class: "my",
  text: "동적 추가",
  click: function () {
    $(this).css("background", "lime");
  }
});
```

---
<br>

## Ⅱ. 자식 요소 접근

### 1. children

> 선택한 요소의 모든 자식 요소를 선택

```js
$("#parent")
  .children()
  .each(function () {
    console.log($(this).text());
  });
```

특정 자식 요소들만 추려낼 수도 있다

```js
const tdList = $(this).children("td");
```

---

### 2. eq

> `children()`으로 선택된 자식 요소들 중에서 특정 인덱스에 해당하는 요소를 선택

- $(selector).children().eq(index)

```js
$("#parent").children().eq(1).css("font-weight", "bold");
```

---
<br>

## Ⅲ. 부모 요소 접근

### 1. parent

> 선택한 요소의 직계 부모 요소를 선택

```js
$('.child').parent().css('background-color', 'lightblue');
```

---

### 2. parents

> 선택한 요소의 모든 조상 요소를 선택

```html
<div class="grandparent">
    <div class="parent">
        <div class="child">Child</div>
    </div>
</div>
```

```js
$('.child').parents().css('border', '2px solid red');
```

- `parents()`는 `.parent`와 `.grandparent`를 가진 `div` 태그를 모두 반환한다.