---
title: CSS - Display와 Position
date: 2024-04-15 15:10:00 +09:00
categories: [Front-End, CSS]
tags:
  [
    Web,
    HTML,
    Browser,
    CSS,
    Display,
    Position
  ]
---

# Ⅰ. CSS Display

### 1. Block

줄 바꿈이 일어나는 요소(다른 요소를 밀어냄)  

화면 크기 전체의 가로 폭을 차지  

블록 레벨 요소 안에 인라인 레벨 요소가 들어갈 수 있음.  

block의 기본 너비는 가질 수 있는 너비의 100%  

너비를 가질 수 없다면 margin이 자동으로 부여됨  

`div` / `ul`, `ol`, `li` / `p` / `hr` / `form`

---

### 2. Inline

줄바꿈이 일어나지 않는 행의 일부 요소  

content를 마크업 하고 있는 만큼만 가로폭을 차지  

`width`, `height`, `margin-top`, `margin-bottom`을 지정할 수 없음  

상하 여백은 `line-height`로 지정
inline의 기본 너비는 컨텐츠 영역 만큼  

내가(inline) 정렬 하는 게 아닌 부모가(block) 정렬해 주는 것  

`span` / `a` / `input`, `label` / `b`, `em`, `i`, `strong`

---

### 3. Inline-block

block과 inline 레벨 요쇼의 특징을 모두 가짐  

inline처럼 한 줄에 표시 가능  

block처럼 width, height, margin 속성을 모두 지정할 수 있음  

---

### 4. None


해당 요소를 화면에 표시하지 않고, 공간조차 부여되지 않음

`visibilty: hidden` : 공간은 차지하나 화면에 표시 X

---
<br>

## Ⅱ. CSS Position

문서 상에서 요소의 위치를 지정

---

### 1. static

모든 태그의 기본 값(기준 위치)

일반적인 요소의 배치 순서에 따름(좌측 상단)

부모 요소 내에서 배치될 때는 부모 요소의 위치를 기준을 배치됨

---

### 2. relative

상대 위치

자기 자신의 static 위치를 기준으로 이동(normal flow 유지)

레이아웃에서 요소가 차지하는 공간은 static 일 때와 같음

normal poistion 대비 offset

---

### 3. absolute

절대 위치

요소를 일반적인 문서 흐름에서 제거 후 레이아웃에 공간을 차지하지 않음

static이 아닌 가장 가까이 있는 부모/조상 요소를 기준으로 이동

---

### 4. fixed

고정 위치

요소를 일반적인 문서 흐름에서 제거 후 레이아웃에 공간을 차지하지 않음

부모 요소와 관계없이 viewport를 기준으로 이동

스크롤 시에도 항상 같은 곳에 위치함

---

### 5. sticky

스크롤에 따라 static > fixed로 변경

속성을 적용한 박스는 평소에 문서 안에서 static 상태

스크롤 위치가 임계점에 이르면 fixed