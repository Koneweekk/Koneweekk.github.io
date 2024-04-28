---
title: JSP - EL, JSTL
date: 2024-04-25 14:00:00 +09:00
categories: [Back-End, Servelt & JSP]
tags: [Back-End, Servelt, JSP, EL]
---

## Ⅰ. EL(Expression Language)

### 1. EL 이란?

> JSP 페이지에서 사용되는 출력 전용 스크립트 언어 

- JSP 2.0에서 새롭게 추가
- 서버에서 해석되는 스크립트언어
- 기존의 Script tag의 표현식 tag에서 업그레이드된 버전
- tomcat 서버가 내장

---

### 2. 주요 특징

- 내장 객체(`request`,  `response`, `session`, `application`)에 저장된 속성 객체의 property를 출력
- 리터럴 데이터, 다양한 연산결과 출력이 가능
- 자바객체가 할 수 있는 다양한 작업은 하지 못함(오직 출력만 담당)
- JSTL과 연동이 가능

---

### 3. 기본적인 문법

`${표현식}`
- 스크립트 릿(`<%=표현식 %>`)을 대체

```jsp
기존코드 : <%= 100 + 500 %>
EL : ${ 100+500 }
```

---

### 4. 내장 객체로의 접근

별도의 변수로 내장객체에 접근 가능
1. param : `getParameter()`
2. paramValues : `getParameterValues()`
3. requestScope : `request`
4. sessionScope : `session`
5. applicationScope : `application`

```jsp
<%
  Date today = new Date();
  request.setAttribute("day", today);
%>

EL : ${requestScope.day}<br>
```

이러한 스코프를 지정하지 않아도 내장 객체 내 속성에 접근 가능

```jsp
<%
  Date today = new Date();
  request.setAttribute("day", today);
%>

EL : ${day}<br>
```

하지만 그 외의 객체에 직접적인 접근은 불가능하다.
- 내장객체에 포함된 데이터에만 접근 가능

```jsp
<%
  Date today = new Date();
  request.setAttribute("day", today);
%>

<!-- 아래 el문은 아무것도 출력하지 않는다. -->
EL : ${today}<br>
```

---
<br>

## Ⅱ. JSTL 개요

### 1. JSTL이란?

> JavaServer Pages Standard Tag Library

- JSP 개발을 보다 쉽게 하기 위해 제공되는 태그 라이브러리
- JSP 페이지에서 반복, 조건 분기, 데이터 포맷팅 등과 같은 일반적인 작업을 수행하는데 사용
- Java 코드를 줄이고 JSP 페이지의 가독성을 향상시키는 데 도움

---

### 2. 기본 문법

코어 태그
- 태그 안 요소가 없을 경우 : `<c:태그명 속성명1:속성값1 속성명2:속성값2 .../>`
- 태그 안 요소가 있을 경우 : `<c:태그명 속성명1:속성값1 속성명2:속성값2 ...> 요소 </c:태그명>`

포매터 태그
- `<fmt:formatXXX 속성명1:속성값1 속성명2:속성값2 ... />`

함수 태그
- `${fn:함수명(매개변수)}`

---
<br>

## Ⅲ. 코어 태그

### 1. set

> 변수를 설정하거나 값에 대한 계산을 수행

```jsp
<c:set var="job" value="농구선수" scope="request"/>
${job }<br>
```

`var`
- 변수의 이름을 지정합니다이 
- 변수는 나중에 JSP 페이지에서 참조할 수 가능
  
`value`
- 변수에 할당될 값

`scope`
- 변수의 범위를 지정

---

### 2. if

> 조건문을 표현하는 태그

`<c:if test="${조건문}">출력문</c:if>`

```jsp
<!-- 기존 코드 -->
<%
  String id = request.getParameter("ID");
  if (id.equals("hong")) {
%>
  <p>true</p>
<%
  }
%>

<!-- JSTL 활용 -->
<c:if test="${param.ID == 'hong'}">
  <p>true</p>
</c:if>
```

`var` 속성을 통해 조건문의 결과를 변수에 담을 수도 있다.

```jsp
<c:if test="${param.ID == 'hong'}" var="result">
  ...
</c:if>

<p>${result}</p>
```

`empty`를 통해 변수가 비어있는지 확인 가능

```jsp
<c:if test="${!empty userpwd}">
  ...
</c:if>
```

---

### 3. choose

> 여러가지 조건을 처리할 수 있는 태그

```jsp
<c:set var="num" value="21"/>
<h4>${num}</h4>
<c:choose>
  <c:when test="${num%2==0}">
    <p>짝수</p>
  </c:when>
  <c:otherwise>
    <p>홀수</p>
  </c:otherwise>
</c:choose>
```

- `<c:when>` : 조건이 참일 때 실행할 내용을 정의
- `<c:choose>` : 모든 `<c:when>` 조건이 거짓일 때 실행할 내용을 정의

