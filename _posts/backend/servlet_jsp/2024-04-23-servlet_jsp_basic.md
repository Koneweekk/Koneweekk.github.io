---
title: Servelt과 JSP 개요
date: 2024-04-23 14:20:00 +09:00
categories: [Back-End, Servelt & JSP]
tags: [Back-End, Servelt, JSP, jsp]
---

## Ⅰ. Servlet

### 1. Servlet 이란?

> 자바로 작성된 웹 애플리케이션의 서버 측 컴포넌트

- 클라이언트의 요청을 처리하고 응답을 생성하는 자바 클래스
- 보통은 HttpServlet 클래스를 상속하여 개발
- HTTP 프로토콜을 통해 통신하며, HTTP 요청을 받아 처리하고, 그에 따른 HTTP 응답을 생성
- Servlet은 주로 비즈니스 로직을 처리하고, 데이터베이스와의 상호 작용을 수행
- 보통은 서블릿 컨테이너(예: Apache Tomcat)에서 실행

---

### 2. Servlet의 장점

플랫폼 독립성
- Java로 작성되었으며, Java Virtual Machine(JVM)에서 실행
- Servlet이 플랫폼 독립적이며, 모든 운영 체제에서 동작할 수 있다는 것을 의미

확장성
- Servlet은 Java의 객체 지향적 특성을 활용하여 구조화되어 있음
- 코드의 재사용성을 증가시키고, 유지보수를 용이하게 함

---

### 3. Servlet의 단점

학습 곡선
- Java 언어에 대한 이해가 필요합니다.
- 처음에는 학습 곡선이 다소 가팔라질 수 있음

HTTP 프로토콜 제약
- Servlet은 주로 HTTP 프로토콜을 다루기 위해 설계되었습니다
- 다른 프로토콜을 다루는 데는 적합하지 않을 수 있습니다.

UI적 한계
- 동적으로 데이터를 생성할 순 있어도 UI를 그리는데는 한계가 있음

---
<br>

## Ⅱ. JSP(Java Server Page)

### 1. JSP란?

> 서버 측에서 동적 웹 페이지를 생성하는 데 사용되는 Java 기반의 기술

- HTML 내에 Java 코드를 삽입하여 동적 콘텐츠를 생성
- Servlet과 함께 작동하여 웹 애플리케이션을 구축
- 이를 통해 웹 애플리케이션의 비즈니스 로직과 표현을 분리 가능

---

### 2. JSP의 장점

간편한 구현
- HTML에 Java 코드를 삽입하는 방식으로 작성기존의 HTML과 유사하게 구현
- 이는 개발자들이 비교적 쉽게 익히고 사용할 수 있다는 것을 의미

종합적인 웹 프레젠테이션 기능
- HTML, Java 코드, 그리고 표현 언어를 결합하여 종합적인 웹 프레젠테이션 기능을 제공합니다
- 동적으로 생성된 콘텐츠를 효과적으로 표시할 수 있다는 것을 의미

커스텀 태그 라이브러리
- JSP는 커스텀 태그 라이브러리를 사용하여 레이아웃 및 기능을 재사용 가능
- 이는 코드의 가독성을 향상시키고 유지보수를 용이하게 함

---

### 3. JSP의 단점

디자이너와 개발자 간의 협업
- Java 코드가 포함되어 있기 때문에, 디자이너와 개발자 간의 협업이 어려울 수 있음
- 디자인과 로직이 섞여 있어서 유지보수가 어려울 수 있음
  
코드와 디자인 분리의 어려움
- JSP는 Java 코드와 HTML이 혼합된 형태로 작성되기 때문에, 코드와 디자인을 분리하기 어려울 수 있음
- 이는 유지보수를 어렵게 만들 수 있음

성능
- JSP는 동적 콘텐츠를 생성하기 위해 Servlet으로 컴파일
- 이는 매번 요청 시마다 Servlet 코드로 변환되어야 하므로, 처리 시간이 더 길어질 수 있음

---
<br>

## Ⅲ. Servlet과 JSP를 함께 사용하는 이유

### 1. 구조적 분리

Servlet
- 컨트롤러(Controller) 역할
- 클라이언트의 요청을 받아들이고 비즈니스 로직을 처리

JSP
- 뷰(View) 역할을 하며
- 사용자에게 결과를 보여주는 역할

> 구조를 분리함으로써, 애플리케이션의 유지보수 및 확장성을 향상시킬 수 있다.

---

### 2. 역할 분담

Servlet
- 주로 비즈니스 로직이나 데이터 처리와 같은 백엔드 작업에 사용

JSP
- 주로 프론트엔드 측면에서 사용자 인터페이스를 디자인하고 표현

> 역할을 분리함으로써 개발자는 각각의 역할에 집중할 수 있다.