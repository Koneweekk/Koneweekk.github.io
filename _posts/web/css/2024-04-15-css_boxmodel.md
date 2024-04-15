---
title: CSS - 박스 모델
date: 2024-04-15 15:30:00 +09:00
categories: [Web, CSS]
tags:
  [
    Web,
    HTML,
    Browser,
    CSS,
    Boxmodel
  ]
---

## Ⅰ. Box Model 이란?

### 1. css 원칙

모든 요소는 네모(박스모델)이다.  

위에서 아래로, 왼쪽에서 오른쪽으로 쌓임  

Normal Flow:
- 인라인 요소는 좌우로 쌓임
- 블록 요소는 상하로 쌓임

---

### 2. box model의 구성

![box model](https://velog.velcdn.com/images/hoonnn/post/a7c296c7-c12b-4148-93ce-28204b117bb2/image.png)

모든 HTML 요소는 box 형태로 되어있음  

하나의 박스는 네 부분으로 이루어짐  
- margin : 테두리 바깥의 외부 여백(배경색 지정 불가)
- padding : 테두리 안쪽의 내부 여백(배경색과 이미지는 padding까지 적용)
- border : 테두리 영역
- content : 요소의 실제 내용

---

### 3. box sizing

기본적으로 모든 요소의 box-sizing은 content-box
- padding을 제외한 순수 contents 영역만을 box로 지정

다만, 우리가 일반적으로 영역을 볼 때는 border까지의 너비를 원함
- box-sizing을 border-box로 설정

---
<br>

## Ⅱ. BoxModel의 구성 요소

### 1. margin

> 테두리 바깥의 외부 여백  
> 배경색 지정 불가

```css
.margin{
  margin-top : 10px;
  margin-right : 20px;
  margin-bottom : 30px;
  margin-left : 40px;
}
```

---

### 2. padding
 
> 테두리 안쪽의 내부 여백  
> 배경색과 이미지는 padding까지 적용

```css
.margin-padding{
  margin : 10px;
  padding: 30px;
}
```

---

### 3. border

> 테두리 영역

```css
.border{
  border-width: 2px;
  border-style: dashed;
  border-color: black;
}
```

---

### 4. 축약 표현

전부
```css
.margin-1{
  margin: 10px;
}
```

상하(10px), 좌우(20px)
```css
.margin-2{
  margin: 10px 20px;
}
```

상(10px), 하(30px), 좌우(20px)
```css
.margin-3{
  margin: 10px 20px 30px;
}
```

상(10px), 하(30px), 우(20px), 좌(40px)
```css
.margin-4{
  margin: 10px 20px 30px 40px;
}
```