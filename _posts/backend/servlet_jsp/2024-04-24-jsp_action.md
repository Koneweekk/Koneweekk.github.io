---
title: JSP - action
date: 2024-04-24 11:40:00 +09:00
categories: [Back-End, Servelt & JSP]
tags: [Back-End, Servelt, JSP, include]
---

## Ⅰ. action 태그

### 1. action 태그란?

- JSP 페이지에서 특정 동작을 수행하도록 지시하는데 사용
- 이러한 액션 태그는 서버 측 코드를 생성하고 실행하여 동적인 콘텐츠를 생성하는 데 도움

---

### 2. 기본적인 문법

`<jsp:actionType property="value">`
- `actionType` : action 타입을 지정합니다.
- `property` : action에서 지정하고자 하는 속성
- `"value"` : 지정하고자 하는 속성의 값

```jsp
<jsp:include page="/common/top.jsp"/>
```

---
<br>

## Ⅱ. include

### 1. include 용도

> 다른 페이지를 현재 페이지에 포함시킴

모듈화
- 공통된 부분을 별도의 파일로 분리하여 관리
- 예를 들어헤더, 푸터, 사이드바 등을 별도의 파일로 분리하여 관리 가능

코드의 재사용성
- 여러 페이지에서 내용의 경우 해당 내용을 하나의 파일로 작성하고 필요한 곳에서 포함하여 재사용
- 중복 코드를 방지하고 개발 시간을 단축시키는 데 도움
  
동적 콘텐츠
- 포함되는 페이지가 동적으로 생성되는 경우, 해당 페이지가 포함되는 페이지도 동적으로 생성
- 이를 통해 동적으로 생성된 콘텐츠를 페이지에 쉽게 포함시킬 수 있음

유지 보수성 향상
- 코드 변경이 필요한 경우, 해당 파일만 수정
- 코드의 일관성을 유지하고 오류를 최소화하는 데 도움

---

### 2. 기본적인 사용법

`<jsp:include page="filePath"/>`
- `include` : action 타입으로 `include`를 입력
- `page` : 포함시키고자 하는 하위 페이지 경로를 입력

```jsp
<table style="width:700">
  <tr>
    <td colspan="2">
      <jsp:include page="/common/top.jsp"/>
    </td>
  </tr>
  <tr>
    <td style="width:200px">
      <jsp:include page="/common/left.jsp"/>
    </td>
    <td style="width:500px">
      <jsp:include page="/common/bottom.jsp"/>
    </td>
  </tr>
</table>
```

여러 페이지에서 자주 내용은 include를 활용하여 활용하는 것이 좋음
- 네비게이션바, 사이드바, 푸터, 헤더 등의 요소가 이에 포함됨

