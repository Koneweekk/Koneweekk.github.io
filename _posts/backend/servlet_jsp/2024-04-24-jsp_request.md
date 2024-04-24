---
title: JSP - request 객체
date: 2024-04-24 09:30:00 +09:00
categories: [Back-End, Servelt & JSP]
tags: [Back-End, Servelt, JSP, request]
---

## Ⅰ. request 객체 개요

### 1. request 객체란?

> HTTP 요청에 대한 정보를 담고 있는 객체

- 클라이언트가 요청 보내면 요청한 건(페이지당)에 대해서 서버는 request 객체 자동 생성
- 이 객체를 사용하여 클라이언트가 서버에 보낸 HTTP 요청에 관련된 다양한 정보를 읽고, 처리할 수 있음
- `request` 객체는 JSP 페이지 내에서 기본적으로 사용할 수 있으며, HTTP 요청의 여러 측면을 다루는 데 유용

---

### 2. Get과 Post

GET 
- url에 데이터를 담음
- `jsp?userid=kglim&pwd=1004&hobby=a&hobby=b&hobby=c`
- jsp에선 get 방식인 경우 한글이 깨지는 경우가 발생
- `request.setCharacterEncoding("UTF-8")` 사용해야함
  
POST
- 내부적(body)으로 데이터를 담아서 보냄
- url에 데이터를 담지 않아 보안성이 강화됨
- 별도의 한글 깨지 방지 처리가 되어있음

---
<br>

## Ⅱ. 관련 메서드

### 1. getParameter

> URL에 포함된 쿼리 문자열이나 HTTP 요청 본문에 포함된 매개변수를 읽을 수 있음

- `getParameterValues` : 매개변수를 배열로 반환받음

```jsp
<%
request.setCharacterEncoding("UTF-8"); //한글처리

String uid = request.getParameter("userid");
String pwd = request.getParameter("pwd");

// 배열 리턴
String[] hobbyarray =  request.getParameterValues("hobby");
%>    
```

---

### 2. 클라이언트 정보

`getRemoteAddr`
- 현재 요청을 보낸 클라이언트의 IP 주소를 반환

```jsp
<p>	접속한 클라이언트 IP : <%= request.getRemoteAddr() %></p>
```

---

### 3. 서버 정보 반환

`getServerPort`
- 현재 요청이 도착한 서버 포트 번호를 반환
  
```java
String port = request.getContextPath();

System.out.println(sp);
// 8090
```

`getServletPath`
- 패키지 + 파일명를 반환

```java
String sp = request.getServletPath();

System.out.println(sp);
// /WebJSP/Member/Ex11_WebApp_RealPath.jsp
```

`getRequestURI`
- 프로젝트 이하 경로

```java
String uri = request.getRequestURI();

System.out.println(uri);
// /WebJSP/Ex11_WebApp_RealPath.jsp
```


`getRequestURL`
- 전체주소 (프로토콜 + IP + 포트 + 상세경로)
  
```java
StringBuffer url = request.getRequestURL();

System.out.println("URL : " + url);
// http://192.168.0.12:8090/WebJSP/Ex11_WebApp_RealPath.jsp
```

---
<br>

## ※ URI, URL, URN

### URI (Universal Resource Identifier)
- 인터넷상의 자원을 식별하기 위한 표기법 및 규약
- URL + URN = URI
- 인터넷 환경에서 URI는 대부분 URL을 의미, URN은 사용이 제한적

### ​URL (Uniform Resource Locator)
- 물리적인 경로, 즉 자원의 위치 정보를 포함
- 프로토콜, 도메인, 아이피, 포트 등
- 컴퓨터로부터 접근 가능한 형태의 자원만 검색될 수 있어 제한적
- 우리가 흔히 보는 'http://test.com/test.jpg' 형태

### ​​URN (Uniform Resource Name)
- 독립적인 이름을 제공하기 위해 존재
- 자원에 대해 영속적이고 유일
- 자원의 위치와는 무관
- 예를들어 우리나라에서는 주민등록번호가 대표적


