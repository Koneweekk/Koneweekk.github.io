---
title: JSP - application 객체
date: 2024-04-24 10:40:00 +09:00
categories: [Back-End, Servelt & JSP]
tags: [Back-End, Servelt, JSP, application]
---

## Ⅰ. web.xml

### 1. web.xml이란?

Java EE(Web) 애플리케이션에 대한 설정과 구성을 정의
- 애플리케이션의 구조 및 동작 방식을 알려줌
- 서버가 시작되면 제일 먼저 로딩되고 해석되고 실행됨
- 서비스 자원 : html, htm , css , js , json , txt , jsp 

---

### 2. 기본 구조

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
	id="WebApp_ID" version="4.0">
	<display-name>Web</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>index.htm</welcome-file>
	</welcome-file-list>
	<!-- 공통변수 -->
	<context-param>
		<description>이메일</description>
		<param-name>email</param-name>
		<param-value>example@naver.com</param-value>
	</context-param>
	<context-param>
		<description>파일패스</description>
		<param-name>filePath</param-name>
		<param-value>C:\Users\Example\eclipse\java-2023-12\eclipse</param-value>
	</context-param>
</web-app>
```

`<display-name>Web</display-name>`
- 웹 프로젝트 이름
- 가상 디렉토리
- context root (문맥 , 흐름 , 전체)

`<welcome-file>index.htm</welcome-file>` 
- 웹 사이트 기본 페이지 설정

`<context-param>...</context-param>`
- 프로젝트 내 모든 jsp 파일에서 사용할 수 있는 공통변수

---

### 3. 보안 관련 이슈

- web.xml에는 DB 접근 코드등 보안 상으로 예민한 정보도 포함된다.
- url을 통한 web.xml로의 접근을 막기 위해 WEB-INF란 폴더 안에 위치해야한다.
- jsp는 application 객체를 통해 이 파일의 정보에 접근 가능

---
<br>

## Ⅱ. application 객체

### 1. application 객체란?

웹 애플리케이션의 범위(Scope) 중 하나
- 해당 애플리케이션 내에서 데이터를 공유하고 유지
- 이는 여러 사용자가 애플리케이션을 사용하는 동안 유지
- 모든 사용자에게 공유되는 데이터를 저장하는 데 사용
- web.xml 파일에 접근하기 위해서 사용된다.

---

### 2. getInitParameter

> 지정된 이름의 초기화 매개변수 값을 반환하는 메서드

- web.xml의 `context-param`태그에 정의한 변수에 접근 가능

```jsp
<%
  String email = application.getInitParameter("email");
  out.print("<h3>" + email + "</h3>");
%>
```

### 3. getRealPath

> 지정된 경로의 실제 파일 시스템 경로를 반환합니다.

```java
String realPath = application.getRealPath(request.getContextPath());

System.out.println(realPath);
// C:\hyosungedu\WEB\BackEnd\weblabs\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps\Web\Web
```
