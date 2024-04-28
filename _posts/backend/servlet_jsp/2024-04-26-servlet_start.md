---
title: Servlet - 기본 문법
date: 2024-04-26 09:10:00 +09:00
categories: [Back-End, Servelt & JSP]
tags: [Back-End, Servelt, JSP]
---

## 1. Servlet 클래스 작성

- Servlet 클래스는 javax.servlet.Servlet 인터페이스를 구현하거나, 구현한 클래스를 상속받음
- 일반적으로는 javax.servlet.http.HttpServlet 클래스를 상속받아 구현

```java
import javax.servlet.http.HttpServlet;

//@WebServlet("/simple")
public class SimpleController extends HttpServlet {
	private static final long serialVersionUID = 1L;

    public SimpleController() {
        super();
    }

    ..

}
```

---

## 2. HTTP 요청 처리 메서드 구현

- HTTP 요청 방식(GET, POST, PUT, DELETE 등)에 따라 함수 호출
- `doGet()`, `doPost()`, `doPut()`, `doDelete()` 등의 메서드를 오버라이딩하여 요청을 처리

```java
package com;

import java.io.IOException;
import java.util.Date;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class SimpleController extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public SimpleController() {
        super();
    }

	// get 방식에 의한 자동 호출
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("클라이언트의 요청이 GET 방식");
		
		String type = request.getParameter("type");
		
		Object resultObj = null;
		if(type==null||type.equals("greetings")) {
			resultObj = "hello world";
		} else if (type.equals("date")) {
			resultObj = new Date();
		} else {
			resultObj = "유효하지않은 출력값입니다.";
		}
		
		request.setAttribute("result", resultObj);
		RequestDispatcher dis = request.getRequestDispatcher("/simpleView.jsp");
		dis.forward(request, response);
		
	}

  // post 방식에 의한 자동 호출
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response)
	}

}
```

---

### 3. 웹서버에 등록 및 매핑

web.xml 파일을 통한 매핑

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>WebServlet_1</display-name>
  <!-- /WebServlet_1 주소가 입력되면 실행될 홈 화면 -->
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <!-- simpleController 서블릿 호출되면 com.SimpleController 파일 실행 -->
  <servlet>
  	<servlet-name>simpleController</servlet-name>
  	<servlet-class>com.SimpleController</servlet-class>
  </servlet>
  <!-- /WebServlet_1/simple 주소가 입력되면 simpleController 서블릿 호출 -->
  <servlet-mapping>
  	<servlet-name>simpleController</servlet-name>
  	<url-pattern>/simple</url-pattern>
  </servlet-mapping>
  <!-- http://localhost:8090/WebServlet_1/simple -->
</web-app>
```

Java 어노테이션을 사용하여 웹 서버에 등록

```java
@WebServlet("/simple")
public class SimpleController extends HttpServlet {
  ...
}
```