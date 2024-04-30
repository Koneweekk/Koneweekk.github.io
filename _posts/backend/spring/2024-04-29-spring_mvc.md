---
title: Spring - MVC
date: 2024-04-30 10:35:00 +09:00
categories: [Back-End, Spring]
tags: [Back-End, Java, Spring, MVC]
---

## Ⅰ. Spring MVC 개요

### 1. MVC 패턴이란??

> Model-View-Controller의 약자로, 소프트웨어 디자인 패턴 중 하나

Model
- 애플리케이션의 비즈니스 로직과 데이터를 담당
- 이 부분은 데이터를 처리하고 관리하는 역할
- Spring MVC에서는 주로 POJO(Plain Old Java Object)들이 이 부분을 대표

View
- 사용자에게 정보를 표시하고 사용자 입력을 처리하는 부분
- HTML, JSP, Thymeleaf 등과 같은 템플릿 엔진을 사용하여 사용자 인터페이스를 생성
  
Controller
- 모델과 뷰 사이의 중간 역할
- 사용자의 요청을 처리하고, 모델을 업데이트하고, 그 결과를 적절한 뷰로 전달
- Spring MVC에서는 주로 POJO 기반의 컨트롤러 클래스들이 이 역할을 수행

---

### 2. Spring MVC의 흐름

![Spring MVC의 흐름1](https://media.geeksforgeeks.org/wp-content/uploads/20231106150237/Spring-MVC-Framework-Control-flow-Diagram.png)

DispatcherServlet
- 클라이언트의 요청을 처음으로 받는 부분
- 이 서블릿은 웹 애플리케이션의 모든 요청을 중앙 집중적으로 처리
- DispatcherServlet이 실행되면 해당 IOC 컨테이너가 자동 생성(디스패처당 컨테이너 하나)
- [DispatcherServlet의 설정 이름]-servlet.xml 이 자동으로 설정파일로 인정 

HandlerMapping
- DispatcherServlet은 요청을 처리할 적절한 컨트롤러를 찾기 위해 HandlerMapping에게 요청을 전달
- HandlerMapping은 요청된 URL을 어떤 컨트롤러가 처리해야 하는지 결정
  
Controller 호출
- HandlerMapping은 적절한 컨트롤러를 찾으면 해당 컨트롤러를 호출
- 컨트롤러는 요청을 처리하고, 비즈니스 로직을 실행하며, 필요한 경우 데이터 모델을 준비
  
비즈니스 로직 처리
- 컨트롤러는 비즈니스 로직을 수행
- 이 단계에서는 데이터의 가공, 검증, 데이터베이스와의 상호 작용 등이 발생
  
Model 생성
- 컨트롤러는 결과 데이터를 모델 객체에 저장
- 이 모델 객체는 뷰에 데이터를 전달하는 데 사용
  
View 선택
- 컨트롤러는 클라이언트에게 보여줄 뷰를 선택
- 이것은 뷰의 논리적 이름이거나, 뷰 객체 자체가 될 수 있음
  
ViewResolver
- 뷰의 논리적 이름을 실제 뷰 객체로 변환하는 역할을 담당
- ViewResolver는 뷰의 논리적 이름을 물리적인 뷰 파일의 경로로 매핑
  
뷰 렌더링
- 선택된 뷰는 클라이언트에게 보여줄 HTML, XML 또는 다른 콘텐츠를 생성
- 이때 뷰는 모델 객체에 저장된 데이터를 사용하여 동적으로 콘텐츠를 생성 가능
  
HTTP 응답
- 생성된 콘텐츠는 DispatcherServlet에게 반환되어 클라이언트로 전송
-  DispatcherServlet은 HTTP 응답을 완성하고 클라이언트에게 전송

---
<br>

## Ⅱ. 

### 1.

