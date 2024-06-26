---
title: CSS - 레이아웃
date: 2024-04-15 16:00:00 +09:00
categories: [Front-End, CSS]
tags: [Web, HTML, Browser, CSS, Layout, Float]
---

## Ⅰ. Float

### 1. float란?

박스를 왼쪽 혹은 오른쪽으로 이동시켜 인라인 요소들이 주변을 wrapping 하도록 함

요소가 Normal flow를 벗어나도록 함

---

### 2. float 속성

`none` : 기본값

`left` : 요소를 왼쪽으로 띄움

`right` : 요소를 오른쪽으로 띄움

※ `clear: "both"`: 이 요소부턴 다시 Normal flow를 따름

---

### 3. float 정리

최근 Flexbox, Grid 등장과 함께 사용도가 낮아짐

Normal Flow에서 벗어난 레이아웃 구성

---

<br>

## <b>Ⅱ. Flexbox</b>

### 1.CSS Flexible Box Layout

행과 열 형태로 아이템들을 배치하는 1차원 레이아웃 모델

축

- main axis(메인 축)
- cross axis(교차 축)

구성 요소

- Flex Container(부모 요소)
- Flex item(자식 요소)

---

### 2. Flexbox 구성 요소

Flex Container(부모 요소)

- flexbox 레이아웃을 형성하는 가장 기본적인 모델
- Flex Item들이 놓여있는 영역
- display 속성을 flex 혹은 inline-flex로 지정

Flex Item(자식 요소)

- 컨테이너에 속해 있는 컨텐츠(박스)

---

### 3. 배치 설정

`flex-direction`

- Main axis 기준 방향 설정
- 역방향의 경우 HTML 태그 선언 순서와 시작적으로 다르니 유의
- `row`, `row-reverse`, `column`, `column-reverse`

`flex-wrap`

- 아이템이 컨테이너를 벗어나는 경우 해당 영역 내에 배치되도록 설정
- 즉, 줄넘김
- `wrap` : 공간이 부족하면 아래쪽으로 넘김
- `nowrap` : 공간이 부족하면 요소들의 width값을 줄임

`flex-flow`

- `flex-direction`과 `flex-wrap`의 shorthand
- 두 속성 값을 차례로 작성

---

### 4. 공간 나누기

`justify-content`

- Main axis를 기준으로 공간 배분
- `flex-start` : 시작부분 집결
- `flex-end` : 끝부분 집결
- `flex-center` : 중앙 집결
- `space-between` : 요소 사이에 공간 균등 배분
- `space-around` : 요소를 둘러싼 공간 균등 배분
- `space-evenly` : 왼쪽 오른쪽에도 공간 균등 배분

`align-content`

- Cross axis를 기준으로 공간 배분
- 아이템이 한 줄로 배치되는 경우 확인 불가능
- `justify-content` 와 설정값 같음

---

### 5. 정렬

`align-items`

- 모든 아이템을 Cress axis를 기준으로 정렬
- `stretch` : 기본 값
- `flex-start` : 시작 부분에 정렬
- `flex-end` : 끝 부분에 정렬
- `center` : 중앙 정렬
- `baseline` : 텍스트 baseline 에 맞춤

`align-self`

- 개별 아이템을 Cross axis 기준으로 정렬
- `align-items` 와 설정값 같음

---

### 6. 기타 속성

`flex-grow`

- 남은 연역을 아이템에 분배

`order`

- 배치 순서 설정

---

<br>

## Ⅲ. Grid

### 1. grid란?

> 행과 열을 사용하여 그리드 기반 레이아웃 시스템을 제공

- `float` 및 `position`을 사용하지 않고도 웹 페이지를 디자인하기를 더 쉽게 만든다.
- flex와 거의 비슷한 것처럼 들립니다만, 각 행을 개별적으로 정의 가능

---

### 2. grid 주요 개념

Grid Container (그리드 컨테이너)

- 그리드 컨테이너는 `display: grid;`가 적용된 요소
- 이 컨테이너는 그리드의 부모 요소로서, 그 안에 그리드 아이템들을 포함

Grid Items (그리드 아이템)

- 그리드 컨테이너 안에 있는 자식 요소
- 이들은 그리드의 셀에 배치

Grid Lines (그리드 라인)

- 그리드의 수평 및 수직 선
- 행과 열 사이를 나타냄

Grid Tracks (그리드 트랙)

- 그리드 라인 사이의 공간
- 행 또는 열을 나타냄

Grid Cells (그리드 셀)

- 그리드의 한 칸을 나타냄
- 행과 열이 만나는 지점

Grid Areas (그리드 영역)

- 그리드 셀들의 집합
- 그리드 아이템을 배치하는 데 사용

---

### 3. grid 사용법

1. `display: grid;`를 사용하면 그리드 컨테이너를 생성
2. `grid-template-columns`, `grid-template-rows` 등의 속성을 사용하여 그리드의 열과 행의 크기 및 구조를 정의
3. `grid-column`, `grid-row` 등의 속성을 사용하여 그리드 아이템을 특정 셀에 배치

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .grid-container {
        display: grid;
        grid-template-columns: 33% 33% 33%;
      }
    </style>
  </head>
  <body>
    <h1>Grid Example</h1>

    <p>Align three DIV elements side by side.</p>

    <div class="grid-container">
      <div style="background-color:#FFF4A3;">
        <h2>London</h2>
        <p>London is the capital city of England.</p>
        <p>London has over 13 million inhabitants.</p>
      </div>

      <div style="background-color:#FFC0C7;">
        <h2>Oslo</h2>
        <p>Oslo is the capital city of Norway.</p>
        <p>Oslo has over 600.000 inhabitants.</p>
      </div>

      <div style="background-color:#D9EEE1;">
        <h2>Rome</h2>
        <p>Rome is the capital city of Italy.</p>
        <p>Rome has almost 3 million inhabitants.</p>
      </div>
    </div>
  </body>
</html>
```