```jsp
<c:set var="num" value="2"/>
<h4>${num}</h4>
<c:choose>
  <c:when test="${num==1}">
    <p>1</p>
  </c:when>
  <c:when test="${num==2}">
    <p>2</p>
  </c:when>
  <c:otherwise>
    <p>그 외의 숫자</p>
  </c:otherwise>
</c:choose>
```

- 다수의 `<c:when>` 태그로 else if 구현 가능

---

### 4. forEach

> 반복문을 처리할 수 있는 태그

```jsp
<ul>
  <c:forEach var="i" begin="2" end="9" step="2">
    <li>3*${i}=${3*i}
  </c:forEach>
</ul>
```

- var: 반복 중 현재 인덱스를 저장할 변수의 이름을 지정
- begin: 반복의 시작 값을 지정합니다
- end: 반복의 끝 값을 지정
- step: 반복 시 인덱스를 증가시키는 단계를 지정

```jsp
<%
  int[] arr = new int[]{10, 20, 30, 40, 50};
  request.setAttribute("arr", arr);
%>

<c:forEach var="value" items="${arr}">
  <p>${value}</p>
</c:forEach>
```

- var: 반복 중 현재 요소를 참조할 변수의 이름을 지정
- items: 반복할 컬렉션을 나타내는 표현식

※ `<% %>`에서 생성한 변수는 `request` 혹은 `session`에 담아 활용하는 것이 좋다.

---

### 5. forTokens

> 자바의 for문과 split의 결합

---

### 6. out

> 있는 그대로 출력해주는 태그
- 출력값 안에 태그가 포함되더라도 그대로 출력해준다.

```jsp
<c:out value="<p>문단 태그입니다</p>" />
```

---

### 7. catch

> 예외 처리를 처리할 수 있는 태그

```jsp
<c:catch  var="msg">
name: <%= request.getParameter("name") %>
<%
  if(request.getParameter("name").equals("hong")){
    out.print("당신의 이름은 : " + request.getParameter("name"));
  }
%>
</c:catch>
<c:if test="${msg != null}">
  <h3>예외발생</h3>
  오류메시지 : ${msg}<br>
</c:if>
```

- `var` : 예외 객체를 저장할 변수명을 지정
- `catch` 구문 안에서 에러가 발생하는 구문은 실행 X
  
---
<br>

## Ⅳ. 포매터 태그

### 1. formatNumber

```jsp
<fmt:formatNumber value="50000000" type="currency" currencySymbol="$" /><br>
<!-- $50,000,000 -->

<fmt:formatNumber value="0.13" type="percent"/><br>
<!-- 13% 변수에 설정 -->

<fmt:formatNumber value="123456789" pattern="###,###,###" var="pdata" />
<!-- 변수에 설정한 값 : 123,456,789 -->
```

- value : 형식화할 숫자가 지정
- type : 형식화 유형을 지정
- pattern : 사용자가 지정한 패턴에 따라 형식화

---

### 2. formatDate

```jsp
변수선언 : <c:set var="now" value="<%= new Date() %>" /><br>
변수값 : ${now}<br>
Basic Date : <fmt:formatDate value="${now}" type="date" /><br>
DateStyle(full) : <fmt:formatDate value="${now}" type="date" dateStyle="full" /><br>
DateStyle(short) : <fmt:formatDate value="${now}" type="date" dateStyle="short" /><br>
시간:<fmt:formatDate value="${now}" type="time"/><br>
날짜 + 시간:<fmt:formatDate value="${now}" type="both"/><br>
혼합:<fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full" /><br>
혼합2:<fmt:formatDate value="${now}" type="both" dateStyle="short" timeStyle="short" /><br>

<!-- 
변수값 : Fri Apr 26 09:25:07 KST 2024
Basic Date : 2024. 4. 26.
DateStyle(full) : 2024년 4월 26일 금요일
DateStyle(short) : 24. 4. 26.
시간:오전 9:25:07
날짜 + 시간:2024. 4. 26. 오전 9:25:07
혼합:2024년 4월 26일 금요일 오전 9시 25분 7초 대한민국 표준시
혼합2:24. 4. 26. 오전 9:25
-->
```

- value : 형식화할 날짜가 지정
- type : 형식화 유형을 지정
- dateStyle : 날짜를 표시할 스타일 지정
- timeStyle : 시간을 표시할 스타일 지정

---
<br>

## Ⅴ. 함수 태그

### 1. 함수 태그란?

- 함수와 관련된 여러 가지 유용한 작업을 수행하는 데 사용
- 이것은 주로 문자열 조작 및 변환, 컬렉션 조작 등에 사용

---

### 2. 사용법

el 태그 안에서 사용
- 다양한 함수가 존재

```jsp
대문자 : ${fn:toUpperCase(str)}<br>
문자열길이 : ${fn:length(str)}<br>
치환 : ${fn:replace(str,'a','AAAA')}<br>

<!-- 
대문자 : ORACLE
문자열길이 : 6
치환 : orAAAAcle
-->
```